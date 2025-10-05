# **Security & Data Governance Strategy**

## **Addendum for Dev Schedule v1.1.2+**

**Purpose:** This document addresses the security architecture, data boundaries, and legal limitations of the AIA Client ecosystem. It specifically focuses on the challenges introduced by the bimodal engine's ability to parse web content that may be accessed via a user's authenticated session (i.e., behind a login).

### **1\. The Main Boundary: The "Irreducible Source" Principle**

The fundamental security and legal boundary of the entire system is the line between public information and private data. The AIA's generated content, even when sourced by a user, must adhere to this principle.

* **Definition:** The system is designed to restructure and synthesize **publicly accessible information**. It must never, under any circumstances, store, transmit, or reward the contribution of data that is private, personally identifiable, or accessible only through a user's individual authenticated session.  
* **AIA Content vs. "Irde" (Irreducible Data Element):**  
  * **AIA Content (Permitted):** A synthesized summary of a publicly available news article, the structured data from a public financial report, a list of store hours from a company's website.  
  * **Irreducible Private Data (Prohibited):** The contents of a user's private email, their account details on a subscription site, direct messages, or any data that is specific to *their* account and not accessible to the general public.  
* **Enforcement:** This boundary is enforced by a combination of client-side responsibility and server-side validation.

### **2\. The Login Bypass Challenge: Client-Side Masking & Sandboxing**

The "Direct Parse Mode" feature, where the client can parse data from any source the user can browse, is the most significant security consideration. The client-side application (main.go) is, by default, the primary guardian against data leakage.

* **Masked External Login Bypass:**  
  * The system does not "bypass" logins. Rather, it operates within the user's existing, authenticated browser session on their local machine. The security risk is that the client-side parsing script might inadvertently capture private session data (cookies, tokens, PII) along with the content.  
* **The Client-Side Mandate: Default Parameter Sandboxing:**  
  * Before any data is sent from the client to the /contribute-update API endpoint, the application **must** execute a rigorous sanitization and masking routine.  
  * **This routine will, by default, strip:**  
    1. All cookies and authentication tokens.  
    2. Script tags and executable code.  
    3. Known PII patterns (email addresses, phone numbers, names).  
    4. User-specific identifiers found in the HTML.  
  * The main.go client effectively acts as a sandbox, ensuring that only the core, unstructured content is sent to the server for restructuring, without the user's session context.

### **3\. Operational Session Flow: Security at Each Step**

The default parameters of a user session are designed with security checkpoints.

* **Browse (AIA Mode):** Inherently secure. The client only communicates with the trusted AWS API.  
* **Update (Direct Parse Mode):** This is the high-risk action.  
  1. User initiates a parse.  
  2. The client application performs the parse *locally*.  
  3. The client executes the mandatory **masking and sanitization routine**.  
  4. The now-anonymized content is sent to the AWS API.  
  5. The AWS backend performs a **second layer of validation**, rejecting any payload that still contains suspicious data patterns.  
* **Recycle (Data Storage):** The AWS backend operates on a **zero-trust model**. All contributed data is treated as public, anonymous, and untrusted. It is only stored after successful validation and is never directly associated with a user's private data, only with their public wallet address for rewards.  
* **Rewards:** The blockchain transaction confirms the *hash* of the anonymized data, not the data itself. This creates a verifiable but anonymous link between the work performed and the reward issued, preventing the raw contributed data from being permanently stored on-chain.

[Image of a network security firewall diagram](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcS78Skc1sbZmVJZPdQrQiA1BwucYB7o0FRjvvwEKy4bmG_zLqe3MC3Zc6Gf7ErH7vr6Lgd0h_MAJvgb81uuLh2WY22b7oObL28e_xgtEtCjtjC91v8)

### **4\. Estimated Legal & Network Security Limitations**

This architecture is designed to be legally defensible and secure, but it operates within clear limitations.

* **Legal Limitations:**  
  * The system's legal defense rests on **transformative use** of public data and a strict adherence to **data minimization**.  
  * The User Agreement must explicitly state that users are prohibited from contributing content that violates copyright (e.g., from behind a paywall) or the privacy of others.  
  * The platform's legal posture is that it is a tool for restructuring public information, and the responsibility for sourcing that information legally during a "Direct Parse" session lies with the user, though the app provides technical safeguards.  
* **Network Security Limitations:**  
  * The primary limitation is the **trust placed in the client application's integrity**. The entire model depends on the client's ability to properly sanitize data.  
  * **Mitigation:** The main.go application must be hardened against tampering. This will require security measures such as:  
    * **Code Signing:** To ensure the application has not been modified.  
    * **Runtime Integrity Checks:** To detect if the application is being manipulated while running.  
    * **Backend Heuristics:** The server must constantly evolve its heuristics to detect and block improperly sanitized data payloads, assuming that a compromised client could exist.
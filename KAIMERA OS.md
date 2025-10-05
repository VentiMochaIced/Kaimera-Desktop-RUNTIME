# **Project Plan: AIA Client \- Version 1.1.1+**

## **Chronological Ideation & Workflow for Go Development Team**

**Document Purpose:** This document outlines the operational parameters and chronological development schedule for the Go-based AIA Client application. It serves as a living workflow checkpoint, using the main.go prototype as the foundational starting point. The goal is to successfully evolve the conceptual UIX into a fully functional, scalable, and commercially viable product by version 1.1.1 and beyond.

### **Phase 1: Foundation & Architectural Proof of Concept (Current \-\> v0.2.1)**

**Goal:** Transition the static main.go UI mock-up into a dynamic, data-aware client shell. The primary objective of this phase is to validate the core architectural decision: a lean Go client that renders structured data from a separate, powerful backend.

* **Checkpoint v0.1.0: UI & Layout Finalization**  
  * **Parameter:** Finalize the Fyne UI layout based on main.go. Ensure all static components are in place.  
  * **Action:** Code review of the existing UI layout. Create a style guide for custom widgets.  
  * **Feedback Loop:** Internal team review for UI/UX consistency.  
* **Checkpoint v0.2.1: Local Data & Mocked API Integration (As per main.go comments)**  
  * **Parameter:** Implement REQ 1 \- a local DatabaseManager for SQLite.  
  * **Action:** Create a database package in Go. Implement functions to initialize the database and dynamically load/save user data (e.g., bookmarks). The bookmarksList will be populated from this database.  
  * **Parameter:** Implement REQ 2 & REQ 3 \- transition to an intent-driven model with mocked data.  
  * **Action:** Change the URL bar's placeholder text. On submission, the client will call a *mocked* API function. This function will return a hardcoded, structured JSON string. The main content area will be updated to parse this mock JSON and render a simple list of results.  
  * **Feedback Loop:** The primary output of this phase is a functional offline client. This proves the client can handle dynamic data rendering before any network code is written.

### **Phase 2: Dynamic Client & Live API Integration (v0.3.0 \-\> v0.5.0)**

**Goal:** Decouple the client from mock data and connect it to a live, albeit simple, server-side backend. This phase focuses on building a robust client-side data handling and rendering pipeline.

* **Checkpoint v0.3.0: Live API Communication**  
  * **Parameter:** Implement the client-side HTTP logic.  
  * **Action:** Create a network package. Implement functions to send the user's intent (from the URL bar) as a POST request to a development server endpoint. This must be done in a **goroutine** to prevent UI freezing.  
  * **Feedback Loop:** Verify that the client can successfully send a request and receive a JSON response from a live development server.  
* **Checkpoint v0.5.0: Dynamic Widget Rendering**  
  * **Parameter:** Design and implement a flexible rendering system.  
  * **Action:** Based on different JSON schemas returned by the server, the client must render different custom widgets (e.g., a "summary card," a "map view placeholder," an "audio player placeholder"). This moves beyond a simple list to a rich, dynamic view.  
  * **Feedback Loop:** Test the client against various JSON payloads to ensure it can render them correctly and gracefully handle unknown or malformed data.

### **Phase 3: Backend Development \- The AIA Refinery (v0.6.0 \-\> v0.9.0)**

**Goal:** Build the scalable, serverless backend on AWS. This phase runs in parallel with Phase 2 and is the core of the "ad-virtualizing" engine.

* **Checkpoint v0.7.0: Core Infrastructure & API Deployment**  
  * **Parameter:** Deploy the serverless stack.  
  * **Action:** Using AWS CDK or Terraform, define and deploy the initial infrastructure: API Gateway, AWS Lambda functions (written in Go), and a DynamoDB table for structured content.  
  * **Feedback Loop:** The client from Phase 2 can now connect to this live AWS endpoint.  
* **Checkpoint v0.9.0: First-Generation AIA Agent**  
  * **Parameter:** Implement the daily data refinery process.  
  * **Action:** Create a scheduled AWS Lambda or Fargate task. This task will fetch data from a small, predefined set of reliable public sources/APIs, parse the content, restructure it into the application's standard JSON format, and store it in DynamoDB.  
  * **Feedback Loop:** The dev team can verify daily that new, relevant content is appearing in the database and is successfully served to the client app.

### **Phase 4: Feature Expansion & Commercialization (v1.0.0 \-\> v1.1.1+)**

**Goal:** Launch the core product and begin iterating on the premium, hybrid internet features that provide commercial value.

* **Checkpoint v1.0.0: The Semantic Internet (Commercial Launch)**  
  * **Parameter:** A stable, performant client and a reliable backend serving high-quality, structured data from a broad set of sources.  
  * **Action:** Finalize the application for public distribution. Implement licensing/authentication for the $100 lifetime app. Begin marketing as a premium, "ad-virtualized" information tool.  
  * **Feedback Loop:** Gather feedback from the first cohort of paying users.  
* **Checkpoint v1.1.0: The Geo-Verse (Subscription Tier)**  
  * **Parameter:** Integrate geospatial data into the AIA Refinery.  
  * **Action:** The backend will begin integrating with premium mapping and location data APIs. The server will be able to process and synthesize location-aware queries. The main.go client will be updated with a new view container for rendering maps and location-based data.  
  * **Feedback Loop:** Beta test this feature with a select group of users to validate the value of the professional subscription tier.  
* **Checkpoint v1.1.1: The Multimodal Internet (Enterprise Tier)**  
  * **Parameter:** Introduce generative AI capabilities.  
  * **Action:** Integrate the backend with text-to-speech (TTS) and other generative AI services. Develop the logic for creating custom, multimodal content like the audio briefings.  
  * **Feedback Loop:** Work directly with initial enterprise clients to build custom, high-value generative workflows.

### **Maximal Summary: A Phased Strategy for a New Internet**

This chronological plan provides a structured, iterative path to realizing the full potential of the AIA Client concept. The workflow is designed to **de-risk the project** by proving out each architectural layer independently—from the offline UI (v0.2.1) to the live API client (v0.5.0) and the scalable backend (v0.9.0).

By separating the client's role (rendering clean data) from the server's role (processing messy content), we create a legally and technically robust system. This allows the team to deliver on the ultimate user promise: a high-performance, ad-free, and intelligent UIX that anticipates user intent. Each phase builds upon the last, culminating in a multi-tiered commercial product that is not just an application, but a pioneer in a new, more meaningful way of interacting with the internet.
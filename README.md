# **Structured Summary — Module 4: Testing and Automating API Tests**

# **1\. API Testing Mindset**

The transition from web interface testing to the service level (API) requires structural validation of contracts and data:

* **Absence of Graphical Interface:** Interaction occurs directly via HTTP protocol between client and server, focusing on *requests*, *responses*, *headers*, HTTP methods, and *status codes*.  
* **Atomic and Fragmented Tests:** Focus on isolating *endpoints* to validate business rules, in contrast to predominantly visual *End-to-End* (E2E) scenarios.  
* **Structural Validation:** Rigorous verification of JSON keys/attributes, data typing, and compliance with business rules.  
* **Authentication Management (*Stateless*):** Protected requests explicitly require sending an authorization token (e.g., Bearer JWT).  
* **Adoption of Shift-Left:** Ability to test the integration layer and validate contracts before user interface development.

# **2\. The VADER Heuristic Applied to the Banking Context**

Mnemonic guide for systematic creation of test scenarios in financial APIs:

* **V — Verbs (HTTP Verbs):**  
  * `POST`: Creation and execution of actions (e.g., `/login` and `/transfers`).  
  * `GET`: Safe and idempotent queries and reads (e.g., `/transfers`, `/accounts/{id}`).  
  * `PUT` and `PATCH`: Full and partial updates of resources.  
  * `DELETE`: Removal or cancellation with focus on *Safe Delete* (*Soft Delete*) to ensure compliance and accounting auditability.  
* **A — Authorization (Authorization and Security):**  
  * Route protection against anonymous access (401 Unauthorized) and insufficient permissions (403 Forbidden).  
  * Tests to prevent horizontal access flaws (*IDOR \- Insecure Direct Object Reference*).  
* **D — Data (Data and Contracts):**  
  * Validation of formats, data types, and payload limits (e.g., rejection of negative values, self-transfers, and missing required fields).  
* **E — Errors (Exception Handling):**  
  * Responses with clear, structured JSON messages (e.g., 404 Not Found, 400 Bad Request / 422 Unprocessable Entity), avoiding generic errors and information leakage.  
* **R — Responsiveness (Performance and Stability):**  
  * Response time monitoring (SLA), pagination in large listings, and concurrency control with *Rate Limiting* (429 Too Many Requests).

1. # **3\. Common Pitfalls in API Testing**

2. **Incorrect Use of HTTP Verbs:** Executing state mutations using the `GET` method.  
3. **Authentication/Authorization Flaws:** Allowing access without a valid token or broken isolation between accounts (IDOR).  
4. **Missing or Weak Data Validation:** Trusting the sent payload without validating types, limits, or malicious injections (SQLi/XSS).  
5. **Generic Error Messages:** Returning messages lacking clarity regarding the violated rule.  
6. **Incorrect Use of Status Codes:** Returning `200 OK` with an internal error payload or `500 Internal Server Error` for client-side failures.  
7. **Poor Performance and Lack of Pagination:** Degradation in unconstrained queries and concurrency without transactional locking.  
8. **Stack Trace Exposure:** Displaying internal architecture and server details in unhandled errors.

# **4\. Architecture and Configuration of the Automation Project**

## **Tech Stack (Node.js)**

* **Mocha:** *Test Runner* responsible for organizing blocks (`describe`, `it`) and lifecycle hooks (`before`, `beforeEach`).  
* **SuperTest:** HTTP client for sending requests and validating responses.  
* **Chai:** Assertion library for BDD style (`expect`).  
* **Mochawesome:** Generator for visual, graphical reports in HTML format.  
* **Dotenv:** Centralized environment variable management.

## **Best Practices and Implemented Patterns**

* **Environment Variables:** Removal of hardcoded URLs and data using `.env` (keeping credentials out of Git via `.gitignore` and providing `.env.example`).  
* **Helpers:** Centralization of repetitive routines (e.g., JWT token generation) applying the DRY principle.  
* **Hooks (`beforeEach`):** Isolation and context renewal for each individual test.  
* **Fixtures:** Decoupled storage of test data sets in static `.json` files (e.g., `transfers.json`).  
* **Response Body Inspection:** Validation of attributes, types, and values in objects and arrays returned by the API.  
* **Technical Documentation:** Creation of a detailed and professional `README.md` for GitHub presentation.


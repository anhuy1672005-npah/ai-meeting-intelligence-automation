# System Architecture

**AI Meeting Intelligence Automation** is designed as a modular, event-driven pipeline orchestrating data between multiple external APIs using a Multi-Agent LLM framework.

## 🏗 High-Level Architecture

The architecture is divided into four primary layers:

### 1. Event & Ingestion Layer
*   **Trigger:** The system is triggered asynchronously via an n8n Webhook when a meeting concludes.
*   **Data Source:** Integrates with the **Fireflies.ai API** to pull down raw audio transcripts, speaker separation data, and meeting metadata (time, participants, title).

### 2. Orchestration Layer (n8n)
*   Acts as the central nervous system of the project.
*   Handles API rate limits, conditional routing (e.g., waiting for manual approval via email), data sanitization via custom JavaScript nodes, and API authentication.

### 3. Cognitive Layer (Multi-Agent LLM System)
Instead of relying on a single prompt, the system utilizes a chained, multi-agent approach via **OpenAI (GPT-4 / GPT-4o-mini)**:
*   **Agent 1 (The Analyst):** Consumes the raw transcript and produces a structured `Meeting Analysis Core` (JSON). This acts as the single source of truth.
*   **Agent 2 (The Strategist):** Consumes the Core Analysis to architect a 7-slide executive presentation, mapping insights to specific layout coordinates.
*   **Agent 3 (The Writer):** Consumes the Core Analysis to draft a comprehensive, long-form business proposal and action plan.

### 4. Delivery & Storage Layer
*   **Google Sheets API:** Logs meeting execution status (e.g., Generated, Declined).
*   **Google Slides API:** Dynamically constructs the presentation deck via batchUpdate requests.
*   **Google Docs API:** Formats and writes the final long-form report.
*   **Gmail API:** Sends human-in-the-loop approval requests and final delivery emails with direct links to the generated assets.

## 🔄 Data Flow
`Webhook Event` -> `Fireflies API (Transcript)` -> `JavaScript Pre-processing` -> `LLM (Core Analysis)` -> `LLM Branches (Slides & Docs)` -> `Google Workspace APIs` -> `Email Notification`
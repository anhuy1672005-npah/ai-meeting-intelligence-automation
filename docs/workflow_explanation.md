# Workflow Logic & Explanation

This document breaks down the operational logic of the n8n automation workflow found in `workflow/ai-meeting-intelligence-automation.sanitized.json`.

## Phase 1: Initiation & Data Retrieval
1.  **Webhook / Google Sheets Trigger:** The workflow begins when a new meeting ID is received.
2.  **Fireflies.ai Node:** Uses the meeting ID to fetch the full conversation transcript and summary gist.
3.  **Data Parsing (JavaScript):** A custom code node extracts and normalizes speaker names, removing empty arrays and formatting the text for the LLM.

## Phase 2: Human-in-the-loop (Approval)
1.  **Gmail Node:** Sends an automated email to the project owner asking for approval to generate the outputs.
2.  **Conditional (If Node):** The workflow pauses. It only proceeds if the owner clicks "Approve". If declined, it logs the status in Google Sheets and terminates.

## Phase 3: The "Source of Truth" Generation
1.  **AI Agent - Meeting Analysis Core:** The raw transcript is sent to OpenAI. The AI strictly outputs a structured JSON object containing key themes, decisions, risks, and action items.
2.  **Parse Core Analysis (JavaScript):** Validates the JSON schema, ensures no hallucinated fields are present, and prepares the payload for downstream nodes.

## Phase 4: Parallel Content Generation
The workflow splits into two parallel tracks:

*   **Track A (Presentation):**
    1.  **AI Agent Slides Writer:** Transforms the core analysis into a 7-slide JSON structure.
    2.  **Build Slides Requests (JavaScript):** Translates the AI's JSON into Google Slides API `batchUpdate` requests (shapes, text boxes, colors).
    3.  **Google Slides API:** Creates a blank presentation and executes the batch update.

*   **Track B (Document):**
    1.  **AI Agent - Report Writer:** Expands the core analysis into professional business prose.
    2.  **Build Docs Requests (JavaScript):** Translates the text into Google Docs API formatting requests (Headings, bullet points).
    3.  **Google Docs API:** Creates the document and inserts the formatted text.

## Phase 5: Finalization
1.  **Google Drive API:** Updates file permissions so the generated Docs and Slides are accessible via link.
2.  **Google Sheets API:** Updates the meeting log status to "Generated".
3.  **Gmail Node:** Delivers the final email containing direct URLs to the newly created presentation and report.
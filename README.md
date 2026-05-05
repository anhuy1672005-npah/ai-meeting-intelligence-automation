# AI Meeting Intelligence Automation

[![Automated with n8n](https://img.shields.io/badge/Automated_with-n8n-FF6D5A?style=flat-square&logo=n8n)](https://n8n.io/)
[![Powered by OpenAI](https://img.shields.io/badge/Powered_by-OpenAI-412991?style=flat-square&logo=openai)](https://openai.com/)
[![Integrated with Google Workspace](https://img.shields.io/badge/Integrated_with-Google_Workspace-4285F4?style=flat-square&logo=google)](https://workspace.google.com/)

An end-to-end automation workflow that transforms raw, unstructured meeting transcripts into highly structured, executive-ready presentations and comprehensive business reports.

---

## 🛑 The Business Problem

In fast-paced corporate environments, manual processing of meeting notes is a significant bottleneck. Reading through hours of meeting transcripts to extract key decisions, action items, and strategic themes is inherently time-consuming and error-prone. 

Without a structured extraction process:
- Critical context and nuances are lost.
- Accountability becomes blurred due to poorly tracked action items.
- Teams spend hours formatting slides and reports instead of executing decisions.

## 💡 Solution & Architecture

This project solves the manual processing bottleneck by deploying a fully automated, multi-agent LLM pipeline. 

**How it works:**
1. **Trigger:** A Webhook listens for a "meeting completed" event.
2. **Data Ingestion:** The system calls the **Fireflies.ai API** to retrieve the raw meeting transcript and metadata.
3. **Core Analysis (AI Agent 1):** **GPT-4** parses the raw transcript and synthesizes a highly structured JSON dataset (The "Meeting Analysis Core"). This acts as the single source of truth.
4. **Content Generation (AI Agents 2 & 3):** Two specialized AI agents consume the JSON data to draft a long-form proposal report and a 7-slide executive presentation.
5. **Delivery:** The workflow utilizes **REST APIs** to dynamically push the generated JSON structures directly into **Google Docs** and **Google Slides**, organizing them neatly in Google Workspace.

## 🛠 Tech Stack

- **Workflow Orchestration:** [n8n](https://n8n.io/)
- **AI & NLP:** OpenAI API (GPT-4 / GPT-4o-mini)
- **Data Integration:** Fireflies.ai GraphQL/REST API
- **Destination APIs:** Google Docs API, Google Slides API, Google Drive API
- **Data Processing:** Advanced JSON data parsing, programmatic prompt engineering, and RESTful API architecture.

## 🚀 Business Impact

This automation shifts the focus from **data entry** to **data-driven execution**.

- **Raw Data to Structured Insights:** Converts messy, multi-speaker conversational data into a clean, analytical structure.
- **Executive Readiness:** Automatically generates professional slide decks outlining key themes, decision tensions, and prioritization frameworks.
- **Clear Accountability:** Extracts concrete Action Plans with inferred owners, strict deadlines, and defined success signals.
- **Operational Velocity:** Saves approximately 2-3 hours of manual administrative work per meeting, enabling immediate strategic follow-up.

---

## 📂 Repository Structure

- `workflow/`: Contains the `.json` file of the complete n8n automation workflow.
- `prompts/`: Contains the detailed System and User prompts engineered for the AI agents (Core Analysis, Slide Writer, Report Writer).
- `samples/`: View real output examples (PDF, PPTX, DOCX) generated entirely by this automation.
- `docs/`: Technical architecture and setup guides.

---
*This project was built as a demonstration of Data Automation, API Integration, and Applied AI/Prompt Engineering capabilities.*
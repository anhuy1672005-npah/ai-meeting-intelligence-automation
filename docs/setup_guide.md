# Setup & Deployment Guide

Follow these steps to deploy the AI Meeting Intelligence Automation on your local machine or cloud server using n8n.

## Prerequisites
*   A running instance of [n8n](https://n8n.io/) (Local, Docker, or Cloud).
*   An active **OpenAI API Key**.
*   An active **Fireflies.ai API Key**.
*   A **Google Cloud Console** project with the following APIs enabled:
    *   Google Docs API
    *   Google Slides API
    *   Google Sheets API
    *   Google Drive API
    *   Gmail API

## Step 1: Import the Workflow
1. Open your n8n workspace.
2. Click on **Workflows** -> **Add Workflow**.
3. In the top right corner, click the `...` menu and select **Import from File**.
4. Upload the `workflow/ai-meeting-intelligence-automation.sanitized.json` file from this repository.

## Step 2: Configure Credentials
You will notice several nodes have missing credentials. You must create and link the following in n8n:
1.  **Fireflies Api:** Enter your Fireflies Auth Token.
2.  **OpenAI API:** Enter your OpenAI API Key.
3.  **Google OAuth2:** Create a credential using your Google Cloud Client ID and Client Secret. Connect this single credential to the Sheets, Docs, Slides, Drive, and Gmail nodes.

## Step 3: Environment Variables
Ensure your system environment variables match the requirements. You can use the `.env.example` file located in the root directory as a template. Rename it to `.env` and fill in your secure keys if running n8n via Docker.

## Step 4: Map Your Google Sheets
1. Create a blank Google Sheet to act as your Meeting Log.
2. Ensure it has the following column headers: `Date/Time`, `Title`, `Attendees`, `Gist`, `Status`, `ID`.
3. Open the **Google Sheets Trigger**, **Append row in sheet**, and **Update row in sheet** nodes in n8n.
4. Update the Document ID and Sheet Name to match your newly created Google Sheet.

## Step 5: Activate and Test
1. Toggle the workflow to **Active** in the top right corner of n8n.
2. Trigger the workflow manually by passing a mock meeting ID or wait for a real webhook payload from Fireflies.
3. Check your email for the approval request and the final output links!
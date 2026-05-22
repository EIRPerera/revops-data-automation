# 📧 Automated Email Verification & HubSpot CRM Sync (Data Hygiene)

### 🎯 Overview
In Revenue Operations (RevOps), maintaining clean data is critical for email deliverability and accurate sales forecasting. This production-ready n8n workflow automates data hygiene by automatically triggering whenever a new lead email is added to a Google Sheet. It verifies the email's validity via the **Allegrow API** and instantly updates the status inside **HubSpot CRM**, ensuring sales reps only reach out to verified contacts.

### 🔄 Data Flow Architecture
1. **Trigger:** Monitors a central Google Sheet (`[Workflow] - Email verification`) for any newly added rows.
2. **Data Transformation:** A custom JavaScript Code node extracts and builds a clean data array of emails.
3. **Data Splitting:** Uses n8n's `Split Out` node to process multiple incoming emails efficiently.
4. **Enrichment/Verification:** Calls the Allegrow Validation API (`POST`) to check the deliverability of the email.
5. **CRM Update:** Issues a `PATCH` request to HubSpot CRM to update the custom property `allegrow_email_verification_status` for that specific contact.

### 🛠️ Tools & Nodes Used
* **Google Sheets Trigger Node:** Real-time polling for new rows.
* **JavaScript Code Node:** Custom data parsing and structuring.
* **Split Out Node:** Loops items seamlessly for bulk operations.
* **HTTP Request Nodes:** Core API integrations with Allegrow and HubSpot CRM Rest API (v3).

### 📸 Workflow Canvas
![Workflow Screenshot](./screenshot.png)

### 🚀 How to Replicate
1. Download the masked `workflow.json` from this folder.
2. Import it into your n8n instance.
3. **Important:** Replace the placeholder API keys in the HTTP nodes with your actual Allegrow API Key and HubSpot Private App Token.

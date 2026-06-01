# 🎯 High-Throughput LinkedIn Ads Library URL Fixer

### 🎯 Overview
In modern B2B Revenue Operations and competitive intelligence, analyzing a prospect’s live digital ad footprint is highly valuable for sales personalization. However, HubSpot CRM data is frequently unstructured—often saving standard LinkedIn company URLs rather than the unique numeric Organization IDs required to query the official **LinkedIn Ad Library**. 

This workflow solves this challenge at scale. It acts as an enterprise-grade automation engine designed to parse up to **30,000 corporate records per day** in batches of 100. It extracts the vanity handles from CRM links, translates them into structural internal IDs via the LinkedIn API, builds explicit Ad Library tracker links, and updates HubSpot asynchronously while natively enforcing API rate-limit protection.

### 🧠 System Architecture & High-Performance Logic
1. **Cron-Scheduled Batching:** A fine-tuned Cron Schedule Trigger executes the operation multiple times a day at high-velocity increments (`0 1,3,6,8,11,13,16,18,21,23 * * *`) to cleanly chunk the heavy data loads.
2. **Stateful Daily Initialization:** A specialized JavaScript execution node builds an immutable execution context inside n8n’s static workflow memory ($getWorkflowStaticData), generating dynamic temporal markers (`current_month`) to prevent re-processing lines within the same cycle.
3. **Cursor-Based Bulk Ingestion:** Instead of standard paginated API nodes, a raw JavaScript helper conducts continuous `POST` search requests directly into HubSpot’s `/crm/v3/objects/companies/search` endpoint. It handles deep token filtering and auto-advances the cursor string until the strict `DAILY_LIMIT` threshold is met.
4. **RegEx Vanity Extraction:** A high-speed JavaScript code snippet uses regular expressions to isolate the active company vanity string from complex URL payloads, gracefully managing `decodeURIComponent` anomalies.
5. **Fail-Safe API Translation & Sync:** For records possessing a clear vanity name, the system hits the official LinkedIn Organization API with automatic three-tier retries. If successful, it maps out the numeric LinkedIn Company ID and generates the target Ads Library search payload.
6. **Graceful Error Handling ("No-Vanity" Fallback):** Companies missing a standard vanity handle or yielding 404 API errors aren't dropped. The workflow redirects them to an isolated logic branch that writes a `no_vanity` or `not_found` sync status flag to HubSpot. This acts as a poison-pill pattern, ensuring these bad records aren't repeatedly picked up in tomorrow's pipeline run.
7. **Rate Safety Regulation:** Features an inline asynchronous wait delay wrapper (`LinkedIn Rate Safety Wait`) configured down to sub-second throttling thresholds, protecting structural API quotas across all synchronized accounts.

### 🛠️ Tools & Nodes Used
* **Advanced Schedule Trigger:** Implements complex cron-expressions for high-volume data spreading.
* **HubSpot Search Engine (JS Node):** Cursor-driven internal loops capable of fetching thousands of matching entries dynamically.
* **JavaScript Parsing Engine:** Executes URL parsing, RegEx extractions, and asynchronous payload formatting.
* **HTTP Request Nodes:** Handles bidirectional secure transactions with LinkedIn Restli protocol specifications and HubSpot CRM endpoints.
* **n8n Split in Batches Node:** Iterates over parsed elements systematically to drive continuous loop executions.

### 📸 Workflow Canvas
![Workflow Screenshot](./screenshot.png)

### 🚀 How to Replicate
1. Download the `workflow.json` file inside this repository folder.
2. Import the JSON payload into your active n8n instance.
3. Configure your HubSpot Private App Token inside the initialization code block.
4. Insert your verified LinkedIn OAuth App credentials within the Organization ID HTTP node headers.

# 🤖 AI-Powered ICP (Ideal Customer Profile) Category Classifier

### 🎯 Overview
In B2B Revenue Operations, maintaining accurate Ideal Customer Profile (ICP) categorization is essential for lead scoring, targeted marketing, and sales routing. However, CRM industry fields are often messy, outdated, or misaligned with actual business models. 

This workflow automates the **ICP Classification** of HubSpot companies using a sophisticated two-tier approach. It first evaluates the company's industry against deterministic rules (Red Flag, Non-Tech, Tech). If the initial data is inconclusive, it dynamically scrapes the company's live website and utilizes an **OpenAI LLM** to analyze the raw HTML content, definitively determining if the company is B2B and categorizing its true industry.

### 🧠 Intelligent Tiered Architecture
1. **Deterministic Rule Engine (Fast & Free):** A custom JavaScript node applies hardcoded Python-equivalent logic against a massive list of predefined industries, instantly flagging irrelevant B2C or heavily restricted categories to save API costs.
2. **Dynamic Web Scraping:** If a company requires deeper analysis, the workflow extracts the company domain from HubSpot and executes a direct HTTP `GET` request to scrape the live website front-page.
3. **Regex Content Cleaning:** A JavaScript code block strips heavy HTML, scripts, and CSS, sanitizing the payload to just the first 3000 characters of raw text for optimal token usage.
4. **LLM Enrichment:** Feeds the cleaned website context and CRM data to an OpenAI model (`gpt-5.4-mini`) with a strict System Prompt, forcing a structured JSON output (`is_b2b`, `confidence`, `reasoning`, `suggested_industry`).
5. **CRM Reconciliation:** Translates the AI's JSON output back into standardized CRM tiers (Tech, Non-Tech, Irrelevant) and securely patches the `icp_category` field in HubSpot.

### 🛠️ Tools & Nodes Used
* **HubSpot CRM API:** Bidirectional sync (GET properties, PATCH updates).
* **HTTP Web Scraper Node:** Raw web content extraction with built-in redirection handling.
* **JavaScript Code Nodes:** Regex-based HTML parsing, complex array mapping, and error-handling Try/Catch blocks.
* **n8n LangChain OpenAI Node:** Prompt engineering for strict JSON formatting and semantic analysis.
* **Split in Batches Node:** Throttles API requests to prevent rate limiting (25 records per batch).

### 📸 Workflow Canvas
![Workflow Screenshot](./screenshot.png)

### 🚀 How to Replicate
1. Download the `workflow.json` from this folder.
2. Import it into your n8n workspace.
3. Connect your HubSpot Private App Token and OpenAI API Key inside the n8n credentials manager.

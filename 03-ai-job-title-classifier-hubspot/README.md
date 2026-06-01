# 🤖 AI-Powered Job Title Classifier & HubSpot CRM Enrichment Pipeline

### 🎯 Overview
In Revenue Operations, unstructured data is the enemy of accurate lead scoring, territory routing, and targeted marketing segmentation. Sales reps often enter non-standardized job titles into the CRM (e.g., "Europe Growth" or "Strategic Partnerships Lead"). 

This advanced n8n production workflow builds a **hybrid automated pipeline** that standardizes incoming unstructured job titles into clear, predefined categories: **Job Seniority** (e.g., Director, Manager, CxO) and **Job Function** (e.g., Sales, RevOps, Marketing). It utilizes a high-efficiency caching layer first, falling back onto a **Large Language Model (OpenAI GPT)** only when necessary to control API costs.

### 🧠 Intelligent Routing Architecture (Cost Optimization)
To prevent unnecessary LLM spending, the workflow applies a smart multi-tiered architecture:
1. **Local Lookup Cache:** Checks an internal mapping sheet (`lookup_sheet`) to see if the job title has been classified before.
2. **Conditional Branching (If Node):** If cached data exists, it instantly issues a `PATCH` request to HubSpot CRM.
3. **AI Fallback Mechanism:** If the title is completely new, it dynamically prompts an LLM via the **n8n LangChain OpenAI Node** to classify the title under strict enterprise JSON constraints.
4. **Data Normalization:** A JavaScript node parses the stringified AI output smoothly, and pushes the enriched properties (`job_function2`, `seniority_new`) straight into HubSpot CRM.

### 🛠️ Tools & Nodes Used
* **Google Sheets Trigger & Nodes:** Real-time stream monitoring, data caching, and logging tracking.
* **n8n LangChain OpenAI Node:** Connects with foundational models (`gpt-mini` variant) using zero-shot system prompts to extract structured semantic data.
* **JavaScript Node:** Safe try-catch parsing for defensive data handling against malformed JSON strings.
* **HubSpot REST API Integration:** Real-time data enrichment executing granular contacts patching with built-in API rate limiting protection.

### 📸 Workflow Canvas
![Workflow Screenshot](./screenshot.png)

### 🚀 How to Replicate
1. Download the cleaned `workflow.json` from this folder.
2. Import it into your n8n environment.
3. Supply your own HubSpot CRM Private App Token and OpenAI API Key inside the respective credential managers.

# 🎯 Automated Intent Signal & Contact Discovery Engine (Apollo & HubSpot)

### 🎯 Overview
In modern B2B Go-To-Market (GTM) strategies, reacting instantly to account-level intent signals is a massive competitive advantage. However, once an account shows intent, Sales Development Representatives (SDRs/BDRs) often waste hours manually searching for the right decision-makers on LinkedIn or Apollo, delaying the speed-to-lead execution.

This workflow automates the entire prospect discovery and qualification process. Activated by incoming account intent webhooks, it dynamically queries the **Apollo API** to extract contacts matching specific criteria. It then runs an embedded algorithmic scoring matrix via JavaScript to rank and filter the top 10 decision-makers, maps structural graph associations inside **HubSpot CRM**, and automatically assigns action-oriented BDR outreach tasks or routes outliers to manual evaluation queues.

### 🧠 System Architecture & Algorithmic Scoring
1. **Dynamic Intent Ingestion:** The process initiates via an incoming `Webhook` mapping a target `hubspot_company_id` that triggered an account-level intent baseline.
2. **Multi-Stage Apollo Exploration:** The engine executes programmatic search queries into Apollo’s target contact discovery endpoints, fetching up to 100 potential contacts matching standard organization boundaries.
3. **Algorithmic Grading Logic:** A high-performance n8n JavaScript node ("Filter and Rank Contacts") acts as a discrete execution framework, processing each contact against a predefined scoring matrix:
   * **Seniority Tier Mapping:** Generates a foundational base score based on absolute hierarchical placement (`c_suite` / CMO = **+40 pts**, `vp` = **+30 pts**, `director` = **+20 pts**, All others = **+10 pts**).
   * **Job Function Hyper-Targeting:** Evaluates string arrays for high-intent operational keywords. Titles containing `'demand gen'`, `'abm'`, `'account based'`, or `'demand generation'` are awarded an additional **+15 points** bonus.
4. **Data Isolation & Slicing:** The algorithm sorts the cumulative arrays in a strict descending order and slices the execution payload to return only the **Top 10 highest-scoring contacts** per corporate account.
5. **CRM Writeback & Direct Association:** Contacts passing structural validation thresholds are created inside HubSpot. The workflow maps transactional matrix edges (`Associate Contacts to Company`) linking new records to the original target enterprise.
6. **Task Escalation Routing:** If the system discovers valid contacts, it automates a direct, pre-formatted outreach task for the assigned Business Development Representative (BDR). If the discovery payload returns empty, it creates an emergency `Manual Review Task` inside HubSpot to safeguard pipeline leakages.

### 🛠️ Tools & Nodes Used
* **Webhook & HTTP Request Nodes:** Consumes webhook alerts and drives authenticated REST API interactions with Apollo and HubSpot CRM.
* **JavaScript Ranking Engine (Code Node):** Executes real-time mathematical sorting, filtering, and data sanitization structures.
* **HubSpot Node (V2):** Orchestrates bulk lookups, object mutations, and operational tasks seamlessly.
* **Loop Over Items Nodes:** Iteratively pushes independent payload sequences into safe batch cycles without stacking instance memory.

### 📸 Workflow Canvas
![Workflow Screenshot](./screenshot.png)

### 🚀 How to Replicate
1. Download the `workflow.json` file inside this repository folder.
2. Import the JSON architecture into your active n8n instance.
3. Establish your HubSpot OAuth credentials and map your Apollo API Key to the HTTP Request fields.
4. Adjust the target scoring properties inside the `Filter and Rank Contacts` node to align with your specific Ideal Customer Profile (ICP).

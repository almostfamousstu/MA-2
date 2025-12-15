# **Gen Merch Flat Files Process Automation**

## 📄 Overview

As part of legacy GM offerings, Circana delivers approximately **130+ weekly and monthly frequencies** to its clients. 

The current process involves **multiple hand-offs and manual interventions**, which are time-consuming and prone to errors. Frequencies are delivered using **Circana proprietary tools** such as **ETL Job Control, Edit Manager, etc.**

---

## 🧭 Solution Objective

### **Core Workflow**
1.  **Job Initiation:**
    *   **User Submission:** A user submits a job via a **streamlit Web Form**.
    *   **Data Storage:** The job details are stored in a **Jobs** database.
    *   **Notification:** Simultaneously, a message is sent to **Power Automate**, which alerts a **Teams Channel** about the new submission.

2.  **Job Processing & Control:**
    *   **Python App Engine:** A central **Python App** acts as the orchestrator. it "Searches and Starts Jobs" by pulling from the database and updates job statuses as they progress.
    *   **Backend Support:** The app interacts with **GOATS** to enable event-driven triggers.

3.  **NAV Integration (The Execution Pipeline):**
    The Python App manages jobs through a series of RPA's (numbered steps 2 through 6) across various NAV components:
    *   **Edit Manager & Console:** The app interacts with the **NAV Edit Manager** (to define job rules and schedule times) and the **NAV Console**.
    *   **Validation:** The system performs automated checks, such as "Verify for 'No Data Found'" and "Verify for 'No Issues' for each hierarchy."
    *   **ETL & Execution:** The job reaches the **NAV Element Centre** and **NAV ETL Job Control**, where data input for job submission occurs.

4.  **Scheduling & Reporting:**
    *   **Schedule Management:** Based on validation results, the system can **Re/Schedule** jobs.
    *   **Final Actions:** Once a job is scheduled, the system updates the **Felix** power-app and updates the final "closeout" status.
    *   **Status Updates:** **Email Updates** are sent throughout the process, and final statuses are fed back to Power Automate and the primary Jobs database to complete the loop.

---

## 🧩 Architecture & Solution Design
### Technical Architecture

| Component | Technology |
|-----------|------------|
| **Backend Framework** | FastAPI 0.104.1 |
| **Web Server** | Uvicorn 0.24.0 |
| **Templating** | Jinja2 3.1.2 |
| **Data Validation** | Pydantic 2.5.0 |
| **Form Handling** | python-multipart 0.0.6 |

### Project Structure
```
gm_pos_tool/
├── config/           # Configuration files (YAML-based config system)
│   ├── config.yaml
│   └── loader.py
├── jobs/             # Job-related data/logic
├── src/              # Source code
│   ├── main.py       # Main FastAPI application (~31KB)
│   ├── pipeline.py   # Pipeline processing logic (~15KB)
│   └── nav/          # Navigation components
├── templates/        # Jinja2 HTML templates
├── index.html        # Landing page
├── requirements.txt  # Python dependencies
└── test_api.py       # API tests
```

### Solution Diagram
<img width="832" height="362" alt="image" src="https://github.com/user-attachments/assets/a83972cd-dabb-47a7-8807-3710b66c8289" />


## 🧰 Reference Documentation

* [LD Service API Docs](https://iriworldwide-my.sharepoint.com/:w:/r/personal/haresh_sheladiya_circana_com/Documents/Docs/LD%20Services%20Login%20API.docx?d=w88823dd1feb049179d823ff2f0a62d21&csf=1&web=1&e=mua3QQ)
* [GenMerch Resource Center](https://iriworldwide.sharepoint.com/sites/product)
* [Coding Standards & Review Process](https://github.com/almostfamousstu/mah-cookbook)

---

## 🔒 Security & Access

*   NAV Apps - Edit Manager, Element Center
*   [Rule Editor](https://webapp-nav-ruleeditor-prod.azurewebsites.net/#/home)
*   [GOATS](https://iriworldwide.sharepoint.com/sites/GlobalOpsProduction/SitePages/G.O.A.T.S.aspx)
*   [GenMerch Model Monitor](https://genmerch-model-monitor.decisionkey.npd.com/)
*   [Felix](http://felix.npd.com/#/login)
*   [GOATS Dashboard](https://iriworldwide.sharepoint.com/sites/GlobalOpsProduction/SitePages/G.O.A.T.S.-Dashboard.aspx)
*   [Master Delivery Calendar](https://iriworldwide.sharepoint.com/sites/GlobalOpsProduction/SitePages/MDS.aspx)
*   Access to library/backend systems which are used by the front-end tools
*   Access to API Interface available for the tools
*   Teams Channel ID to read messages
---
## ⚙️ Development Environment

The application runs locally using Uvicorn: 
```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Then access it at `http://localhost:8000`

---

## 🧪 Testing

Run all tests before submitting for review:

```bash
pytest tests/
```

Test coverage includes:

* DK connection validation
* Flat file schema conformity
* Delivery endpoint availability

---

## 🚀 Deployment / Delivery

Once validated, the automation will be scheduled or integrated into internal orchestration tools (e.g., Airflow, Control-M, or internal schedulers).
For the POC, execution will be manual via the CLI.

---

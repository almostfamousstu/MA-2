# **Decision-Key-Flat-File-MiAu**

## 📄 Overview

This repository serves as the **proof of concept (POC)** for the new internal Developer Portal initiative.
The goal of this POC is to demonstrate a streamlined onboarding and execution workflow for third-party developers within our standardized GitHub environment.

The contained solution automates the generation and delivery of a flat file using an internal reporting service’s prebuilt functions and API endpoints.

Developers can use this repository to:

* Quickly spin up a ready-to-use **Codespace** environment with all dependencies preinstalled.
* Review structured documentation outlining **requirements**, **constraints**, and **solution design**.
* Implement, test, and deliver a working automation with minimal setup friction.

---

## 🧭 Objective

The task is to **develop and validate a Python script** that:

1. Calls the internal reporting service via provided SDK or REST interface.
2. Retrieves the required dataset.
3. Generates a properly formatted flat file (e.g., CSV or pipe-delimited).
4. Delivers the file to the designated internal location (e.g., S3 bucket, shared drive, or secure FTP endpoint).

All service access details, sample payloads, and endpoint documentation are provided in the `/docs` directory.

---

## 🧩 Architecture & Solution Design

### Components

| Component                                   | Description                                                                             |
| ------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Reporting Service API / SDK**             | Provides prebuilt functions for data extraction.                                        |
| **Automation Script (`main.py`)**           | Implements extraction logic, flat file generation, and delivery.                        |
| **Config (`config.yaml`)**                  | Stores environment variables such as service endpoint, credentials, and delivery paths. |
| **Dev Container (`.devcontainer/` folder)** | Defines a portable development environment for GitHub Codespaces.                       |
| **Tests (`/tests`)**                        | Unit tests for validating the automation workflow.                                      |

### Flow

1. Developer launches a Codespace → container builds automatically using the provided `Dockerfile` and `devcontainer.json`.
2. The Python environment installs all dependencies from `requirements.txt`.
3. Developer configures `config.yaml` with valid credentials and parameters.
4. The `main.py` script is executed via `python main.py`.
5. Output file is validated and delivered to the target location.

---

## ⚙️ Development Environment

### Option 1: GitHub Codespaces (Recommended)

Click **“Code → Open with Codespaces”** to launch a preconfigured environment.

### Option 2: Local Setup

1. Clone the repository:

   ```bash
   git clone <repo-url>
   cd flat-file-automation-poc
   ```
2. Create and activate a virtual environment:

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # or venv\Scripts\activate on Windows
   ```
3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

---

## 🧪 Testing

Run all tests before submitting for review:

```bash
pytest tests/
```

Test coverage includes:

* API connection validation
* Flat file schema conformity
* Delivery endpoint availability

---

## 🚀 Deployment / Delivery

Once validated, the automation will be scheduled or integrated into internal orchestration tools (e.g., Airflow, Control-M, or internal schedulers).
For the POC, execution is manual via the Codespace or CLI.

---

## 🔒 Security & Access

* **Authentication:** Service credentials are stored as GitHub Secrets or in a local `.env` file (excluded via `.gitignore`).
* **Data Handling:** Generated flat files contain internal data; do not upload them to public repositories.
* **Access Control:** Only approved developers will have access to this repo through the internal Developer Portal.

---

## 🧭 Next Steps

Once validated, this repo will serve as the **template baseline** for future Developer Portal projects.
Subsequent solutions will follow this standardized structure for consistency, governance, and rapid onboarding.

---

## 🧰 Reference Documentation

* [Internal Reporting Service API Docs](./docs/reporting_service_api.md)
* [Developer Portal Guidelines](./docs/developer_portal_standards.md)
* [Coding Standards & Review Process](./docs/code_review_standards.md)

---

Would you like me to tailor this README into a **template version** (with placeholders and standardized markdown sections) that future project repos can copy from? That would make it easy to apply this structure to other internal solutions.

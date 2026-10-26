Smart move. Jumping straight into code is how "technical debt" is born. A **Functional Specification Document (FSD)** will serve as your blueprint, ensuring the app actually solves the "repetitive task" problem rather than just becoming another project to manage.

Here is a structured **Spec Document Template** tailored specifically for your BI Automation App.

---

# Specification Document: Project "BI-Flow" (Working Title)
**Author:** [Your Name]  
**Version:** 1.0  
**Objective:** To automate the end-to-end BI development lifecycle from requirement intake to report generation.

---

## 1. Problem Statement
The current BI development process is manual, highly repetitive, and prone to "translation errors" between business requirements and technical implementation. Significant time is spent on boilerplate tasks (writing standard DAX, mapping columns, setting up thresholds) rather than high-value analysis.

## 2. Target Workflow (The "App Modules")
The app will be divided into five logical modules:

### Module A: The Intake (Requirements & Signal Define)
*   **Input:** Natural language business requirements.
*   **Function:** Use an LLM to extract **Dimensions**, **Measures**, and **Thresholds**.
*   **Output:** A "Project Manifest" (JSON/YAML) defining the scope.

### Module B: The Explorer (Data Discovery)
*   **Input:** Database Metadata (Table/Column names from Information Schema).
*   **Function:** Fuzzy match business requirements from Module A to actual database assets.
*   **Output:** A suggested SQL Join path and Column Mapping.

### Module C: The Architect (Semantic Model Generator)
*   **Input:** Mapping from Module B.
*   **Function:** Generate the metadata for a BI tool (e.g., a Power BI `.pbip` or `model.bim` file). 
*   **Key Feature:** Auto-generate "Standard DAX" (Time Intelligence, YOY Growth) based on identified date columns.

### Module D: The Visualizer (Mock Report)
*   **Input:** Semantic Model.
*   **Function:** Create a wireframe (Streamlit charts or SVG) to show stakeholders the "Shape" of the data before the actual report is built.

### Module E: The Sentinel (Signal & Threshold)
*   **Input:** Defined thresholds from Module A.
*   **Function:** Generate a SQL/Python monitor that alerts if data quality or business logic drifts.

---

## 3. Technical Stack (Proposed)
*   **Frontend:** **Streamlit** (Python-based UI) for rapid development.
*   **Backend:** **Python** (FastAPI if needed, but Streamlit can handle most logic).
*   **AI Engine:** **OpenAI GPT-4o or Claude 3.5 Sonnet** (for SQL/DAX generation and mapping).
*   **Data Connection:** `SQLAlchemy` (to read warehouse schemas).
*   **BI Integration:** 
    *   **Power BI:** `PBIP` (text-based format) and `TOM` (Tabular Object Model).
    *   **Generic:** Outputting DBT Semantic Layer YAML.

---

## 4. User Persona & Use Case
*   **User:** BI Developer.
*   **Scenario:** 
    1.  User enters: *"I need a report for the Sales VP showing Daily Revenue vs. Last Year. Alert me if revenue drops by 20%."*
    2.  App identifies `fact_sales.amount` and `dim_date`.
    3.  App generates the DAX for `Total Revenue` and `Revenue LY`.
    4.  App creates a shell `.pbip` file.
    5.  App provides a SQL snippet for a Snowflake/BigQuery alert.

---

## 5. Success Metrics (MVP Goals)
*   **Speed:** Reduce the time from "Requirement" to "Semantic Model Shell" from 4 hours to 15 minutes.
*   **Consistency:** Standardize DAX naming conventions across all reports.
*   **Accuracy:** LLM-suggested mappings should require <20% manual correction.

---

## 6. Implementation Roadmap
*   **Phase 1 (MVP):** Build the **Intake** and **Semantic Model Generator** (The biggest time savers).
*   **Phase 2:** Integrate **Data Discovery** (Connecting to the actual DB).
*   **Phase 3:** Build the **Mock Report** generator.
*   **Phase 4:** Build the **Sentinel** alerting module.

---

### Next Steps: The "Core Logic" Decision
Before we build the UI, we need to decide on the **Output Format**. 

Since you are a BI developer, which tool do you use most?
1.  **Power BI?** (We will focus on generating `.pbip` or `.bim` files).
2.  **Tableau?** (We will focus on `.tds` files).
3.  **Looker?** (We will focus on `LookML`).
4.  **dbt?** (We will focus on `dbt Semantic Layer` YAML).

**Which one should be our primary target for Phase 1?**
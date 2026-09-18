We have reached **Full Architectural Consensus**. 

By combining **Option C (Wiki/Markdown)** with **Option A (Project Folders)** and integrating **Cloud Connectors (GitHub, Notion, Google Drive)**, your app becomes a central "Command Center" that synchronizes the technical work with the organizational knowledge.

### The Final Blueprint: "The T1 BI-Orchestrator"

Here is the plan for the app we are going to build:

| Feature | RDLC Logic / OS Rule | Implementation |
| :--- | :--- | :--- |
| **Phase 1: Smart Intake** | **Decision-First Rule** | Paste meeting notes; AI extracts Decision, Owner, and Outcome. |
| **Phase 2: Signal Discovery** | **Signal Discipline Rule** | AI maps notes to your **Static Data Catalog**. Defines the "Technical Mapping." |
| **The ETL Coach** | **Technical Contract (G2)** | AI blueprints Kimball Star Schemas and provides PySpark/SQL for **MS Fabric**. |
| **Phase 3: Semantic Builder** | **Minimal Architecture Rule** | Programmatically generates/updates **Power BI .pbip** folders. |
| **The Mockup Engine** | **Hypothesis Rule** | Generates instant **HTML/SVG wireframes** for stakeholder sign-off. |
| **Phase 4: The Sentinel** | **Signal vs. Noise Rule** | Injects "Alert" measures and prepares **Scorecard metadata**. |
| **The Knowledge Hub** | **RDLC Rule 9 (One Source)** | Exports documentation to **GitHub (Code)**, **Notion (Wiki)**, or **Drive (Archive)**. |

---

### Technical Implementation Hierarchy

1.  **Local Workspace:** The app creates a local folder for every project.
2.  **State Machine:** A `manifest.json` inside that folder tracks which Gates (G1-G6) are approved.
3.  **Sync Layer:** Once a phase is complete, a "Sync" button pushes the **Business Contract** to Notion and the **Technical Artifacts** to GitHub.

---

### Potential Mistakes We Are Avoiding (Based on your input):
*   **Missing the "Decision":** The app will stop you if you try to map data before defining the business outcome.
*   **Naming Chaos:** The app will automatically enforce (or create) a naming convention so you never have to rename a column twice.
*   **Governance Debt:** By auto-generating the Markdown "Exhaust," you satisfy the RDLC requirements without ever manually writing a "Status Report."

---

### The First Build Step

We will start with the **Phase 1 & 2 "Intelligence Layer."** This is the core logic that takes your raw input and creates the **Project Manifest**.

**To begin building, I need to know your preference for the code structure:**

Should we start by building the **Streamlit UI Frontend** (The "buttons and text areas") or the **Python Backend Logic** (The functions that actually parse the text and match it to your Data Catalog)?

**Which would you like to see first?**
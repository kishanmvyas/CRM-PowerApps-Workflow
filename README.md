<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/4/44/Microsoft_logo.svg" alt="Microsoft Logo" width="120" />
  <h1>Enterprise CRM Workflow Management</h1>
  <p><strong>A robust, end-to-end Customer Relationship Management solution built with Microsoft PowerApps & Power Automate.</strong></p>
  
  [![PowerApps](https://img.shields.io/badge/PowerApps-Microsoft-0078D4?style=for-the-badge&logo=microsoft)](https://powerapps.microsoft.com/)
  [![Power Automate](https://img.shields.io/badge/Power_Automate-Workflow-0078D4?style=for-the-badge&logo=microsoft)](https://powerautomate.microsoft.com/)
  [![Dataverse](https://img.shields.io/badge/Dataverse-Database-0078D4?style=for-the-badge&logo=microsoft)]()
</div>

<br />

![CRM Dashboard Overview](./assets/crm_dashboard_mockup.png)

## 📌 Project Overview
This repository contains the architecture, schema definitions, and exported workflows for a comprehensive **Enterprise CRM System** built entirely on the Microsoft Power Platform.

The solution was designed to replace legacy, manual spreadsheet-based lead tracking methods. By implementing a centralized Dataverse schema, a responsive Canvas PowerApp, and sophisticated logic via Power Automate, this system reduces manual data entry and enforces stringent sales processes.

### 🌟 Key Enhancements & Business Impact
* **Automated Lead Routing:** Decreased lead assignment delay by over 95% (from 24 hours to instantaneous).
* **Unified Dashboard:** Provides sales teams with a single pane of glass for all leads, opportunities, and accounts.
* **Data Integrity:** Eliminated duplicate CRM entries by leveraging Dataverse alternate keys and Power Automate validation flows.
* **Alerting & Notifications:** Custom Outlook and Teams notifications mapped to critical opportunity stage changes.

---

## 🏗️ System Architecture

The solution embraces a decoupled, scalable architecture utilizing Dataverse as the primary data layer.

```mermaid
graph TD
    %% Styling
    classDef ui fill:#0078D4,color:#fff,stroke:#005A9E,stroke-width:2px,rx:10px,ry:10px
    classDef logic fill:#006197,color:#fff,stroke:#004578,stroke-width:2px,rx:10px,ry:10px
    classDef data fill:#00A4EF,color:#fff,stroke:#0071C5,stroke-width:2px,rx:10px,ry:10px
    classDef external fill:#7FBA00,color:#fff,stroke:#5A8200,stroke-width:2px,rx:10px,ry:10px

    subgraph "Frontend / User Interface"
        A[PowerApps Canvas App<br/>(Sales Dashboard)]:::ui
        B[Model-Driven App<br/>(Admin Configuration)]:::ui
    end

    subgraph "Automation Layer (Power Automate)"
        C{Lead Routing Flow}:::logic
        D{Deal Won Notification}:::logic
        E{Weekly Summaries}:::logic
    end

    subgraph "Database Layer"
        F[(Dataverse)]:::data
        F_T1[Table: cr_Leads]:::data
        F_T2[Table: cr_Accounts]:::data
        F_T3[Table: cr_Opportunities]:::data
        F -.-> F_T1
        F -.-> F_T2
        F -.-> F_T3
    end
    
    subgraph "External Integrations"
        G[Microsoft Teams]:::external
        H[Office 365 Outlook]:::external
    end

    %% Relationships
    A -->|CRUD Operations| F
    B -->|Management| F
    
    F_T1 -->|On Create/Update Trigger| C
    F_T3 -->|On State Change Trigger| D
    
    C -->|Update Owner| F_T1
    C -->|Send Alert| H
    D -->|Post to Channel| G
```

---

## 📂 Repository Structure

The source files in this repository are structured to separate workflows, database schemas, and documentation.

```text
CRM-PowerApps-Workflow/
├── assets/                    # Screenshots and UI assets
│   └── crm_dashboard_mockup.png
├── src/
│   ├── workflows/             # Power Automate JSON Logic Exports
│   │   ├── lead_routing_flow.json
│   │   ├── opportunity_won_notification.json
│   │   └── data_cleanup_job.json
│   ├── dataverse_schema/      # Custom Dataverse Table JSON definitions
│   │   ├── accounts_table.json
│   │   ├── leads_table.json
│   │   └── opportunities_table.json
├── docs/                      # Extensive system documentation
├── CONTRIBUTING.md
└── README.md
```

---

## 🛠️ Data Model / Schema (Dataverse)
The core of the CRM relies on three primary custom Dataverse tables:

1. **`cr_Lead`**: Captures raw incoming inquiries.
2. **`cr_Account`**: Represents a validated business entity.
3. **`cr_Opportunity`**: Stores potential revenue-generating deals linked to Accounts.

*Explore the `src/dataverse_schema/` directory to view the detailed JSON schema representations, complete with Choice (Picklist) definitions and lookup relationships.*

---

## ⚙️ Core Workflows

### 1. Lead Routing Engine
* **Trigger:** When a row is added to the `cr_Lead` table.
* **Logic:** Evaluates the `cr_territory` picklist. If `North America`, automatically assigns the record to the NA-Sales Team ID. Otherwise, assigns to the Global Queue.
* **Action:** Sends an Office 365 Email to the assigned owner.

### 2. Opportunity Won Broadcaster
* **Trigger:** Status reason changes to "Won" on `cr_Opportunity`.
* **Logic:** Retrieves Account and Opportunity Revenue data.
* **Action:** Posts an Adaptive Card into the General "Sales Wins" Microsoft Teams channel celebrating the closed deal.

---

## 🚀 Setup & Deployment
To replicate this environment inside your own Power Platform Tenant:

1. Navigate to [Power Apps Maker Portal](https://make.powerapps.com).
2. Ensure you have the **System Customizer** or **Environment Admin** role.
3. Import the schemas located in `src/dataverse_schema/` to recreate the tables natively.
4. Set up unmanaged solutions and import the Flows from `src/workflows/` utilizing the native import wizard.
5. Grant necessary connection permissions to **Office 365 Outlook** and **Microsoft Teams** connectors.

## 🤝 Contributing
Feel free to open issues or submit Pull Requests for UI enhancements to the Canvas App concepts or additional automated flow logics.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

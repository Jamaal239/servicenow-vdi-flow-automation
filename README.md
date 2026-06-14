# servicenow-vdi-flow-automation 
├── images/
│   ├── 01_Flow_Properties.png
│   ├── 02_Blank_Flow_Canvas.png
│   ├── 03_Flow_Trigger.png
│   ├── 04_Get_Catalog_Variables.png
│   ├── 05_If_Admin_Condition.png
│   ├── 06_Ask_For_Approval.png
│   └── 07_Create_Catalog_Task.png
│
└── README.md   # ServiceNow Flow Designer Automation: VDI Access Fulfillment Flow

A comprehensive, automated backend workflow built within ServiceNow's Workflow Studio / Flow Designer. This solution digitizes, filters, routes, and creates human-in-the-loop tasks for corporate Virtual Desktop Infrastructure (VDI) provisioning.

---

## 🚀 Project Overview
Manual environment provisioning slows down workforce velocity and creates systemic compliance risks. This enterprise integration automates the lifecycle of a **Virtual Desktop (VDI) Access Request** catalog submission. 

By utilizing dynamic variable extraction and conditional branching paths, the flow guarantees that standard access configurations deploy smoothly, while elevated access profiles are halted until an implicit security approval is explicitly signed off.

### 🌟 Key Automation Mechanics
- **Event-Driven Execution:** Triggers instantly upon Service Catalog submission.
- **Dynamic Variable Contextualization:** Extracts user choices out of complex transactional structures to utilize them natively inside decision engines.
- **Risk-Based Branching:** Evaluates authorization vectors on-the-fly (`Standard` vs. `Administrator`).
- **Policy Enforcement:** Automates audit trail logging via systemic `Ask For Approval` tracking.
- **Unified Downstream Fulfillment:** Creates clean, scoped engineering work orders (`sc_task`) with deep variable visibility injected for operations teams.

---

## 📐 Architecture & Logic Topography

The workflow behaves as a strict, deterministic finite state machine represented as follows:

```text
[Service Catalog Trigger]
           │
           ▼
[Extract Form Variables] (business_justification, access_level)
           │
           ▼
 ⚖️ [Decision Branch: access_level == 'Administrator Access'?]
           │
           ├──► [YES] ──► 🛑 [Ask For Approval: System Admin] ──► (If Approved)
           │                                                               │
           └──► [NO] ───────────────────┐                                  │
                                        ▼                                  ▼
                            [Create Fulfillment Catalog Task] ◄────────────┘
                                        │
                                        ▼
                             [Close Loop / Active State]

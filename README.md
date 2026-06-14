# ServiceNow Flow Designer Lab: VDI Access Fulfillment Flow

## 📋 Lab Objectives
The objective of this lab is to configure an automated backend workflow within ServiceNow's Flow Designer (Workflow Studio) for a Service Catalog item named **Request Virtual Desktop (VDI) Access**. 

The automation must meet the following operational business logic requirements:
1. Trigger automatically upon a catalog item submission.
2. Extract user data variables seamlessly from the front-end catalog item form.
3. Evaluate the requested access level via conditional branching logic.
4. Route an approval request to the **System Administrator** if an elevated security profile (`Administrator Access`) is requested.
5. Generate a fulfillment work ticket (**Catalog Task**) automatically for the IT engineering team, ensuring form choices are visible to the processing technician.

---

## 🛠️ Complete Step-by-Step Lab Submission Documentation

### Step 1: Core Flow Properties Definition
Configure the baseline container properties for the automated engine. The flow is built in the **Global** application scope and set to run as the **System User** to prevent processing timeout failures or permission errors across restricted corporate data tables.

<img width="1920" height="945" alt="01_Flow_Properties" src="https://github.com/user-attachments/assets/300508fa-8b6a-4187-84d3-e617492d9108" />
*Figure 1: Initializing flow properties globally within the ServiceNow engine, setting execution permissions to run as System User.*

* **Application Scope:** Global
* **Run As:** System User
* **Verification Point:** Ensure the flow properties box is fully populated before initializing the workspace layout.

---

### Step 2: Initializing the Flow Workspace Canvas
Set up an empty, clean development staging canvas environment within Workflow Studio to map out sequential execution nodes sequentially.

<img width="1920" height="944" alt="02_Blank_Flow_Canvas" src="https://github.com/user-attachments/assets/aa1444bc-e543-48a5-92c9-d1763b5f6c0d" />
*Figure 2: The initialized development canvas layout, creating a clean visual workspace for compiling sequential actions.*

* **Verification Point:** Verify that the core workspace canvas is properly generated with no broken dependencies or orphaned blocks.

---

### Step 3: Service Catalog Core Trigger Configuration
Establish the structural entry criteria for the flow. The automation uses an event-driven model that monitors the Service Catalog table, launching the execution engine instantly when a user submits a portal form.

<img width="1920" height="942" alt="03_Flow_Trigger" src="https://github.com/user-attachments/assets/1be5bff8-c6fa-4ba7-856f-159ae7bd5610" />
*Figure 3: Mapping the foundational event trigger configuration to automatically evaluate incoming Service Catalog submissions.*

* **Trigger Type:** Service Catalog
* **Verification Point:** Confirm the block displays "Trigger - Service Catalog" on the canvas panel.

---

### Step 4: Contextual Form Variable Extraction
Query the submitted transactional payload using the `Get Catalog Variables` action to extract the flat questionnaire variables and transform them into reusable data pills for downstream flow logic.

<img width="1920" height="946" alt="04_Get_Catalog_Variables" src="https://github.com/user-attachments/assets/5736704e-3e64-4b74-ad9d-5c074aff0c3d" />
*Figure 4: Binding backend variables to pull context dynamically out of the 'Request Virtual Desktop (VDI) Access' catalog item.*

* **Action Name:** Get Catalog Variables
* **Template Catalog Item Selected:** Request Virtual Desktop (VDI) Access
* **Extracted Variables:** `business_justification` (String), `access_level` (Choice)
* **Verification Point:** Both variables must be moved successfully from the *Available* column into the *Selected* runtime output grid.

---

### Step 5: Conditional Branching Logic Implementation
Implement an `If` evaluation logic node to inspect the extracted data pills and dynamically check for high-risk access level configurations.

<img width="1920" height="942" alt="05_If_Admin_Condition" src="https://github.com/user-attachments/assets/ba8595e8-750c-41ac-8732-4881eeeed4f1" />
*Figure 5: Building conditional criteria to flag high-risk entries where the access level variable strictly evaluates to Administrator Access.*

* **Condition Title:** Check if Admin Access
* **Condition Logic:** `1 - Get Catalog Variables -> access_level` **is** `Administrator Access`
* **Verification Point:** Validate that the string matches the precise technical backend choice value for administrator access.

---

### Step 6: Human-in-the-Loop Security Approval Routing
Create an asynchronous `Ask For Approval` step within the conditional `If` sub-branch to stall provisioning and verify manager authorization for elevated user roles.

<img width="1920" height="950" alt="06_Ask_For_Approval" src="https://github.com/user-attachments/assets/f6a7b815-989e-4d83-8e12-4788c39ca096" />
*Figure 6: Routing approval requirements to the System Administrator with the rules criteria requiring an 'Anyone approves' logic gate.*

* **Action Name:** Ask For Approval
* **Record Target Mapping:** `Trigger -> Requested Item Record`
* **Table Target:** Requested Item `[sc_req_item]`
* **Approval Rules Matrix:** Approve When -> `Anyone approves`
* **Designated Approver User:** System Administrator
* **Verification Point:** Ensure the user icon silhouette button is utilized to input and pin "System Administrator" inside the rules panel field.

---

### Step 7: IT Fulfillment Work Order Generation
Construct the final action step outside the conditional sub-branch to automatically create a fulfillment ticket (`sc_task`) for the desktop support engineering queue.

<img width="1920" height="944" alt="07_Create_Catalog_Task" src="https://github.com/user-attachments/assets/d59099c4-3dfb-43d1-b24c-f40b16d8299f" />
*Figure 7: Generating the final Catalog Task assignment work order, passing structural item contexts directly to backend engineers.*

* **Action Name:** Create Catalog Task
* **Table Target:** Catalog Task `[sc_task]`
* **Requested Item Linkage:** `Trigger -> Requested Item Record` Data Pill
* **Short Description Text:** Configure VDI Environment for User
* **Template Catalog Item Linked:** Request Virtual Desktop (VDI) Access
* **Injected Task Variables:** `business_justification`, `access_level`
* **Verification Point:** Verify that variables are selected and pushed to the right side so that portal form details display inside the processing task view.

---

## 💡 Professional Insights: Corporate Application & Technical Key Takeaways

### What I Learned From This Lab
* **Dynamic Data Lifecycle Management:** Learned how to target complex transaction payloads using `Get Catalog Variables` and transform user form inputs into accessible, real-time "data pills" for downstream steps.
* **Risk-Aware Conditional Branching:** Mastered how to inject deterministic `If` evaluation logic blocks directly into a linear canvas layout to route operational flows based on security risk levels.
* **Asynchronous Process Interruption:** Configured human-in-the-loop dependencies using the `Ask For Approval` matrix, learning how ServiceNow safely pauses background workflow states until explicit manager authorization records are signed off.

### How This Applies to Enterprise Corporate Environments
* **Business Velocity & Operational Scale:** In a real corporate environment, manually setting up user profiles takes up hours of IT technician time. By automating standard requests to bypass the approval block and go straight to task creation, this flow minimizes provisioning times from hours to mere seconds.
* **Enforcing Strict Security & Compliance:** Elevated rights (like `Administrator Access`) are common targets for security audits. Forcing the workflow to route through the System Administrator ensures that no user can gain high-level privileges without an immutable audit trail being automatically logged.
* **Eliminating Communication Silos:** Pushing specific catalog form selections directly into the generated `sc_task` work ticket gives the assigned IT technician instant context. This removes the corporate overhead of engineers needing to ping users back-and-forth for business justifications or configuration profiles.

<p align="center">
  <img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket: Ticket Lifecycle Examples

This repository documents **end-to-end ticket lifecycle workflows** in osTicket, from ticket creation by end users to triage, escalation, assignment, and resolution by help desk staff.


---

## Access URLs

Use the following URLs throughout this exercise:

- **Admin / Analyst (Staff Control Panel):**  
  http://localhost/osTicket/scp/login.php

- **End User Portal:**  
  http://localhost/osTicket

---

## Lab Objective

In this section, we will:

- Create tickets as **end users**
- Observe and manage ticket properties as **help desk agents**
- Understand how **departments, SLAs, priorities, and assignments** affect ticket visibility
- Work tickets to completion under different roles
- Observe real-world ticketing behaviors and constraints

This simulation is designed to focus on **process and workflow**, rather than just clicking buttons.

---

## Initial Configuration/Setup: Create Required Departments (Admin Panel)

The ticket scenarios in this lab assume the following departments exist:

- **SysAdmins** (used for escalation and access-control behavior)
- **Online Banking** (used in Ticket Scenario 1)
- **Support** (used in Ticket Scenarios 2 and 3)
- **Maintenance** (will be deleted as part of cleanup)

If your osTicket instance does not already have these departments, create them first using the steps below.

---

### 1) Log into the Admin Panel

1. Go to: http://localhost/osTicket/scp/login.php
2. Log in with the admin account John (created during installation/setup)

<img width="711" height="522" alt="image" src="https://github.com/user-attachments/assets/771f3089-e37b-4218-8b8e-8d8137c6ae12" />

3. In the top navigation, click **Admin Panel** (if you are not already in it)

<img width="1193" height="432" alt="image" src="https://github.com/user-attachments/assets/7eb9ed95-2359-4a9d-8477-77c551f382d7" />


---

### 2) Navigate to Departments

1. In the Admin Panel, click the **Agents** tab (top navigation)
2. Click **Departments**
<img width="1192" height="542" alt="image" src="https://github.com/user-attachments/assets/f9afe735-576f-41f5-92af-b8402c75eb69" />

You should now see the Departments list.
<img width="1194" height="409" alt="image" src="https://github.com/user-attachments/assets/66dd058e-56fb-4abb-bb16-f3160bb03356" />

---

### 3) Create the Required Departments

You will repeat the same process for each department.

#### Click path to add a department
1. On the **Departments** page, click **Add New Department**
2. Fill in the **Name**
3. (If prompted) Choose the department type / settings as defaults unless your lab requires otherwise
4. Click **Create Dept** (or **Add Department**, depending on your version)

Create the following departments (one at a time):

- `SysAdmins`
- `Online Banking`
- `Support`

> Tip: If your osTicket version shows a "Type" option, leave it as the default.  
> The critical requirement for this lab is simply that these departments exist by name.

---

### 4) Promote SysAdmins to a Top-Level Department

Now that `SysAdmins` exists, we will make it a **Top-Level Department** (i.e., not a child of another department).

1. In **Admin Panel → Agents → Departments**
2. Click on **SysAdmins** (department name) to edit it
3. Locate the **Parent Department** (or similar) setting
4. Set it to **Top-Level** / **No Parent** / **—** (wording depends on version)
5. Click **Save Changes** (or **Save Dept**)

This step matters because it affects hierarchy and access behavior during escalation.

---

### 5) Delete the Maintenance Department (Do Not Archive)

This lab requires **deleting** the Maintenance department entirely (not archiving it).

1. In **Admin Panel → Agents → Departments**
2. Click **Maintenance**
3. Choose **Delete** (not Archive)
4. Confirm the deletion when prompted
<img width="1196" height="565" alt="image" src="https://github.com/user-attachments/assets/fbe5e57f-f4e8-4f13-9eb2-fbda585f9df8" />


> If you only see "Archive" from the list view, click into the department first—many versions expose "Delete" from inside the department settings page.

---

### 6) Confirm Department List Before Proceeding

Before starting Ticket Scenario 1, confirm that the Departments list includes:

- SysAdmins (Top-Level)
- Online Banking
- Support

And that:

- Maintenance is deleted
<img width="1193" height="533" alt="image" src="https://github.com/user-attachments/assets/91be35e6-2bde-4987-81a3-08efa8a87022" />


Once this is confirmed, proceed to the **Agents and Department Access** section.

---

## Pre-Lab Setup: Agents and Department Access

The ticket lifecycle scenarios in this lab require multiple help desk agents with different department access levels.

During the osTicket installation process (Part 1), an **initial administrator account** is created. In this lab, that account is named **john**. Because this account already exists and has administrative access, we will **reuse it** rather than recreate it.

We will:
- Adjust **department access** for the existing `john` account
- Create an additional agent (`jane`) to demonstrate escalation and role-based access control

---

### Agents Used in This Lab

- **john** — Initial administrator account (created during installation)  
- **jane** — Additional agent used for escalation and ticket resolution  

---

## Step-by-Step: Configure Agents in osTicket

### 1) Log into the Admin Panel

1. Navigate to:  
   `http://localhost/osTicket/scp/login.php`
2. Log in using the **john** account
3. Click **Admin Panel** in the top navigation

---

### 2) Navigate to Agents

1. In the Admin Panel, click the **Agents** tab
2. Click **Agents** from the sub-menu

You should now see a list of existing agents, for now only **john**.
<img width="1196" height="432" alt="image" src="https://github.com/user-attachments/assets/a88e9b2f-91e3-4e16-9e2b-d2a2bf6c0565" />

---

## Configure Existing Agent: john

The `john` account was created during installation and already has administrative access. In this lab, we will **limit department access intentionally** to demonstrate escalation behavior later.

### 3) Assign Department Access for john

1. Click **john** in the Agents list
2. Locate the **Department Access** section
3. Ensure access is granted to:
   - `Support`
4. Ensure **SysAdmins** is **not** assigned at this stage
5. Save changes

> John should not have access to the SysAdmins department yet.  
> This restriction is required to demonstrate loss of access when tickets are escalated.

---

## Create New Agent: jane

The `jane` account will be used to work escalated tickets and demonstrate role-based access control.

### 4) Create Agent jane

1. From the **Agents** page, click **Add New Agent**
2. Fill in the following fields:

**Account Information**
- Name: `Jane Doe`
- Username: `jane`
- Email: (any valid test email)
- Password: (set a password for the lab)

**Access**
- Primary Department: `SysAdmins`
- Role: `Agent` or `Admin` (depending on your osTicket version)

3. Click **Create** or **Add Agent**

---

### 5) Assign Department Access for jane

1. Click **jane** in the Agents list
2. Confirm access to:
   - `SysAdmins`
   - (Optional) `Support`
3. Save changes

Jane will be used to resolve escalated tickets that john cannot modify.

---

## Validation Before Proceeding

Before starting Ticket Scenario 1, confirm the following:

- **john**
  - Can view and work tickets in `Support`
  - Loses modification access when tickets escalate to `SysAdmins`

- **jane**
  - Can access the `SysAdmins` department
  - Can work escalated tickets to completion
  <img width="1196" height="462" alt="image" src="https://github.com/user-attachments/assets/49f7521b-9a4c-4d49-a13c-756c53c4746b" />


Once these conditions are met, proceed to the ticket lifecycle scenarios.



---

## Ticket Scenario 1 — Critical Outage (SEV-A)

### End User Action

As an **end user**, create the following ticket:

> **Subject:** Entire mobile/online banking system is down

Submit the ticket through the **End User Portal**.

<img width="1050" height="497" alt="image" src="https://github.com/user-attachments/assets/23ae8da9-a734-4fd0-af81-e8fddbe0ab61" />

<img width="1048" height="842" alt="image" src="https://github.com/user-attachments/assets/1f5eb5d6-96ce-41a9-bed6-d0d18c6451a0" />



---

### Help Desk Agent Review (john)

Log in as **Help Desk Agent (john)** and observe the ticket’s properties:

- Priority
- Department
- SLA
- Assigned To

Do **not** change anything yet—only observe.

---

### Set Ticket Properties

Update the ticket with the following settings:

- **SLA:** Sev-A (1 hour, 24/7)
- **Department:** Online Banking

<img width="1194" height="758" alt="image" src="https://github.com/user-attachments/assets/5b8cf81d-c71f-4a67-9a6f-62733f977d87" />


---

### Access Validation

Attempt to observe the ticket again as **john**.

- Can you still view the ticket?
- Can you modify the ticket?

Take note of the behavior.

---

### Resolution

Work the ticket to completion as **jane**.

---

## Ticket Scenario 2 — Software Request (SEV-B)

### End User Action

As an **end user**, create the following ticket:

> **Subject:** Accounting department needs Adobe upgrade, broken

<img width="992" height="796" alt="image" src="https://github.com/user-attachments/assets/a87e1a28-c67a-498a-94ce-b3d9840d7864" />

---

### Help Desk Agent Review (john)

Log in as **john** and observe:

- Priority
- Department
- SLA
- Assigned To

---

### Set Ticket Properties

Update the ticket:

- **SLA:** Sev-B (4 hours, 24/7)
- **Department:** Support

<img width="1180" height="560" alt="image" src="https://github.com/user-attachments/assets/311a6dc0-5040-4966-9694-084c5ff43446" />

---

### Resolution

Work the ticket to completion as **john**.

---

## Ticket Scenario 3 — Executive Hardware Issue (SEV-B)

### End User Action

As an **end user**, create the following ticket:

> **Subject:** CFO’s laptop will no longer turn on

<img width="1040" height="833" alt="image" src="https://github.com/user-attachments/assets/35bbd645-bb3e-441a-a231-4b698a928d11" />

---

### Help Desk Agent Review (john)

Observe the ticket properties:

- Priority
- Department
- SLA
- Assigned To

---

### Set Ticket Properties

Update the ticket:

- **SLA:** Sev-B (4 hours, 24/7)
- **Department:** Support

<img width="1183" height="570" alt="image" src="https://github.com/user-attachments/assets/23d3f8f4-a47a-4f20-b420-c6318f059882" />

---

### Resolution

Work the ticket to completion as **john**.

---

## Escalation and Permission Behavior

### Escalate All Tickets

Set **all tickets** to:

- **SLA:** Sev-A  
- **Department:** SysAdmins (last)

Observe that the tickets become **inaccessible** to standard agents.

---

### Restore Visibility

1. Switch to the **Admin Panel**
2. Assign yourself **View access** to the **SysAdmins** department
3. Switch back to the **Agent Panel**

Observe:

- You can now **see** the escalated ticket
- You can **no longer modify** it

This demonstrates role-based access control in osTicket.

---

## Final Resolution

Solve all remaining tickets and confirm they are closed properly.

---

## Real-World Ticketing Concepts

### Email Notifications

In most ticketing systems (including osTicket):

- Every ticket update triggers an **email notification**
- End users receive updates automatically
- Users can often **reply directly to emails**, which updates the ticket

This creates a continuous communication loop.

---

### Ticket Intake in Real Life

Tickets may originate from:

- Phone calls
- Chat applications
- Email
- Web forms
- Hallway conversations or desk walk-ups

While it is acceptable to fix issues on the spot, **best practice is to create a ticket for everything you do**.

Why this matters:

- Metrics
- Accountability
- Workload tracking
- SLA reporting

If it wasn’t ticketed, it effectively **didn’t happen**.

---

## Professional Takeaways

This lab demonstrates practical experience with **enterprise-style ticketing workflows**, rather than isolated technical tasks.

By repeatedly creating, triaging, escalating, and resolving tickets, this project highlights how structured ticketing systems support real-world IT operations, including:

- Incident prioritization and SLA enforcement
- Department-based ownership and role separation
- Controlled escalation paths and access boundaries
- Consistent communication between technical staff and end users

These workflows closely mirror how tickets are handled in production IT environments.

---

## Operational Relevance

In real IT environments, issues do not always originate from a web form. Requests may arrive via phone calls, chat tools, email, or informal walk-ups.

This lab reinforces a core operational principle:

**If work is not documented in a ticketing system, it cannot be tracked, measured, or improved.**

By intentionally creating tickets for all work performed—regardless of intake method—organizations gain:

- Accurate workload and performance metrics
- SLA accountability
- Clear prioritization of business-impacting incidents
- Improved long-term service planning

This project demonstrates an understanding of **why ticket discipline matters**, not just how to use the software.

---

## Skills Demonstrated

This project showcases hands-on experience with:

- End-user support workflows
- Help desk triage and prioritization
- SLA-driven incident response
- Role-based access control and escalation
- Agent versus administrator responsibilities
- Documentation-first operational practices

These skills are directly applicable to entry-level and junior roles in IT support, help desk operations, and systems administration.

---

## Continued Practice and Growth

Ticketing systems are foundational tools in IT operations, and proficiency improves through repetition.

Repeating this lab builds intuition around prioritization, escalation, and access control. Further exploration of email integration, automation, and reporting features would deepen operational effectiveness and readiness for real-world environments.

---

## Final Outcome

This project demonstrates readiness for professional IT environments by combining:

- Technical execution
- Process awareness
- Security-conscious behavior
- Clear and structured documentation

Together, these elements reflect the ability to function effectively within structured IT support teams and adapt to real-world operational demands.

This concludes the osTicket project series.

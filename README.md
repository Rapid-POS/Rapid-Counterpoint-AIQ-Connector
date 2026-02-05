# Rapid POS AIQ Connector - Version 1.0
Updated 2/5/2026

---

## Overview

The Rapid AIQ Connector automatically syncs customer and sales data from Counterpoint to AIQ to support targeted email and SMS marketing. AIQ specializes in regulated industries, including firearms. The connector syncs customer personas and transaction information for customers with a populated **Email Address 1** in Counterpoint.

If configured, **Phone 1** or **Mobile Phone 1** can be included to support AIQ SMS marketing.

---

## Minimum System Requirements

- Minimum Counterpoint version: **8.5.6.2**  
- Minimum SQL Server version: **2016**  
- Minimum Windows Server version: **2016**  
- Minimum PowerShell version: **5.1**  

If you would like the AIQ connector but your system does not meet these minimum requirements, please consult your Care Team Lead (vCIO) for an upgrade quote.

---

## Table of Contents

- [Minimum System Requirements](#minimum-system-requirements)
- [Section 1: AIQ Customer Records](#section-1-AIQ-customer-records)
- [Section 2: AIQ Configuration](#section-2-AIQ-configuration)
- [Section 3: AIQ Field Mapping – Customers Up](#section-3-AIQ-field-mapping--customers-up)
- 
- [Section 4: AIQ Custom Properties – Documents Up](#section-5-AIQ-custom-properties--documents-up)
- [Section 6: AIQ Custom Events](#section-6-AIQ-custom-events)
- [Section 7: AIQ Profile Custom Properties](#section-7-AIQ-profile-custom-properties)
- [Section 8: Queued to Be Sent to AIQ](#section-8-queued-to-be-sent-to-AIQ)
- [Section 9: AIQ Customer Status View](#section-9-AIQ-customer-status-view)
- [Section 10: Run AIQ Connector Button](#section-10-run-AIQ-connector-button)
- [Section 11: Mark All AIQ Messages as Read](#section-11-mark-all-AIQ-messages-as-read)
- [Section 12: AIQ Connector Execution and Sync Timing](#section-12-AIQ-connector-execution-and-sync-timing)
- [Section 13: Customer Profile Sync Logic and Workflow](#section-13-customer-profile-sync-logic-and-workflow)
- [Section 14: Managing Customer Email and Phone Updates](#section-14-managing-customer-email-and-phone-updates)
- [Conclusion](#conclusion)

---

## SECTION 1: AIQ Customer Records

The AIQ Connector adds an **AIQ Customers** button within Counterpoint, providing access to AIQ-specific customer fields directly from the Counterpoint customer record.

![Customer Record - AIQ Customers Button](./images/counterpoint-customer-record-AIQ-customers-button.png)

The email address on the AIQ customer record is populated from **Email Address 1** on the Counterpoint customer record.

Depending on configuration, the phone number is populated from either **Phone 1** or **Mobile Phone 1** on the Counterpoint customer record, **only when it meets the following criteria**:
- Contains **exactly 10 numeric digits**
- Does **not** include letters 

If the configured phone number field contains more than or fewer than 10 digits, or if it includes letters, the number will **not** be pushed to AIQ.

![AIQ Customer Record](./images/counterpoint-AIQ-customer-record.png)

### Accessing AIQ Customer Records

All AIQ customer records can also be accessed from:

**Connectors > AIQ > AIQ Customers**

This view allows records to be displayed in **table view**, where filters can be applied to review customers based on their current sync status.

![AIQ Customers in Table View](./images/counterpoint-AIQ-customers-table-view.png)

### AIQ Sync Status Codes

Each AIQ customer record includes a sync status value indicating its current state in the sync process:

- **0** – Fully synced; nothing pending  
- **1** – Recently created or updated; will sync on the next connector run  
- **2** – Customer is currently in the active sync queue  
- **5** – Invalid email address  
- **6** – Invalid phone number  
- **9** – Sync error; requires remediation before it can be re-synced  

---

## SECTION 2: AIQ Item Records

The AIQ Connector syncs item and inventory record data from Counterpoint to AIQ to support ticket data and product-specific marketing initiatives. 

![AIQ Item Record](./images/counterpoint-AIQ-item-record.png)

### Accessing AIQ Item Records

All AIQ item records can be accessed from:

**Connectors > AIQ > AIQ Items**

This view allows records to be displayed in **table view**, where filters can be applied to review items based on their current sync status.

![AIQ Items in Table View](./images/counterpoint-AIQ-items-table-view.png)

### AIQ Sync Status Codes

Each AIQ item record includes a sync status value indicating its current state in the sync process:

- **0** – Fully synced; nothing pending  
- **1** – Recently created or updated; will sync on the next connector run  
- **2** – Item is currently in the active sync queue  
- **9** – Sync error; requires remediation before it can be re-synced  

---

## SECTION 3: AIQ Configuration

The AIQ Connector includes a user interface for managing configuration options that control how the connector interacts with AIQ and Counterpoint.  

![AIQ Configuration](./images/counterpoint-AIQ-configuration.png)

For clients who use **multiple AIQ accounts**, a separate configuration record will exist for each account.

### Account Name, Account UID, URL, API Key, and API Revision
- Identifies the AIQ account and its associated credentials.
- Especially important for companies with more than one AIQ account (for example, separate accounts for a retail store and a restaurant).

### Is Enabled?
- Used to temporarily disable the connector while troubleshooting or testing.

### Connector Version, Last Maintained By, Last Maintained
- Displays the current connector version and the most recent maintenance information for reference.

### Workgroup
- Workgroup **234** will be created and used when inserting new customer records from AIQ into Counterpoint.
- The associated Customer Template for this workgroup is applied during customer creation.

### Mail Group ID
- [INSERT INFORMATION HERE]

### Auto Create AIQ Persona
Controls when AIQ customer records are automatically created in Counterpoint.

- **EMAIL ONLY**  
  A AIQ customer record is automatically created when a customer is added to Counterpoint with **Email Address 1**.  
  - If the configured phone number (**Mobile Phone 1** or **Phone 1**) is also present and valid, it will be included on the AIQ persona.

- **NO**  
  AIQ customer records must be created manually.

### Phone # for AIQ
Defines which Counterpoint Customer Record phone number field is used to populate the AIQ **Phone Number** persona property.
- Supported options:
  - **Mobile Phone 1**
  - **Phone 1**
- If the selected phone field does not meet the _exactly 10 numeric digits_ requirement, the phone number will not be sent to AIQ

### Last Customer Sync Date (UTC)
- Displays the most recent date a customer record was successfully synced to AIQ.

### Max Customers to Sync
- Used to optimize connector performance.
- Defines the maximum number of customers that can be synced in a single connector run.

### Documents Up Queue Batch Size
- Used to optimize connector performance.
- Defines the number of documents (tickets) to sync in a single connector run.

### Documents Up Start Date
- Used to limit the furthest historical date from which documents will sync.
- Also enables syncing of previously posted (historical) tickets to AIQ during installation of the connector.

### Documents Up Look Back Days
- Each connector run checks for documents that have not yet synced, going back the specified number of days.
- Ensures documents sync after a power outage or temporary connector interruption.
- Regardless of this setting, the **Documents Up Start Date** is always respected as the absolute limit.

### Max Queue to Sync
- [INSERT INFORMATION]

### Retain Queue Sync
- [INSERT INFORMATION]

### Other Configuration Options
Additional configuration fields exist for internal use by Rapid programmers. These options are used to optimize performance or assist with troubleshooting and should not be modified by end users.

---

## SECTION 4: AIQ Account Store Mapping

[INSERT INFORMATION HERE]

---

## SECTION 5: AIQ Field Mapping Customers Up

The **AIQ Field Mapping Customers Up** screen provides a user interface for managing which customer fields are sent from Counterpoint up to AIQ.

This table defines how customer record data in Counterpoint maps to AIQ persona properties. The standard deployment includes a predefined set of fields that are automatically synced. Adjustments to this table should generally be performed by a programmer.

Note: This is best viewed in _table view_.

![AIQ Field Mapping Customers Up in Table View](./images/counterpoint-AIQ-field-mapping-customers-up-table-view.png)

Calculated fields are not included by default. Any request to add calculated fields must be reviewed and quoted separately by Rapid.

**Note:** **Email Address 1** is a required field and must be sent to AIQ.

### Standard Customer Record Fields Sent to AIQ

The following customer fields are included in a standard AIQ connector deployment:

1. Email Address 1  
2. Mobile Phone 1
3. Customer Number
4. First Name  
5. Last Name  
6. Address 1  
7. Address 2  
8. City  
9. State  
10. Zip Code  
11. Country  
12. A/R Balance  
13. Customer Category  
14. First Sale Date  
15. Last Sale Date  
16. Loyalty Point Balance  

Note: The AIQ Connector currently supports **customers up** only. Customer records can be pushed from Counterpoint to AIQ, but customers cannot be downloaded (imported) from AIQ into Counterpoint.

---

## SECTION 6: AIQ Field Mapping Items Up

The **AIQ Field Mapping Items Up** screen provides a user interface for managing which item and inventory fields are sent from Counterpoint up to AIQ.

This table defines how item record data in Counterpoint maps to AIQ product fields. The standard deployment includes a predefined set of fields that are automatically synced. Adjustments to this table should generally be performed by a programmer.

Note: This is best viewed in _table view_.

![AIQ Field Mapping Items Up in Table View](./images/counterpoint-AIQ-field-mapping-items-up-table-view.png)

Calculated fields are not included by default. Any request to add calculated fields must be reviewed and quoted separately by Rapid.

### Standard Item Record Fields Sent to AIQ

The following item and inventory fields are included in a standard AIQ connector deployment:

1. Item Number
2. Description
3. Long Description
4. Item Type  
5. Price 1 
6. Regular Price
7. Stocking Unit
8. Category Code
9. Vendor Number 
10. Barcode 
11. Quantity  
12. Last Maintained Date

---

## SECTION 7: AIQ Field Mapping Documents Up

The **AIQ Field Mapping Documents Up** screen provides a user interface for managing which document (ticket) fields are sent from Counterpoint up to AIQ.

This table defines how customer record data in Counterpoint maps to AIQ persona properties. The standard deployment includes a predefined set of fields that are automatically synced. Adjustments to this table should generally be performed by a programmer.

Note: This is best viewed in _table view_.

![AIQ Field Mapping Documents Up in Table View](./images/counterpoint-AIQ-field-mapping-documents-up-table-view.png)

Calculated fields are not included by default. Any request to add calculated fields must be reviewed and quoted separately by Rapid.

### Standard Document Fields Sent to AIQ

The following document fields are included in a standard AIQ connector deployment:

#### Ticket Header Data
1. Document ID
2. Store ID
3. Business Date
4. Customer Number
5. Discount Amount
6. Total

#### Ticket Line Data
1. Item Numbers
2. Descriptions
3. Prices
4. Category Codes
5. Subcategory Codes
6. Vendor Numbers
7. Weights
8. Quantities Sold

---

## SECTION 8: AIQ Customer Status View

Each AIQ customer record includes a **sync status** that indicates its current state in the connector process. In some cases, it is helpful to review how many customer records fall into a particular status category.

For example, you may want to identify that **43 customers have an invalid email address (status 5)** so those records can be reviewed and corrected.

The **AIQ Customer Status View** displays a summary table showing:
- Each sync status code (0, 1, 2, 5, 6, 9)
- The total number of customer records currently associated with that status

**Notes:**
- If no customer records exist for a given status, that status will **not** appear in the table.
- The table can be refreshed at any time to display the most up-to-date information.
- This is best viewed in _table view_.

![AIQ Customer Status View](./images/AIQ-customer-status-view.png)

For details on the meaning of each customer sync status value, refer back to **SECTION 1: AIQ Customer Records**.

---

## SECTION 9: AIQ Item Status View

Each AIQ item record includes a **sync status** that indicates its current state in the connector process. In some cases, it is helpful to review how many item records fall into a particular status category.

For example, you may want to identify that **43 items have encountered an error (status 9)** so those records can be reviewed and corrected.

The **AIQ Item Status View** displays a summary table showing:
- Each sync status code (0, 1, 2, 9)
- The total number of item records currently associated with that status

**Notes:**
- If no item records exist for a given status, that status will **not** appear in the table.
- The table can be refreshed at any time to display the most up-to-date information.
- This is best viewed in _table view_.

![AIQ Item Status View](./images/AIQ-item-status-view.png)

For details on the meaning of each item sync status value, refer back to **SECTION 2: AIQ Item Records**.

---

## Section 10: AIQ Documents Queue

When sending document data to AIQ, each ticket is first placed into a queue in Counterpoint. Tickets are then synced from this queue to AIQ during connector runs.

![AIQ Document Queue](./images/AIQ-document-queue.png)

### Queue Status Values

Each document in the queue includes a status value indicating its current state in the sync process:

- **0** – Document has already been synced to AIQ; nothing pending  
- **1** – Document has been recently created or updated and will be added to the queue on the next connector run  
- **2** – Document is currently in the active sync queue  
- **9** – Document encountered an error and requires remediation before it can be re-synced

---

## Section 11: AIQ Items Quantity on Hand View



## SECTION 10: Run AIQ Connector Button

The **Run AIQ Connector** menu option allows authorized users to manually trigger the AIQ Connector when needed. Manual execution is typically used for testing or troubleshooting and is not required during normal operation.

### How Manual Execution Works

When the **Run AIQ Connector** menu option is selected:

- A **Manual Run Connector** action flag is set in the AIQ configuration.
- The flag functions as a **one-time execution request** and remains enabled until it is processed by the connector.
- Execution is handled in the background on the server (not on the workstation) to prevent overlapping executions.

### Background Processing and Scheduling

A background process periodically checks for the **Manual Run Connector** action flag based on a configurable **CRON schedule** stored in the AIQ configuration.

- The **Manual Run Connector Execution Time** schedule can be configured from the **AIQ Configuration** screen.
- When the action flag is detected:
  - If the AIQ connector is **not currently running**, it will execute for **all configured AIQ accounts**, typically within one minute.
  - If the connector **is already running**, the system waits for the current execution to complete, then automatically restarts the connector for all configured AIQ accounts.

In both scenarios, the action flag is **automatically cleared** when execution begins.

**Important:** Manual execution is intended primarily for **programmer-led testing or troubleshooting**, often when the connector has been **temporarily disabled**. It is not designed for routine operational use, as the connector runs automatically according to its configured schedule.

---

## SECTION 11: Mark All AIQ Messages as Read

The **Mark All AIQ Messages as Read** menu option allows users to suppress repeated pop-up alerts in Counterpoint while retaining all AIQ connector messages for later review.

This is especially useful in scenarios such as:
- Repeated error messages following a temporary internet outage
- High-volume alert conditions that have already been reviewed or acknowledged

Marking messages as read stops the pop-up notifications but does **not** delete the messages. All connector messages remain accessible in Counterpoint and can be reviewed at any time.

---

## SECTION 12: AIQ Connector Execution and Sync Timing

The AIQ Connector operates as a **Windows Service**, automatically syncing customer profiles and transactional documents between Counterpoint and AIQ.

The connector runs continuously in the background and is responsible for keeping both systems aligned while respecting AIQ API rate limits and configured sync rules.

### Sync Intervals

The connector processes different types of data on separate schedules:

- **Customer Profiles**  
  New and updated customer profiles are synced every **15 minutes**.

- **Documents in the Queue**  
  Transactional documents are synced every **1 minute**.  
  This interval is configurable and may be adjusted to prevent AIQ rate limiting.

If a document being synced contains a **new customer**, the customer profile is created in AIQ **immediately as part of the document sync**. The connector does not wait for the next 15-minute customer profile sync cycle.

For details on how customer profile changes are evaluated and synchronized between AIQ and Counterpoint, refer to **SECTION 13: Customer Profile Sync Logic and Workflow**.

---

## SECTION 13: Customer Profile Sync Logic and Workflow

This section describes the logical order and decision-making process used by the connector after a sync cycle begins.

The AIQ Connector processes customer profile updates in a defined sequence to ensure that the most recent and authoritative data is preserved between Counterpoint and AIQ.

### Step 1: Sync Changes from AIQ Down to Counterpoint

The connector first retrieves profile changes made in AIQ and evaluates whether those changes should be applied to Counterpoint.

- The connector compares the **date and time** of the most recent profile change in AIQ to the **date and time** of the most recent update in Counterpoint.
- The system uses the values from the source with the **most recent timestamp**.

Behavior depends on configuration settings:

- If **Insert/Update Customers** is **enabled**, all configured fields are synced down from AIQ to Counterpoint.
- If **Insert/Update Customers** is **disabled**, only **subscription status changes** are synced down.

### Step 2: Sync Changes from Counterpoint Up to AIQ

After processing inbound changes, the connector identifies customer records in Counterpoint that have been **created or modified** since the previous sync.

These updates are then pushed up to AIQ, ensuring that AIQ profiles reflect the most current customer information stored in Counterpoint.

---

## SECTION 14: Managing Customer Email and Phone Updates

When a customer is synced to AIQ, the connector stores the associated **AIQ Profile ID** on the customer record in Counterpoint. This Profile ID becomes the permanent link between the Counterpoint customer and the AIQ profile and is used for all future updates.  

Using the Profile ID ensures that customer history, engagement data, events, and flow activity are preserved in AIQ even when identifying information changes.

### Updating Email Address and Phone Number

If **Email Address 1** or the configured phone number (**Mobile Phone 1** or **Phone 1**) is updated in Counterpoint for a customer who already has a AIQ profile:

- The connector updates the email address or phone number on the **existing AIQ Profile ID**.
- A new AIQ profile is **not** created.
- The customer retains their full AIQ history, including events, metrics, and flow participation.

This behavior ensures continuity in AIQ while allowing customer contact information to be updated over time.

### Handling Duplicate Customer Records

The AIQ Connector enforces strict rules to prevent **duplicate AIQ profiles** and to maintain data integrity. Because AIQ profiles are uniquely identified by email address (per AIQ account), a single email address can only be associated with **one** Counterpoint customer record for that account.

The following scenarios describe how the connector behaves.

#### Scenario 1: Duplicate Email Addresses Already Exist in Counterpoint During Initial Setup

If the connector is installed and **multiple Counterpoint customers already share the same Email Address 1**:

- The connector creates or associates **one** AIQ profile for that email address.
- Only one Counterpoint customer record can be linked to that AIQ profile.
- Any additional Counterpoint customers using the same email address will **not** be able to create or associate their own AIQ customer record for that email address.

This behavior is expected and prevents duplicate AIQ profiles from being created during initial deployment.

#### Scenario 2: A AIQ Customer Record Already Exists and the Same Email Is Assigned to Another Counterpoint Customer

If a AIQ customer record already exists in Counterpoint for a given email address, and a user attempts to assign that **same Email Address 1** to a different Counterpoint customer record (either by editing an existing customer or creating a new one):

- Counterpoint blocks the action.
- An error is returned to the user.
- The connector does **not** allow a second Counterpoint customer to be linked to the same AIQ profile.

This prevents multiple Counterpoint customer records from sharing a single AIQ profile.

### Handling Merged Customers in Counterpoint

When two customer records are merged in Counterpoint:

- The AIQ customer record associated with the **“To”** customer (the retained record) remains linked to the AIQ profile.
- If the **“From”** customer had an associated AIQ customer record, that record becomes detached from any active customer.

It is recommended to **manually delete** the detached AIQ customer record after the merge. Otherwise, it will remain in Counterpoint with no functional association to an active AIQ profile.

---

## Conclusion

The Rapid AIQ Connector streamlines the exchange of customer profiles and transactional data between Counterpoint and AIQ, enabling powerful email and SMS marketing, accurate segmentation, and automated flows.

Before go-live, review configuration settings, field mappings, and list configurations to ensure customer data and subscription preferences are handled correctly. After deployment, monitor customer and document sync status views to identify invalid data or records requiring remediation.

For assistance with configuration changes, custom field mapping, event setup, or troubleshooting, contact Rapid Support.  

  

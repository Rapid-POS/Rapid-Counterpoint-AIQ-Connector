# AIQ Connector Video

The video is posted on Rapid's YouTube channel: [**Rapid POS: AIQ Connector Overview**](https://www.youtube.com/watch?v=XXXXXXXXXXXXX)   

---

Below is a quick outline of the video along with timestamps for easy reference:

#### 1. Overview of Rapid's AIQ Connector & Sample Customer Sync (0:00 – 2:20)
- Introduction to the Rapid AIQ Connector
- How the connector syncs Counterpoint customer and transaction data to AIQ
- Enables AIQ email and SMS marketing using POS sales data
- Review a sample customer record in Counterpoint
- Review the same customer after syncing to AIQ

#### 2. Sample Document (Ticket) Sync (2:21 – 6:55)
- Review a posted ticket synced to AIQ
- Examine **Extended Price** (line price × quantity)
- Discuss the difference between **extended price totals and ticket totals**
- Review item fields synced with ticket history:
  - Category
  - Subcategory
  - Brand
- Review **Shopping Habits** and other insights calculated by AIQ

#### 3. AIQ Consent Information (6:56 – 7:52)
- Overview of consent settings managed within AIQ
  - Loyalty membership status
  - Email marketing consent
  - Text message marketing consent
  - Age-gating and regulatory controls
- Clarify that consent is managed within AIQ, **not** by the connector

#### 4. Deeper Dive on Sample Documents (Tickets) (7:53 – 14:37)
- Extended price vs ticket totals
- Line-level discounts vs document-level discounts
- Transaction date and time details
- Reviewing multiple transactions for the same customer
- First and Last Name vs Company Name

#### 5. Creating a New AIQ Customer in Counterpoint (14:38 – 18:21)
- Create a new customer using **Counterpoint Touchscreen**
- Demonstrate the automatic creation of the AIQ customer record 
- Sync Status Overview
  - 0 – Fully synced; nothing pending
  - 1 – Recently created or updated; will sync on the next connector run
  - 2 – Currently in the active sync queue
  - 9 – Sync error; requires remediation before it can be re-synced
- Demonstrate how Email and Phone values are populated based on configuration settings

#### 6. AIQ Connector Configuration & Tools (18:22 – 21:53)
- Open **AIQ Configuration** in Counterpoint and review key settings
- Open **AIQ Account Store Mapping** in Counterpoint
- Explanation of connector tools:
  - **Run AIQ Connector**
  - **Mark All AIQ Messages as Read**
  
#### 7. Viewing AIQ Customer Records and Item Records (21:54 – 23:37)
- Review AIQ Customer Records in Table View
- Review AIQ Item Records in Table View

#### 8. Counterpoint to AIQ Field Mapping (23:38 – 26:02)
- Review **Customers Up** field mapping
- Review **Items Up** field mapping
- Review **Documents Up** field mapping
- Discuss opportunities and limitations for custom field mapping

#### 9. AIQ Monitoring Tools (26:03 – 29:08)
- Review **Customer Status View**
- Review **Item Status View**
- Review the **Documents Queue**
- Review the **Quantity on Hand View** for the `CRM_AIQ` location group
- Demonstrate how filters and table view can be used to identify sync issues

#### 10. Conclusion (29:09 – 33:04)
- Example of manual AIQ Customer record creation (`Auto Create AIQ Persona` = **No**)
- Deleting an AIQ Customer record
- Controlling which customers are synced to AIQ based on the presence of an AIQ Customer record
- Recap of AIQ connector functionality
- Where to find documentation and support

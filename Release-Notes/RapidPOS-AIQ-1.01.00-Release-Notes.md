# Rapid POS AIQ Connector v1.01.00 Release Notes  

_Release Date: May 4, 2026_

---

## New Functionality

- Added **Use Fast Endpoints Until** configuration setting to control which AIQ API endpoints are used when syncing customers, items, and documents.
  - Defines a **date value** that controls which AIQ API endpoints are used by the connector.
    - When the current date is **prior to this value**, the connector uses **high-performance (undocumented) endpoints** to temporarily accelerate initial data syncing.
    - When the current date is **on or after this value**, the connector automatically switches to **standard AIQ endpoints**, which follow AIQ’s normal rate limits.
  - This setting is **read-only** and is managed by Rapid programmers in coordination with AIQ when elevated rate limits are permitted.
 
## Bug Fixes and Performance Enhancements

This release focuses on performance improvements, scalability, and stability of queue processing within the AIQ Connector.

- Improved overall syncing performance, resulting in faster and more efficient processing of customer, item, and document data.
  - Improved queue processing performance by implementing **table-valued parameters (TVPs)**, enabling more efficient batch handling and reduced execution time.

- Enhanced queue processing to better handle large batches of data, improving reliability and reducing processing time.
  - Refactored logic in the following stored procedures to replace cursor-based processing with TVP-driven execution:
    - `USER_SP_AIQ_QUEUE_CRON`
    - `USER_SP_AIQ_QUEUE_AFTER_PS_DOC_POST_RUN`
  - Updated `USER_SP_AIQ_QUEUE_ONE_DOCUMENT` to:
    - Accept TVPs as parameters
    - Utilize TVPs within dynamic SQL joins for improved performance
    - Resolve issues related to dynamic SQL execution

- Updated internal processing logic to eliminate inefficiencies, leading to more consistent and scalable performance.
  - Replaced cursor-based logic with TVP-driven processing in merge-related procedures:
    - `USER_SP_BEFORE_ITEM_MERGE_AIQ`
    - `USER_SP_BEFORE_CUSTOMER_MERGE_AIQ`

- Improved handling of item and customer merges in Counterpoint, ensuring accurate data syncing and reducing the risk of duplicate or missed records.

- Standardized how ticket history is processed, improving consistency and maintainability of synced data.
  - Standardized how valid ticket history records are passed to `USER_SP_AIQ_QUEUE_ONE_DOCUMENT`, improving consistency and maintainability.

- Fixed issues related to data processing and system stability, including:
  - Prevention of duplicate records during processing
  - Improved handling of background processing errors
  - More accurate tracking of document processing status
  - Resolved a primary key violation issue by preventing duplicate inserts into `@VALUES`, including adding a cleanup step during calculated column query processing.
  - Corrected logic for determining `QUEUE_DOC_ITERATION`, ensuring accurate behavior when triggered by merge-related stored procedures.

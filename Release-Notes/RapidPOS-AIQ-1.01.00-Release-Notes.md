# Rapid POS AIQ Connector v1.01.00 Release Notes  

_Release Date: May 4, 2026_

---

## New Functionality

- Added **Use Fast Endpoints Until** configuration setting to control which AIQ API endpoints are used when syncing customers, items, and documents.
  - Defines a **date value** that controls which AIQ API endpoints are used by the connector.
    - When the current date is **prior to this value**, the connector uses **high-performance (undocumented) endpoints** to temporarily accelerate initial data syncing.
    - When the current date is **on or after this value**, or when the value is null, the connector automatically switches to **standard AIQ endpoints**, which follow AIQ’s normal rate limits.
  - This setting is **read-only** and is managed by Rapid programmers in coordination with AIQ when elevated rate limits are permitted.
 
## Bug Fixes and Performance Enhancements

- Fixed an issue where custom fields beginning with `USER_` (e.g., `USER_DOB`, `USER_SEX`) in customer and item field mappings were not included in the JSON payload sent to AIQ during data synchronization.

- Improved overall syncing performance, resulting in faster and more efficient processing of document data.

- Enhanced queue and internal processing performance to better handle large data volumes, improving reliability, scalability, and execution time.
  - Replaced cursor-based logic with TVP-driven processing across key stored procedures, including:
    - `USER_SP_AIQ_QUEUE_CRON`
    - `USER_SP_AIQ_QUEUE_AFTER_PS_DOC_POST_RUN`
    - `USER_SP_BEFORE_ITEM_MERGE_AIQ`
    - `USER_SP_BEFORE_CUSTOMER_MERGE_AIQ`
  - Updated `USER_SP_AIQ_QUEUE_ONE_DOCUMENT` to:
    - Accept TVPs as parameters
    - Utilize TVPs within dynamic SQL joins for improved performance
    - Resolve issues related to dynamic SQL execution

- Improved data processing reliability and consistency across merges, ticket history handling, and queue operations.
  - Enhanced handling of item and customer merges in Counterpoint, reducing the risk of duplicate or missed records
  - Standardized ticket history processing, including how valid records are passed to `USER_SP_AIQ_QUEUE_ONE_DOCUMENT`

- Resolved multiple data processing and stability issues, including:
  - Prevention of duplicate records during processing
  - Improved handling of background processing errors
  - More accurate tracking of document processing status
  - Fixed a primary key violation by preventing duplicate inserts into `@VALUES`, including a cleanup step during calculated column query processing
  - Corrected logic for determining `QUEUE_DOC_ITERATION`, ensuring accurate behavior when triggered by merge-related stored procedures

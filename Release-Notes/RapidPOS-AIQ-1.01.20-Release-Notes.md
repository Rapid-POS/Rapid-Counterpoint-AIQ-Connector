# RapidPOS AIQ Connector v1.01.20 Release Notes - Coming Soon

**Release Date:** September 27, 2026

_This release improves customer sync reliability, with fewer unnecessary retries, less database load, and fixes for several issues that could cause alerts or uploads to get stuck._

## New Features & Improvements

### Customers awaiting Alpine IQ setup now retry on their own schedule

Customers who are still waiting on Alpine IQ to finish creating their account no longer share a retry schedule with everyday sync rollbacks, so they no longer force a full re-upload every time a regular sync cycle runs.

- You can now set a dedicated "Rollback Customer Retry Execution Time" in the AIQ configuration screen, separate from the daily rollback schedule.

### Faster, more reliable customer field-mapping sync

Customer data sent to Alpine IQ, including any custom field mappings you've configured, is now pulled through a single, purpose-built data view instead of being looked up field-by-field for every customer.

- Template/placeholder customer records are automatically kept out of the sync.
- Field mapping setup now supports additional mapping types (calculated values, formatted dates, and direct value lookups), so you have more flexibility in what gets sent to Alpine IQ.

### Reduced database load during routine syncs

Updating your AIQ configuration no longer triggers an unnecessary rebuild of underlying customer-table logic on every sync cycle. This reduces extra load on your Counterpoint database during normal operation.

## Bug Fixes

### Customers no longer get stuck endlessly retrying an Alpine IQ lookup

A "no match found" response from Alpine IQ was, in some cases, misread as a rate-limit error. This caused already-synced customers to be flagged for a full re-upload and lookup retry over and over, which burned through API calls unnecessarily. Alpine IQ's "no match" responses are now recognized correctly, so this no longer happens.

### Stopped Message Center from being flooded with alerts for customers correctly skipped due to missing contact info

When "Send if no email or phone" is turned off, customers without a usable email or phone are intentionally skipped during sync. That's expected, not an error. The connector no longer sends a Message Center alert every time this expected skip happens.

### Ticket queue uploads that can never succeed no longer retry forever

Queue uploads that fail for a permanent reason (rather than a temporary service hiccup or rate limit) are now marked as failed so staff can see and address them, instead of being retried on every cycle indefinitely.

### Fixed "Mark All Messages as Read" silently failing for longer account names

Running the AIQ mark-all-messages-read action previously did nothing for accounts with a longer account name, due to a field-length mismatch. Account names of any supported length now work correctly.

## Maintenance

- Added automated test coverage for core sync logic to catch regressions earlier. The test project is now excluded from published connector builds.

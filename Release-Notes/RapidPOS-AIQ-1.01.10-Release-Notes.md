# Alpine IQ Connector v1.01.10 Release Notes

**Release Date:** October 17, 2026

_This release makes customer syncing to Alpine IQ faster and more reliable, fixes several cases where a customer's contact ID or sale details didn't come through correctly, and makes first-time setup safer._

## New Features & Improvements

### Safer first-time setup
New installs no longer start syncing customers automatically before setup is finished.

- Syncing now stays off by default until the connector is explicitly turned on, so a new install can't start pushing customer data before the Alpine IQ account is fully configured.

### More reliable customer syncing to Alpine IQ
Customer syncing to Alpine IQ now handles larger customer lists more smoothly.

- Each sync run now processes a safe, limited batch of customers at a time instead of trying to handle an entire backlog at once.
- Customers with no email address or phone number on file are now automatically set aside instead of being retried over and over.

## Bug Fixes

### Customer syncing could slow down or time out
Fixed an issue that could cause customer syncing to Alpine IQ to slow down or time out, especially for stores syncing a large number of customers at once.

### Synced customers could get stuck reprocessing
Fixed an issue where customers that had already finished syncing, or that failed to sync, could get stuck and keep getting reprocessed instead of settling into their correct status.

### Contact ID could fail to save after a successful match
Fixed an issue where a customer's Alpine IQ contact ID could fail to save even though the match with Alpine IQ succeeded, which could leave a customer's record incompletely linked.

### A customer missing contact info could block an entire enrollment batch
Fixed an issue where, during automatic customer enrollment, a single customer missing both email and phone could stop the rest of that batch's customers from being enrolled in Alpine IQ at all.

### Customers not found in Alpine IQ kept retrying every sync
Fixed an issue where a customer Alpine IQ couldn't recognize kept getting retried on every single sync run instead of being flagged so the connector stops repeating it.

### Sale/visit details could be formatted incorrectly
Fixed an issue where custom sale and visit details sent to Alpine IQ could be formatted incorrectly.

### Incorrect date range for past ticket history
Fixed an issue with the date range used when syncing past ticket and sale history to Alpine IQ.

## Maintenance

- Improved the reliability of the automatic setup process that connects customer records between Counterpoint and Alpine IQ, particularly for stores with longer account configurations.
- Increased the maximum length allowed for custom field mapping names, so longer mapping names no longer get cut off.
- General stability and security hardening improvements under the hood.

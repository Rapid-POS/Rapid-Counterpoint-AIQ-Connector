# AIQ Connector V1.01.11 Release Notes

**Release Date:** August 24th, 2026

This release makes the connector recover automatically from temporary Alpine IQ rate limits instead of marking customers, items, and sales as failed, and fixes a database error that could stop certain sales from syncing.

## New Features & Improvements

### Improved sync error logging

Internal error logging for sale/ticket syncing has been improved, making sync failures easier to diagnose and resolve.

## Bug Fixes

### Rate-limited syncs no longer get stuck as failed

Fixed several issues where a temporary Alpine IQ rate limit could cause customers, items, or sale records to get marked as permanently failed instead of trying again.

- When Alpine IQ asks the connector to slow down, it now waits the correct amount of time before retrying instead of giving up almost immediately.
- Customers, items, and sales that hit a rate limit are now automatically retried on the next sync instead of sitting stuck in an error state.

### Sales with certain custom field mappings could fail to sync

Fixed an issue where sale/ticket records using certain custom calculated field mappings could fail to sync to Alpine IQ with a database error.

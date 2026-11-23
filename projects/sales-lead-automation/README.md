# Sales Lead Automation

**An n8n-based lead capture and notification workflow with validation, duplicate detection, failure tracking, and scheduled email recovery.**

> **Project status:** MVP 03 — core workflows, retry-limit handling, and a controlled scheduled-recovery test have passed. Long-term reliability and production hardening remain pending. This is a demonstration project, not a production-ready service.

## Business Problem

Small teams often receive leads from forms or other inbound channels and manually copy details into spreadsheets, check for duplicates, and notify sales staff. These steps can create delays, duplicate records, and missed follow-ups—especially when a notification fails.

This project demonstrates how to automate that process while making delivery failures visible and recoverable.

## What It Does

- Accepts lead submissions through an HTTP POST webhook.
- Validates required fields and email format; records invalid submissions in an error log.
- Normalizes lead data, calculates a simple lead score, and assigns a priority.
- Checks existing Google Sheets records for duplicate email addresses.
- Writes accepted leads to Google Sheets and sends a Gmail notification.
- Tracks notification status (`PENDING`, `SENT`, `FAILED`, `RETRYING`, or `REVIEW`).
- Uses a separate global error workflow to email alerts for unhandled workflow failures.
- Provides a recovery workflow configured to check failed notifications every 15 minutes, subject to retry limits.
- Marks previously failed notifications that have reached the retry limit as `REVIEW` for manual investigation.

## Architecture

```text
Inbound HTTP POST
       |
       v
  Lead Capture Workflow
  Validate -> Score -> Check Duplicate -> Append to Google Sheets
                   |                         |
             Reject duplicate         Send Gmail notification
                                             |
                                      Update lead status

  Global Error Handler
  Unhandled workflow error -> Format alert -> Gmail alert

  Gmail Recovery (configured for every 15 minutes)
  Read Google Sheets -> Loop through leads -> Eligible for retry?
                                           | Yes
                                           v
                                    Mark RETRYING -> Resend Gmail
                                           |                |
                                        Success          Failure
                                           |                |
                                        Mark SENT      Mark FAILED / REVIEW

                              Not eligible for retry?
                                           |
                                 Retry limit reached?
                                    | Yes      | No
                                    v          v
                                Mark REVIEW   Skip
                                           
  Each path returns to the item loop.
```

**Note:** The lead-capture webhook returns a success response after the lead is stored, before Gmail notification completes. A successful webhook response therefore confirms lead acceptance, **not** email delivery.

## Workflow Files

| File | Purpose |
| --- | --- |
| [`workflows/lead-capture.json`](workflows/lead-capture.json) | Webhook intake, validation, scoring, duplicate detection, lead storage, and initial Gmail notification. |
| [`workflows/global-error-handler.json`](workflows/global-error-handler.json) | Email alert for unhandled workflow execution errors. |
| [`workflows/gmail-recovery.json`](workflows/gmail-recovery.json) | Scheduled retries, retry-limit review routing, and notification status updates. |

Supporting folders:

- `docs/` — architecture and setup notes (to be expanded).
- `tests/` — test scenarios and evidence (to be expanded).

## Retry and Recovery Rules

The main workflow records the initial notification attempt. The recovery workflow examines leads one at a time:

- **`FAILED` and attempts below 3, with a nonempty `lead_id`:** increment `notification_attempts`, set `RETRYING`, and attempt Gmail delivery again.
- **Successful resend:** update the record to `SENT`.
- **Explicit Gmail rate-limit response (e.g., HTTP 429), with attempts still below 3:** update to `FAILED` for a later retry.
- **Failed resend on the third attempt, or uncertain delivery outcome:** update to `REVIEW` for manual investigation.
- **Already `FAILED` with attempts of 3 or more and a nonempty `lead_id`:** update to `REVIEW` without sending another email.
- **Other records (for example `SENT`):** skip without modifying them.

When an already-failed record reaches the retry limit, the `Mark Review` node changes only `notification_status` to `REVIEW`, matching on `lead_id`. The original attempt count and last error are preserved.

**Known limitations:** `REVIEW` is a status for human follow-up; automated administrative notification or a dedicated review queue has not yet been implemented. The failure-classification path requires further regression testing. Scheduled execution has passed a controlled test, but long-term unattended operation remains unverified. Google Sheets does not provide a transactional lock for competing workflow runs; this demonstration has not been validated for concurrent high-volume processing.

## Requirements and Setup

- A running [n8n](https://n8n.io/) instance with compatible Google Sheets and Gmail nodes.
- A Google account authorized for Google Sheets and Gmail access.
- A Google Sheets document with a `Leads` worksheet and an `Error_Log` worksheet.

### Setup Steps

1. Import the three JSON files from `workflows/` into your n8n instance.
2. Configure Google Sheets and Gmail OAuth credentials in the relevant nodes.
3. Replace placeholder Google Sheets document IDs, worksheet selections, and notification email addresses.
4. Ensure the `Leads` sheet has the fields used by the workflows, including `lead_id`, `name`, `email`, `company`, `interest`, `source`, `lead_score`, `priority`, `notification_status`, `notification_attempts`, `last_error`, `last_attempt_at`, and `notified_at`. The `Error_Log` sheet needs the validation-error fields configured in the lead-capture workflow.
5. Set **Error Workflow** in the lead-capture workflow settings to the imported **Global Error Handler** workflow.
6. Test valid, invalid, and duplicate submissions, followed by Gmail success and failure paths, using non-sensitive test data.
7. Review the recovery schedule and retry behavior before activating the scheduled workflow.

> The committed JSON files are **sanitized templates**. They deliberately omit credentials, live Google Sheets identifiers, notification addresses, and environment-specific workflow bindings. Importing them alone does not configure a working integration.

## Test Status

| Scenario | Status |
| --- | --- |
| Basic lead intake and Google Sheets/Gmail integration | Tested during MVP 01–02 development |
| Input validation and duplicate handling | Implemented during MVP 02 development |
| Global error alert via an intentionally triggered workflow error | Tested |
| Recovery retry with simulated Gmail HTTP 429 response | Tested in a controlled scenario |
| B1 — `FAILED` with attempts = 3 changes to `REVIEW`, without another Gmail send | Passed (manual test) |
| B2 — `SENT` with attempts = 1 is skipped without modification | Passed (manual test) |
| B3 — `FAILED` with attempts = 2 follows the normal retry path | Passed (manual test) |
| All recovery failure classifications and status write-backs | Further regression testing required |
| C — Scheduled recovery (3-minute test interval) | Passed — automatic execution, Gmail delivery, and Google Sheets status update verified on 2026-11-02 |

**Test C notes (2026-11-02):**
- Temporarily changed Schedule Trigger from 15 minutes to 3 minutes.
- Confirmed that n8n automatically created an execution without manual intervention.
- Confirmed that the test lead was updated to `SENT` and the notification email was received.
- Restored the configured interval to 15 minutes and deactivated the workflow after testing.
- This was a controlled scheduled-run test, not a long-term unattended reliability test.

These results are based on manual development testing and are not a production reliability guarantee.

## Security and Operational Notes

- Never commit API keys, OAuth tokens, credential references, private sheet IDs, or real customer records.
- Use test data and a separate test sheet while validating the workflows.
- Limit Google account permissions and restrict access to the n8n instance and spreadsheets.
- Handled Gmail errors are tracked by workflow branches; the global error handler is intended for **unhandled** workflow failures, not every business-level failure.
- Review retry and notification behavior before using the project with real leads.

## Next Steps

Extend failure-path regression coverage, validate long-term unattended execution at the configured 15-minute interval, implement manual-review escalation notifications, and document reproducible test cases and deployment instructions.

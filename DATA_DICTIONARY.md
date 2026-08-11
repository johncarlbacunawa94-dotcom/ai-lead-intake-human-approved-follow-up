# Lead Operations Register — Data Dictionary

The Lead Operations Register is the shared operational record used across the lead workflows.

It stores the lead identity, source information, qualification result, review state, CRM identifiers, follow-up state, notification status, and failure details needed by later workflow stages.

The register is used as persistent state between workflows so downstream steps can verify what has already happened instead of relying only on transient execution output.


## Lead identity and source

| Field | Purpose |
|---|---|
| `record_key` | Unique internal identifier for the lead record. |
| `source_channel` | Where the lead entered the system, such as Gmail or website. |
| `source_event_id` | Identifier for the original source event used for duplicate protection. |
| `source_message_id` | Gmail message identifier when the lead originated from email. |
| `source_thread_id` | Gmail thread identifier when applicable. |
| `received_at` | Timestamp showing when the lead was received. |
| `raw_subject` | Original email subject or equivalent source subject. |
| `raw_message` | Original submitted message or lead inquiry text. |


## Contact and company information

| Field | Purpose |
|---|---|
| `contact_name` | Name of the lead or primary contact. |
| `email` | Submitted email address. |
| `normalized_email` | Standardized email value used for matching and validation. |
| `phone` | Submitted phone number when available. |
| `normalized_phone` | Standardized phone value used for matching and CRM lookup. |
| `company` | Company or organization associated with the lead. |
| `service_requested` | Service or solution the lead is interested in. |
| `request_summary` | Short summary of the lead’s request. |
| `urgency` | Indicates how urgent the lead’s request appears to be. |
| `estimated_value` | Estimated opportunity value used for qualification and reporting. |
| `currency` | Currency associated with the estimated opportunity value. |


## Qualification and routing

| Field | Purpose |
|---|---|
| `ai_category` | Category assigned during AI-assisted lead analysis. |
| `ai_priority` | Priority recommendation produced during qualification. |
| `ai_confidence` | Confidence value returned by the AI qualification step. |
| `ai_suggested_action` | Recommended next action from the AI assessment. |
| `deterministic_score` | Score calculated from fixed qualification rules. |
| `qualification_status` | Final qualification state, such as qualified, review required, or rejected. |
| `processing_status` | Current operational state of the lead within the workflow system. |
| `duplicate_type` | Indicates whether a duplicate condition was identified. |
| `duplicate_match_key` | Value used to identify or explain the duplicate match when applicable. |


## Human review and decision

| Field | Purpose |
|---|---|
| `review_required` | Indicates whether the lead must be reviewed by a person before continuing. |
| `review_reason` | Explains why the lead was routed to human review. |
| `review_status` | Current state of the review, such as pending, approved, or rejected. |
| `assigned_reviewer` | Name of the person responsible for reviewing the lead. |
| `review_decision` | Final reviewer action, such as approve, correct and approve, or reject. |
| `reviewer_notes` | Notes entered by the reviewer to explain the decision or corrections. |
| `decision_at` | Timestamp when the review decision was recorded. |
| `corrected_at` | Timestamp showing when reviewed lead data was corrected, when applicable. |


## CRM and follow-up

| Field | Purpose |
|---|---|
| `hubspot_contact_id` | HubSpot contact identifier saved after CRM resolution or synchronization. |
| `hubspot_deal_id` | HubSpot deal identifier associated with the lead. |
| `hubspot_sync_status` | Current CRM synchronization result or state. |
| `draft_email_subject` | Subject prepared for the customer follow-up draft. |
| `draft_email_body` | Body prepared for the customer follow-up draft. |
| `gmail_draft_id` | Gmail draft identifier created for the approved follow-up. |
| `follow_up_date` | Scheduled internal follow-up date and time when one is required. |
| `calendar_event_id` | Google Calendar event identifier when an internal reminder is created. |
| `notification_status` | Status of the internal notification associated with the lead. |


## Operational tracking and failure data

| Field | Purpose |
|---|---|
| `batch_execution_id` | Identifier used to associate the lead with the workflow execution or processing batch. |
| `failure_stage` | Workflow stage where a failure occurred, when applicable. |
| `failure_message` | Error or failure detail recorded for operational troubleshooting. |
| `processed_at` | Timestamp showing when processing for the lead completed. |
| `last_updated_at` | Timestamp of the most recent update to the lead record. |
| `internal_notification_message_id` | Gmail message identifier for the internal notification when one was sent. |
| `internal_notification_thread_id` | Gmail thread identifier associated with the internal notification. |


## How the register is used

The Lead Operations Register is not only a reporting table. It is part of the control logic for the automation.

Later workflow stages use the stored record to verify whether important actions have already happened, including CRM synchronization, review decisions, Gmail draft creation, Calendar reminders, and internal notifications.

This makes the workflow less dependent on a single execution path and reduces the chance of repeating an external action after a retry, browser resubmission, or partial failure.


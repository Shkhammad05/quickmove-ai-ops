# QuickMove AI Ops — Build Log

## Project Goal

Build a small AI-assisted operations tool that converts a coordinator's message into a proposed relocation task update, requests human approval, updates Google Sheets, and records an audit entry.

## Development Log

### Step 1 — Workflow Discovery
Mapped relocation operations, key stakeholders, handoffs, dependencies, and possible automation opportunities.

### Step 2 — Google Sheets Setup
Created the workbook with the following tabs:

- Relocations
- Tasks
- Audit Log

Added fictional test records to validate the prototype.

### Step 3 — Apps Script and User Interface
Built a Google Apps Script backend and an HTML interface for entering messages, reviewing AI proposals, selecting a task, and approving or rejecting updates.

### Step 4 — Gemini API Integration
Connected the application to the Gemini API and configured the model request to return structured proposal fields.

**Issue encountered:** A model request returned HTTP 404, indicating that the selected model was not available at the requested endpoint.

**Issue encountered:** Another request returned HTTP 503, indicating temporary service unavailability or overload.

**Resolution:** Changed the model configuration and tested again. A model configuration later returned a working response. Confirm the exact model name in the final deployed code before submission.

### Step 5 — Task Update and Audit Logging
Implemented approval and rejection handling. Approval updates the selected task and creates an audit entry; rejection should log the decision without changing the task.

**Issue encountered:** `TypeError: Cannot read properties of null (reading 'waitLock')`

**Cause:** The selected document lock was null in the web app execution context.

**Resolution:** Changed the locking mechanism to `LockService.getScriptLock()` and tested the approval flow again.

**Observed result:** The application displayed a successful approval message, and the task row and audit log were updated.

### Step 6 — Rejection Testing
Tested the rejection flow.

**Observed result:** Rejection worked during the manual test. Verify that the task remained unchanged and the rejection appeared in the audit log.

### Step 7 — Edge-Case Testing
Executed tests for:
- Date without an explicit year
- Conflicting arrival times
- Missing Move ID
- Unconfirmed time change
- Prompt injection
- Normal mover confirmation

Record the exact output and whether unsafe task updates were prevented for each scenario.

## AI Prompt and Response Evidence

The complete original prompt history and all API responses must be captured from the actual conversation and application logs where available. Do not reconstruct missing messages and present them as verbatim history.

### Example test input

"Update for QM-1001: the mover confirmed the truck will arrive at 9 AM on 18 October. The booking is confirmed."

### Recorded AI response

```json
{
  "reason": "The year is not explicitly stated in the message for the date '18 October'.",
  "blocker": "",
  "workflow": "",
  "confidence": "medium",
  "eventTime": "09:00 AM",
  "nextAction": "",
  "needsHumanReview": true,
  "proposedStatus": "",
  "moveId": "QM-1001",
  "eventDate": "",
  "eventType": "Truck Arrival"
}
```

This response left the year-dependent date blank and requested human review. It also left workflow, proposed status, and next action blank, showing that the proposal still required coordinator input.

## Current Limitations

- AI responses can omit fields or misinterpret messages.
- Human approval is required.
- The prototype is not a live WhatsApp integration.
- Test outcomes should be supported by observed application behavior.
- Production use would require stronger validation, monitoring, and security review.
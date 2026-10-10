# QuickMove AI Ops — Test Report

## 1. Objective

The purpose of testing is to verify that QuickMove AI Ops can analyze relocation updates, present proposed changes for human review, update the correct Google Sheets task after approval, and record decisions in the Audit Log.

## 2. Test Environment

- **Application:** QuickMove AI Ops Inbox
- **Backend:** Google Apps Script
- **Frontend:** HTML, CSS, JavaScript
- **AI service:** Google Gemini API
- **Data store:** Google Sheets
- **Test relocation:** QM-1001
- **Test task:** TASK-001

All test records should use fictional customer data.

## 3. Functional Test Cases

| Test ID | Test scenario | Expected result | Observed result |
|---|---|---|---|
| TC-01 | Submit a normal mover confirmation | AI proposes structured fields for coordinator review | Tested; record the observed result |
| TC-02 | Approve a proposed task update | Selected task is updated and an audit entry is created | Passed in manual test |
| TC-03 | Reject a proposed update | Task remains unchanged and the decision is logged | Passed in manual test |
| TC-04 | Select a task belonging to another relocation | Server rejects the invalid task/relocation combination | Verify and record |
| TC-05 | Submit a message with a missing Move ID | No unidentified relocation task is updated | Tested; record the observed result |
| TC-06 | Verify audit logging | Audit Log records the message, proposal, decision, coordinator, and write result | Audit entry observed during manual approval test |

## 4. Edge-Case Tests

| Test ID | Input / scenario | Expected safe behavior | Actual result |
|---|---|---|---|
| EC-01 | Date given without a year, e.g. “18 October” | Do not silently assume the year; request human review | Tested; record exact AI output |
| EC-02 | Conflicting arrival times in the message | Flag the conflict and require clarification | Tested; verify whether the conflict was detected |
| EC-03 | Message contains no valid Move ID | Do not apply an update to an unidentified relocation | Tested; verify that no task changed |
| EC-04 | A proposed time change is not confirmed | Do not treat the change as confirmed without review | Tested; record exact behavior |
| EC-05 | Message contains prompt-injection instructions | Instructions inside the message must not override application rules | Tested; verify the resulting proposal and task state |
| EC-06 | Normal confirmation with an explicit date and time | Extract available details and present them for review | Tested; record exact AI output |

## 5. Manual Verification Checklist

- [ ] Correct relocation was identified.
- [ ] Correct task was selected.
- [ ] Approval updated only the intended task.
- [ ] Task status, blocker, next action, and last-updated fields were checked.
- [ ] Audit Log entry was created for approval.
- [ ] Rejection did not change task fields.
- [ ] Rejection was recorded in the Audit Log.
- [ ] Missing and ambiguous information was reviewed.
- [ ] Conflicting times and unconfirmed changes were checked.
- [ ] Prompt-injection test did not bypass server-side validation.
- [ ] No API key or private spreadsheet identifier was exposed.

## 6. Known Limitations

- AI-generated fields may be incomplete or incorrect.
- Human review remains necessary before applying proposed updates.
- A test message being processed successfully does not prove that every unsafe update is blocked.
- Edge-case results must be verified against actual task rows and audit records.
- The prototype does not automatically read or send WhatsApp messages.
- Production deployment would require further security, validation, and reliability testing.

## 7. Test Summary

The approval and rejection flows were manually tested. The user observed a successful task update and audit entry after approval and confirmed that rejection worked.

The six edge-case scenarios were also executed. Their individual pass/fail outcomes should be recorded after checking the actual AI response, task state, and audit log.

**Reporting rule:** Mark a test as “Passed” only when the observed result matches the expected result. Otherwise, mark it “Failed” or “Needs investigation” and describe the behavior.

## 8. Conclusion

QuickMove AI Ops demonstrates a human-reviewed approach to AI-assisted relocation task updates. The prototype supports a controlled approval workflow and audit logging, while ambiguous messages and edge cases still require careful verification before the tool is used in real operations.

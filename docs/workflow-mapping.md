# QuickMove AI Ops — Workflow Mapping

## 1. Business Context

QuickMove is a relocation company coordinating customer moves across multiple cities. The operations team manages relocation activities using messaging applications, spreadsheets, and email.

Manual transfer of information between these tools can lead to delays, missed follow-ups, and inconsistent task records.

## 2. Existing Workflow

1. Customer onboarding and relocation details collection
2. Moving budget and timeline confirmation
3. Property matching and viewing coordination
4. Rental agreement and document readiness
5. Security deposit coordination
6. Mover sourcing, pricing, and booking
7. Move-out and move-in access coordination
8. Utility and internet setup
9. Address change paperwork
10. Damage inspection and final partner billing

## 3. Stakeholders

- Customer: provides requirements and confirms arrangements
- Operations coordinator: tracks tasks, updates records, and follows up
- Mover/vendor: confirms availability, pricing, and arrival time
- Property owner or society contact: coordinates access and move-in requirements
- Utility/service provider: handles service activation and related tasks

## 4. Main Handoffs and Dependencies

| Handoff | Dependency | Potential issue |
|---|---|---|
| Customer → Coordinator | Accurate move details | Missing or unclear information |
| Coordinator → Mover | Confirmed date, inventory, and access | Incorrect booking details |
| Coordinator → Property/Society | Move-in approval and access | Delayed permissions |
| Coordinator → Utility provider | Address and service details | Delayed activation |
| Vendor → Coordinator | Confirmation and final charges | Late updates or billing mismatch |

## 5. Automation Opportunities

### Opportunity 1: Message-to-Task Update
Extract a proposed task update from an operations message and match it to the correct relocation record.

### Opportunity 2: Document Readiness Tracking
Track required agreements and documents, identify missing items, and flag tasks requiring follow-up.

### Opportunity 3: Mover Booking and Reconfirmation
Track booking status, arrival time, outstanding confirmations, and follow-up actions.

## 6. Implemented Prototype

The QuickMove AI Ops Inbox prototype focuses on message-to-task updates.

1. Coordinator enters an operations message.
2. Gemini analyzes the message and proposes structured fields.
3. The application checks the relocation reference and displays the proposal for review.
4. The coordinator selects the relevant task and approves or rejects the update.
5. Approved updates are written to the selected task in Google Sheets.
6. The application records the decision in the Audit Log.

## 7. Safety and Human Oversight

- AI output is a proposal, not an automatic business decision.
- The coordinator reviews the proposed update before approval.
- Rejected proposals should not change the selected task.
- Ambiguous dates, conflicting times, and missing relocation references require careful review.
- API keys and private configuration values must not be published in the repository.

## 8. Scope Limitations

The prototype does not automatically read WhatsApp messages or send customer/vendor messages. Document readiness and mover reconfirmation are identified as future automation opportunities, not implemented features.
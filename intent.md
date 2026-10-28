# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded history and approves nothing in this intent.

## Problem

- **I-1** The clinic receives avoidable calls from patients checking whether their results are ready. These status-only calls take support time and tell the patient nothing they could not learn themselves. (PRD: *"The fictional clinic receives avoidable calls from patients checking whether results are ready."*)

## Proposed outcome

- **I-2** The patient is notified when a result is ready for their review in the existing authenticated portal. (PRD: *"Notify the patient when a result is ready for their review in the existing authenticated portal."*)
- **I-3** "Ready" means a doctor has reviewed **and** released the result. A finished lab upload alone never makes a result eligible for notification. (PRD: *"A doctor must review and release the result before any notification is eligible."* Notes: *"'Ready' must not mean that the laboratory upload merely finished."*)

**How success will be observed**

- **I-4** Results-status calls go down, counted separately from clinical questions and compared against a measured baseline. (PRD: *"Measure reduction in results-status calls"*. Support lead: *"count status-only calls separately from clinical questions; compare against a measured baseline."*)
- **I-5** Delivery failures are measured and do not lead to duplicate patient messages. (PRD: *"Measure reduction in results-status calls and delivery failures."*)
- **I-6** No baseline or numeric target is approved. Both will be agreed during discovery, and this intent sets no target. (PRD: *"The baseline and numerical target will be agreed during discovery; none is approved yet."*)

## Affected users and systems

- **I-7** Patients: the only approved recipient of the notification.
- **I-8** Doctors: their review and release of a result is what makes it eligible for notification.
- **I-9** Support staff: they handle the calls this work aims to reduce, and they classify calls as status-only or clinical.
- **I-10** The existing authenticated portal and its existing result route, which the notification directs the patient to. (Engineering lead: *"the existing portal path is adequate."*)
- **I-11** The notification sending and logging components. Neither may carry sensitive content.

**In scope:** use of the existing portal result route, prevention of duplicate notifications for the same released result version, and handling of delivery failures without duplicate patient messages.

## Out of scope

- **X-1** Portal redesign. (PRD scope)
- **X-2** A new or rebuilt patient dashboard, which is a separate project. (Engineering lead: *"A new patient dashboard is a separate project."*)
- **X-3** Appointment scheduling. (PRD scope)
- **X-4** Clinical interpretation of results. (PRD scope)
- **X-5** Notifying third parties. (PRD scope) The referring doctor is not settled by this exclusion. It remains a separate open question (OPEN-2).
- **X-6** Everything that only the superseded request asked for: notifying on lab upload, including the test result in an SMS, copying the referring doctor, rebuilding the dashboard and a "90% call reduction in one week" target. None of it is approved.

## Constraints

- **C-1** Review before notification: no notification is eligible until a doctor has reviewed and released the result. (PRD, product owner)
- **C-2** The message only says that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, clinical measurement or treatment detail goes in the message. (PRD)
- **C-3** The same restriction applies to notification and operational logs. (PRD; privacy owner: *"no sensitive content in messages or operational logs."*)
- **C-4** Access to the result is authenticated in the existing portal. No attached results and no public result link. (PRD: *"Do not attach results or introduce a public result link."*)
- **C-5** No duplicate notifications for the same released result version, including when delivery fails and is retried. (PRD)
- **C-6** Only the patient is notified. No other recipient is approved until the open questions below are decided.

## Open questions and owners

- **OPEN-1** Channel (SMS, email or portal push). **Owners: product owner and privacy owner.** They decide after the consent and delivery review. No channel is chosen in this intent. (PRD: *"product owner and privacy owner must decide after consent/delivery review."* Privacy owner: *"Consent and the actual channel are still under review."*)
- **OPEN-2** Whether to notify the referring doctor. **Owners: clinical owner and privacy owner.** They decide whether the referring doctor is an authorized recipient. This is not approved by the PRD, and approval must not be inferred from the superseded request. (PRD: *"It is not approved by this PRD."* Clinical owner: *"do not infer approval for referring-doctor notifications from the old request."*)

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```

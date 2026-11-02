# 001 — OT Certification screen

Risk tier: The risk **High** as the approved Overtime information is updated in SuccessFactors and then to Payroll for salary payment.
Owner: Human Resources department
Status: Draft. Subject to Department Management and TMS Administrators approval.
Design reference: "OT Certification Screen" mock-up (23 Oct 2026)

## Problem

Employees entered their Overtime comprising of OT Type (Pre-OT, Pre-OT for Next Day, Post OT, Post OT for Previous Day, Call Back OT, Meal break OT) and the number of hours they are claiming into AutoStar time clock. This information is updated into SuccessFactors. The TMS Administrators will verify the Overtime and shifts per employee per week in SuccessFactors. 

The TMS Administrators verify the Overtime by checking the assigned overtime assigned by Resource Planners, Allocators, Control desk and approved by department managers or Duty Managers. The assigned over time can be a Hardcopy PDF (Cargo Services), Online Approval App export (Customer Services and Airside) and others that we need to check (Catering and Technical Services).

Besides the OT hours approved by Management and the OT hours declared by the employees, the TMS Administrators also refer to the Clock in and clock out (refer to Attendance hereafter) received from the time-clock to address the following scenario:
a. Employees who did not show up for Call Back OT. They will not have any Attendance received from the time-clock
b. Employees who leave early due to OT completed earlier than expected for Post OT. They should be paid up till the clock-out. This practice is not applicable to Pre OT since the employee reports for work earlier upon by the instructions from management.

Currently, the verification is done manually. As a result, OT is sometimes paid without an approved request, for more hours than were approved, or for time the staff member wasn't on site. Catching these cases is slow and inconsistent between admins.

## Goal

For one shift date, a department admin can see every declared OT entry side by side with the approved request and the clock-in/out times. Discrepancies are flagged automatically. The admin corrects or rejects entries as needed and certifies the rest to SuccessFactors in one submission, with no manual cross-checking between systems.

## Users and journeys

1. As a department admin, I retrieve all OT entries for a shift date and one or more of my departments, optionally narrowed to one staff member by SAP No, so I can work through the day's OT.
2. As a department admin, I see each entry's approved request directly above what the staff member declared, with clock-in/out beside them, so I can compare them at a glance.
3. As a department admin, I can tell which entries need attention from a red/amber/green alert with a plain-language reason, so I check the problem cases first.
4. As a department admin, I can bulk-select all entries that match their approved request, so routine OT takes seconds.
5. As a department admin, I can correct a declared entry (type, hours, start, end) to what was actually approved or attended, and still see what the staff member originally declared.
6. As a department admin, I can mark an entry for rejection so it is never sent for payment.
7. As a department admin, I can save my work as a draft and come back later before submitting.
8. As a department admin, I submit my certifications and rejections in one action and get confirmation of what was sent.

## Acceptance criteria

### Retrieval

- [ ] AC1: Given no shift date or no department is selected, when I press Retrieve, then nothing is retrieved and the missing field is flagged inline ("Shift date is required." / "Select at least one department.").
- [ ] AC2: Given a shift date, one or more departments and an optional SAP No, when I press Retrieve, then I see every declared OT entry that matches, each showing the employee's name, SAP No, department and roster for that day.
- [ ] AC3: The department picker lets me select multiple departments, and includes Select all and Clear.
- [ ] AC4: The screen shows the time of the last retrieval and the three source systems (SharePoint OT requests, time clock, roster).
- [ ] AC5: Given no entries match, then I see "No OT entries found for the selected shift date, department(s) and SAP No."
- [ ] AC6: Reset returns the filters to their defaults and clears any validation messages.

### Comparison view

- [ ] AC7: Each entry shows two rows. The **Approved** row is read-only and shows request number, status, OT type, hours, start and end from SharePoint. The **Declared** row is editable and shows type, hours, start and end as the staff member declared them.
- [ ] AC8: Each entry shows clock-in and clock-out from the time clock, and "Not recorded" for any that is missing.
- [ ] AC9: Any declared value that differs from the approved value is visibly highlighted.
- [ ] AC10: Given there is no request, then the Approved row shows "No request found" and dashes for its values.
- [ ] AC11: OT types are Pre-shift OT, Post-shift OT, Recall – REST day and Recall – OFF day.

### Alerts

Each entry gets an overall severity, which is its worst issue. All issues are listed, worst first.

- [ ] AC12 (red): Given any of the following, then the entry is red:
    - no clock-in and no clock-out ("staff did not report for OT; do not award")
    - only one of clock-in or clock-out missing
    - clock-out before clock-in
    - clock-in/out with no overlap with the declared OT window
    - no request found in SharePoint
    - the request was rejected by the manager
    - declared hours exceed approved hours by more than 1 h
    - OT end before OT start
    - OT start or end missing
- [ ] AC13 (amber): Given any of the following, then the entry is amber:
    - clocked in more than 5 min after OT start
    - clocked out more than 5 min before OT end
    - request still pending manager approval
    - declared type differs from the approved type
    - declared hours above approved by up to 1 h, or below approved by any amount
    - declared start or end more than 15 min from the approved time
    - declared hours not equal to the duration from start to end
- [ ] AC14 (green): Given none of the above, then the entry is green with "Matches approved request", or "Matches approved request (adjusted by admin)" if the admin edited it.
- [ ] AC15: Summary counts at the top show the total, red, amber and green entries, plus the number selected (with total hours) and the number marked for rejection. The counts update as I edit.
- [ ] AC16: Alerts are recalculated as soon as a declared value changes.

### Corrections

- [ ] AC17: When I edit a declared field, the field is marked as edited and the staff member's original value is shown beneath it ("Staff: …" for type and hours; "Roster: …" for start and end).
- [ ] AC18: "Revert edits" restores all of that entry's declared values to what was originally retrieved.
- [ ] AC19: Given the staff member arrived late or left early and the attended time is less than the declared hours, then a one-click fix offers "Set to X h attended". X is the overlap of attendance with the OT window, rounded down to the nearest 15 min. Applying it sets hours, start and end to match.
- [ ] AC20: Hours accept 0 to 16 in 0.25 steps.

### Selection, rejection and submission

- [ ] AC21: I can select entries individually, select all eligible entries, select all green entries, or clear the selection. Rejected and already-processed entries cannot be selected.
- [ ] AC22: Marking an entry for rejection deselects it, locks its inputs and shades the row. "Undo reject" reverses this before submission.
- [ ] AC23: Given one or more selected entries are red, then a warning shows how many before I submit. Submission is not blocked.
- [ ] AC24: Submit is disabled until at least one entry is selected or marked for rejection.
- [ ] AC25: When I submit, selected entries become **Certified** and are queued to SuccessFactors External Time Data, using the declared values as corrected by me. Rejected entries become **Rejected** and are not sent. A confirmation states the number certified, total hours and number rejected.
- [ ] AC26: Certified and rejected entries are locked and show their final status. They cannot be edited, selected or resubmitted.
- [ ] AC27: Save draft keeps my selections, rejections and corrections until I submit, and confirms the save.

### Error cases

- [ ] AC28: Given a source system (SharePoint, time clock or roster) is unavailable during retrieval, then I am told which source failed and no partial comparison is presented as complete.
- [ ] AC29: Given submission to SuccessFactors fails for some or all entries, then those entries stay uncertified, I am told which failed, and I can retry.

## Non-goals

- Creating, approving or editing OT requests (these stay in SharePoint).
- Editing time-clock records or the roster.
- Calculating OT pay rates or payroll amounts (SuccessFactors does this).
- Staff-facing views; this screen is for department admins only.
- Mobile layout; this is a desktop web app.
- Undoing a submission after it has gone to SuccessFactors.

## Constraints

- **Data sources:** OT requests from SharePoint; clock-in/out from the time-clock database (SQL Server); roster as a third source. Certified output goes to SuccessFactors External Time Data, then Timesheet, then Payroll.
- **Access:** admins only see and certify departments they are authorised for.
- **Audit:** every certification, rejection and admin correction must be recorded with who, when, the original value and the new value. The record must be kept for <retention period>.
- **Personal data:** staff names, SAP numbers and attendance are personal data and must not be logged or exported outside approved systems.
- **Accessibility:** alert severity is never conveyed by colour alone, and every control is keyboard-operable and labelled.
- **Time zone:** all times are local airport time (Asia/Singapore). OT that crosses midnight belongs to the shift date it started on.

## Open questions

- [ ] Should red entries be blocked from certification rather than only warned (AC23)? Today the design allows it.
- [ ] Should rejection require a reason, and should the staff member or their manager be notified?
- [ ] Are the 5 min, 15 min and 1 h tolerances (AC12, AC13) company policy? Should they be configurable per department?
- [ ] Can a pending request (amber) be certified, or must it wait for manager approval?
- [ ] Can two admins work on the same department and date at once? If yes, how are conflicting drafts handled?
- [ ] Can one staff member have more than one OT entry on the same shift date, e.g. pre-shift and post-shift?
- [ ] Is OT certified per shift date, or can a pay period's entries be retrieved together?
- [ ] What is the cut-off for certifying before payroll runs? Should the screen show it?
- [ ] What is the audit retention period?

## Risks and rollback

| Risk | Who is affected | Mitigation / rollback |
| --- | --- | --- |
| Alert rules wrong, so overpayment is shown as green | Company payroll cost | Run in parallel with the manual process for one pay cycle and compare the results before go-live |
| Admin certifies a red entry by mistake | Payroll, audit | Red-selection warning (AC23); audit trail; finance can reverse it in SuccessFactors |
| Duplicate submission to SuccessFactors | Staff are paid twice | Each entry can only be certified once (AC26); the interface rejects duplicate entry IDs |
| Source data stale or missing | Wrong certification decisions | Show the retrieval time (AC4) and fail loudly when a source is unavailable (AC28) |
| Rollback | — | Feature flag returns admins to the current manual process; certified entries already sent are corrected in SuccessFactors by payroll |

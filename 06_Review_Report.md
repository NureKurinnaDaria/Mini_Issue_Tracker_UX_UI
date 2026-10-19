# G6 — Updated Mini Issue Tracker Review



This update incorporates the student-reported results of a manual walkthrough in Figma Present mode. These are observations reported by the student, not actions I independently performed or observed. Static screenshots provide evidence of the UI's appearance, while the reported Present mode results provide evidence of the claimed functionality of the listed transitions.



## Initial check: Screen/State IDs and readability



The stated set contains 9 frames:



| ID     | Purpose                           | Readability in the provided images                                     |
| ------ | --------------------------------- | ---------------------------------------------------------------------- |
| S-01   | Authentication boundary           | Readable                                                               |
| S-02-A | Issues — All                      | Readable                                                               |
| S-02-B | Issues — Important                | Readable                                                               |
| S-02-C | Empty All                         | Readable                                                               |
| M-01   | Create Issue modal                | Readable                                                               |
| S-03   | Issue Details                     | Readable                                                               |
| REF-01 | Per-issue permissions             | Readable                                                               |
| REF-02 | Compact state references          | Generally readable; small annotations are less comfortable to read     |
| REF-03 | Contract & navigation annotations | Generally readable; small tables and annotations require magnification |



The provided images make it possible to read the main values, states, and most annotations. However, this is not a complete inspection of the source Figma file or all its properties.



## Part A — Requirements



### A1. Updated status of transitions



The results in the table below are student-reported: the student reports having performed a manual walkthrough in Present mode.



| Source / Transition  | Expected                                                  | Screen/State    | Observed evidence                                                                               | Result                              | Remaining test                                                                   |
| -------------------- | --------------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------- | -------------------------------------------------------------------------------- |
| D-02 / T-01          | S-01 leads to S-02-A                                      | S-01 → S-02-A   | Student reports that the transition works in Present mode                                       | PASS — reported                     | Independent walkthrough not performed                                            |
| D-01 / T-22          | Navigate to Important                                     | S-02-A → S-02-B | Student reports that the tab works                                                              | PASS — reported                     | Verify filtering against the full data set                                       |
| D-01 / T-23          | Return to All                                             | S-02-B → S-02-A | Student reports that the tab works                                                              | PASS — reported                     | Verify list-state preservation                                                   |
| US-01 / T-04         | Create opens the modal                                    | S-02-A → M-01   | Student reports that the overlay opens                                                          | PASS — reported                     | Test the overlay from both tabs; T-04 is specified for S-02-A/B                  |
| US-01 / T-08         | Cancel closes the modal and returns to the previous state | M-01 → S-02-A   | Student reports that Cancel/Close works                                                         | PASS — reported                     | Return to the exact previous tab has not been demonstrated for Important         |
| US-02 AC-02.3 / T-02 | Open issue details                                        | S-02-A → S-03   | Student reports that row ISS-003 opens S-03                                                     | PASS — reported                     | Test other rows and confirm the selected ID matches                              |
| US-02 AC-02.3 / T-03 | Open Important issue details                              | S-02-B → S-03   | Student reports that ISS-003 opens S-03                                                         | PASS — reported                     | Test other Important rows                                                        |
| D-07 / T-24          | Back returns to the previous tab                          | S-03 → S-02-A   | Student reports that Back works; conditional return has been simplified to a single destination | PARTIAL — reported                  | Test returning to Important after entering from S-02-B                           |
| US-01 AC-01.4 / T-05 | After successful Create, open S-03 for the new issue      | M-01 → S-03     | Student reports that the prepared outcome works                                                 | PASS — reported for navigation only | Verify actual creation, ID, Open status, author, timestamp, and Priority Not Set |



Important distinction: T-05 has a reported successful route to S-03, but this does not confirm actual issue creation and persistence. Likewise, transitions between tabs do not prove that the filtering algorithm is correct.



### A2. Transitions not wired as separate prototype paths



These states are shown in REF-02 / REF-03, but the student reports that the corresponding separate prototype paths are not wired. Therefore, the presence of a visual state is not equivalent to a tested interaction.



| Source / Transition             | Expected                                                       | Screen/State | Evidence                                                             | Result         | Remaining test                                                  |
| ------------------------------- | -------------------------------------------------------------- | ------------ | -------------------------------------------------------------------- | -------------- | --------------------------------------------------------------- |
| BR-03, BR-04; D-03 / T-09       | Team Lead applies an assignee                                  | S-03         | Selector and annotations are present, but there is no separate route | NOT VERIFIABLE | Wire/test Apply and confirm the assignee changes                |
| BR-05, BR-06; D-03, D-06 / T-11 | Team Lead applies priority                                     | S-03         | Selector and annotations are present, but there is no separate route | NOT VERIFIABLE | Test Apply, allowed values, and the Priority update             |
| BR-08, BR-08.1; US-07 / T-13    | Open → In Progress                                             | S-03         | Allowed path is described in REF-01/03                               | NOT VERIFIABLE | Perform the transition in Present mode or the implementation    |
| BR-08, BR-08.1; US-07 / T-14    | In Progress → Resolved                                         | S-03         | Allowed path is described in REF-01/03                               | NOT VERIFIABLE | Perform the transition                                          |
| BR-08, BR-08.1; US-07 / T-15    | Resolved → Closed                                              | S-03         | Allowed path is described in REF-01/03                               | NOT VERIFIABLE | Perform the transition and check Important afterward            |
| BR-08, BR-08.1; US-07 / T-16    | Resolved → In Progress                                         | S-03         | Allowed path is described in REF-01/03                               | NOT VERIFIABLE | Perform the transition                                          |
| US-06 AC-06.1–AC-06.4 / T-19    | A valid comment is added                                       | S-03         | A success fixture is present, but the route is not wired             | NOT VERIFIABLE | Test adding a comment and displaying its text, author, and time |
| US-06 AC-06.1 / T-20            | An empty comment is rejected                                   | S-03         | A validation fixture is present, but the route is not wired          | NOT VERIFIABLE | Submit an empty comment and confirm that no comment is created  |
| US-06 AC-06.5 / T-21            | A save failure does not create a comment and displays an error | S-03         | An error fixture is present, but the route is not wired              | NOT VERIFIABLE | Trigger the failure outcome and check field behavior and state  |



### A3. Other requirements and visible evidence



| Source                                   | Expected                                                                                                 | Screen/State      | Observed evidence                                                                                                  | Result                                       | Remaining test                                                                 |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------- | ------------------------------------------------------------------------------ |
| US-01 AC-01.1–AC-01.3                    | Create contains only Title and Description; both are required                                            | M-01, REF-02      | Two fields and validation examples are visible                                                                     | PASS — visual                                | Verify that creation is blocked in both cases                                  |
| US-01 AC-01.4–AC-01.5; BR-03–BR-05; D-03 | New issue: Open, Not Set, no assignee                                                                    | REF-02            | ISS-007 is shown with these values                                                                                 | PASS — visual fixture                        | Verify the actual Create outcome                                               |
| US-02 AC-02.1–AC-02.2                    | An authorized user can view the list; each row has ID, Title, Status, Priority, and Assignee if assigned | S-02-A/B          | Columns and example rows are visible                                                                               | PASS — visual                                | Test behavior with real data                                                   |
| US-02 AC-02.4                            | Details contains the required fields                                                                     | S-03              | ID, Title, Description, Status, Priority, Assignee, Author, and Created at are shown                               | PASS — visual                                | Verify that the data matches the selected issue                                |
| US-02 AC-02.5; D-01; P-D-01              | Empty All and the approved Empty Important state                                                         | S-02-C, REF-02    | Both states are represented; Empty Important is marked as an approved UX decision, not a source requirement        | PASS — visual                                | Verify actual empty-result outcomes                                            |
| US-03; BR-06; D-04–D-06                  | Only Team Lead can assign; selection from three users; Apply                                             | S-03, REF-01/02   | Controls and permission matrix are visible                                                                         | PASS — visual                                | Test Apply and retain the D-04 caveat regarding teacher confirmation           |
| US-04 AC-04.1–AC-04.2; BR-05–BR-06       | Priority: Low/Normal/High/Critical; only Team Lead can change it                                         | S-03, REF-01/02   | Options and disabled state are shown                                                                               | PASS — visual                                | Test role restrictions and Apply                                               |
| US-07 AC-07.4; BR-08                     | Only the assignee can change status; a non-assignee Team Lead cannot                                     | S-03, REF-01/02   | Enabled/disabled states are shown visually                                                                         | PASS — visual                                | Server-side authorization has not been tested                                  |
| BR-08.1; US-07 AC-07.5                   | Only allowed destinations are available; Closed has no next transition                                   | S-03, REF-01/02   | The matrix shows the correct allowed paths and Closed without a next action                                        | PASS — contract/visual                       | Test interactive and server-side restrictions                                  |
| US-08 AC-08.1–AC-08.4; BR-09, BR-09.1    | Important = High/Critical and Status ≠ Closed                                                            | S-02-B, REF-01/02 | ISS-002 and ISS-003 are included; Closed ISS-004, Normal ISS-005, and Not Set ISS-006 are excluded in the fixtures | PASS — fixture coverage                      | Test actual filtering and updates after a status change                        |
| US-06 AC-06.2–AC-06.5                    | Comment has text, author, timestamp; failure does not create a comment                                   | S-03, REF-02      | Static success, empty, and error examples are present                                                              | PASS — visual / NOT VERIFIABLE — persistence | Test persistence and failure handling                                          |
| D-05                                     | Forbidden role-dependent controls remain visible but disabled, rather than hidden                        | S-03, REF-01/02   | Disabled controls are shown                                                                                        | PASS — visual                                | Verify actual permissions                                                      |
| D-06                                     | Apply is beside Assign/Priority/Status; success remains on S-03                                          | S-03, REF-03      | Apply placement is visible; the relevant paths are not wired                                                       | PARTIAL                                      | Test successful Apply outcomes                                                 |
| D-07                                     | Back preserves the previous tab                                                                          | S-03, REF-03      | Student reports a simplification to a single Back destination                                                      | PARTIAL — reported                           | Conditional return or an explicitly documented simulation limitation is needed |
| D-08                                     | Fictitious data and predefined outcomes; no real DB/login                                                | All               | Data appears as fixed examples; REF-03 describes the limitations                                                   | PASS — visual scope                          | This is not evidence of persistence or real authentication                     |
| NFR-01                                   | Response time < 1 s                                                                                      | All               | No measurement                                                                                                     | NOT VERIFIABLE                               | Measure in the implementation                                                  |
| NFR-02 / BR-01                           | Real authentication is required                                                                          | S-01              | Only the authentication boundary is shown                                                                          | NOT VERIFIABLE                               | Test authentication and access control                                         |
| NFR-03                                   | HTTPS only                                                                                               | All               | No network evidence                                                                                                | NOT VERIFIABLE                               | Check environment configuration                                                |
| NFR-04                                   | Back up all data                                                                                         | All               | No infrastructure evidence                                                                                         | NOT VERIFIABLE                               | Check backup policy and recovery                                               |
| BR-10, BR-11                             | Issues cannot be deleted; no separate user-facing change history in v1                                   | All               | No delete or change-history controls are visible                                                                   | PASS — visible scope                         | Inspect the full file/implementation                                           |
| Release Scope                            | MVP = US-01–US-08; US-09 deferred; SR-10/11 future                                                       | All               | Attachments, AI controls, and extra pages are not visible                                                          | PASS — visible scope                         | Inspect the rest of the Figma file if it contains additional pages             |
| D-01                                     | Desktop, grayscale, low-fidelity; one list surface with All/Important; Create modal                      | S-02, M-01, S-03  | Visual structure matches the decision                                                                              | PASS — visual                                | Inspect native layers and Auto Layout in the Figma file itself                 |



Overall conclusion for Part A: the additional information allows an improved assessment of the nine navigation transitions that the student reports as tested. However, Apply, status transitions, and comment scenarios remain unwired; implementation-level properties cannot be considered proven.



## Part B — UX: Up to 5 Evidence-Based Problems



This is an expert review of the mockups, not an accessibility certification or a study with real users.



| ID    | Severity | Location                       | Evidence                                                                                                                | Minimal correction                                                                                                 |
| ----- | -------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| UX-01 | Medium   | S-03 — Status / Apply          | Allowed destinations are visible in the reference, but status transitions are not wired as separate paths               | Wire and test each allowed transition without adding new statuses                                                  |
| UX-02 | Medium   | S-03 — Assignee/Priority Apply | Controls are visible, but T-09/T-11 have no separate prototype paths                                                    | Wire the approved Apply outcomes or clearly keep them marked as static fixtures                                    |
| UX-03 | Medium   | S-03 → Back                    | D-07 is simplified to a single destination; returning to the previous tab is not demonstrated                           | If technically possible, test both return paths; otherwise retain an explicit annotation describing the simulation |
| UX-04 | Medium   | REF-02 / REF-03                | Reference states and annotations are dense, and some text is small                                                      | Increase small text or better group the reference material without creating new product screens                    |
| UX-05 | Low      | S-03 — disabled controls       | It is visually clear that controls are unavailable, but the reason for unavailability is not always stated next to them | Add a short explanation where compatible with D-05 and the approved design                                         |



## Part C — DEF-ID / UX-ID | Severity | Location | Source | Evidence | Minimal Correction



| ID     | Severity | Location                       | Source                                      | Evidence                                                                                 | Minimal correction                                                                         |
| ------ | -------- | ------------------------------ | ------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| DEF-01 | Medium   | S-03 — status transitions      | BR-08.1; US-07 AC-07.5; D-06                | T-13–T-16 remain annotations, not separate Present mode paths                            | Wire and run all four allowed transitions                                                  |
| DEF-02 | Medium   | S-03 — Apply Assignee/Priority | BR-06; D-04–D-06                            | T-09/T-11 are not wired; static selectors do not prove a value change                    | Test Apply outcomes and the updated value within the simulation                            |
| DEF-03 | Medium   | S-03 — comments                | US-06 AC-06.1–AC-06.5                       | T-19/T-20/T-21 are not wired as separate routes                                          | Test success, empty validation, and failed-save outcomes                                   |
| DEF-04 | Medium   | S-03 → S-02 tab state          | D-07; T-24                                  | Student reports a single Back destination instead of conditional return                  | Test return from both tabs or retain the documented prototype limitation                   |
| DEF-05 | Medium   | S-02-B — Important             | US-08 AC-08.1–AC-08.4; BR-09.1              | Fixtures are correct, but the actual transition of ISS-003 to Closed has not been tested | Perform Resolved → Closed and revisit Important                                            |
| DEF-06 | Low      | M-01 — Create outcome          | US-01 AC-01.4–AC-01.5; T-05                 | T-05 is reported as a working prepared outcome, but this does not prove actual creation  | Verify all initial values and the new ID; do not treat the prepared outcome as persistence |
| UX-01  | Medium   | S-03 — Status / Apply          | BR-08.1; D-06                               | Status transitions are not wired                                                         | Wire the allowed paths without adding functionality                                        |
| UX-02  | Medium   | S-03 — Assignee/Priority Apply | D-06                                        | No separate prototype paths for Apply                                                    | Wire the approved outcomes                                                                 |
| UX-03  | Medium   | S-03 — Back                    | D-07                                        | The simplified route does not preserve the previous tab in all cases                     | Test conditional return or retain the documented limitation                                |
| UX-04  | Medium   | REF-02 / REF-03                | D-01; requirements for readable annotations | Dense reference text                                                                     | Improve readability without changing functional scope                                      |
| UX-05  | Low      | S-03 — disabled controls       | D-05                                        | The reason for unavailability is not always explained next to the control                | Add a short explanation if it does not conflict with the approved design                   |



DEF-IDs are audit identifiers, not IDs from the original specification.



## Final Status



1. UI design coverage



## Complete for supplied visual states



All nine stated Screen/State/Reference IDs are represented in the provided visual evidence. The main requirements and states are visually covered, but this does not mean that all ACs have been tested interactively or at implementation level.



2. Prototype evidence



## Reported — partially verified by student



The student reports nine successful routes in Present mode. This is more informative than static screenshots alone, but I did not independently run through the prototype. T-09, T-11, T-13–T-16, and T-19–T-21 are not wired as separate paths; D-07 has been simplified.



3. Implementation tests outstanding



## Required



Outstanding tests include Apply outcomes, the four status transitions, comment success/validation/error, conditional Back, actual creation and data persistence, server-side authorization, Important filtering after a status change, authentication, HTTPS, backups, and response-time measurement.



### Final G6 Conclusion



The visual set has complete coverage of the stated core UI states, and the student reports successfully completing nine navigation routes. However, the prototype evidence remains reported, not independently observed, and a significant number of important scenarios remain unwired. Therefore, a general statement that “all ACs have been met” is not justified.


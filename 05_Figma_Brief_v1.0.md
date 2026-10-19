# Mini Issue Tracker — Figma Design Implementation Brief



## 1. Purpose and scope



Create a desktop, grayscale, low-fidelity Figma mockup and a clickable prototype for the final Release MVP only: US-01–US-08. Implement the approved screen contracts in `03_Screen_Contracts_v1.0.md` and the latest `Decision_Log.md`; do not reinterpret them or add requirements.



This is a design/prototype task, not a software implementation task. Use fixed fictitious data and predefined prototype outcomes. The prototype should communicate screen structure, control availability, representative states, and annotated navigation.



Source references: Release scope and product surfaces — G3 §0; approved decisions — D-01–D-08; fixture/prototype constraints — D-08.



## 2. Design constraints and Figma construction



* Desktop layout; grayscale; low-fidelity. (D-01)



* Use editable, Figma-native layers, not flattened screenshots.



* Build reusable local components for repeated controls and patterns.



* Use Auto Layout where it improves consistency and maintainability.



* Give frames, components, layers, and variants meaningful names that include canonical IDs where applicable.



* Keep all product frames on one working Figma page.



* Put specification notes, source references, verification examples, and state matrices outside product frames, on the same working page.



* Create only the canonical product frames and states listed below. Do not create a full screen for every role/status combination.



* Prototype work at this step consists of transition annotations and clickable navigation only. Do not implement backend, real authentication, coded conditional logic, or code-based permission enforcement.



* Use fixed fictitious fixture data and predefined outcomes; do not connect to a real database. (D-08)



* Do not add page-icon placeholders, decorative squares, row-selection controls, or bulk-action controls.



Source references: G3 §0, §16; D-01, D-05, D-06, D-08.



## 3. Canonical screens and states



Create these exact canonical IDs. States of S-02 are states of one list surface, not separate product pages.



### S-01 — Authentication boundary



* Purpose: show that authentication is a boundary before the product demo, not a designed login flow.



* Show only a concise annotation such as `Authentication required — demo starts authenticated`.



* No login fields, login form, or product controls.



* Annotate `T-01 → S-02-A`.



* The demonstration starts in an already-authenticated context. (D-02)



Source references: G3 §1; BR-01, NFR-02; D-02.



### S-02-A — All Issues



* One issue list with `All` active and `Important` available.



* Each issue row shows: Issue ID, Title, Status, Priority, and Assignee when assigned.



* Display an unset priority exactly as `Not Set`; never substitute `Low` or `Normal`.



* Include `Create`, available to any authenticated Team Member, including Team Lead.



* Selecting an issue opens S-03.



* If there are no issues, show S-02-C.



Source references: G3 §2; US-02 AC-02.1–AC-02.3, BR-02, BR-04; D-01, D-03.



### S-02-B — Important Issues



* Use the same list structure and fields as S-02-A, with `Important` active.



* Apply the exact Important predicate in Section 7.



* Include `All` and `Create`; `Create` is available to any authenticated Team Member, including Team Lead.



* Selecting a matching issue opens S-03.



* When there are zero matching issues, show an empty state consistent with S-02. This empty Important representation is approved UX, not a new source requirement. (P-D-01 / APPROVED)



Source references: G3 §3, §10; US-08 AC-08.1–AC-08.4, BR-09, BR-09.1; D-01, D-03; P-D-01 / APPROVED in `Decision_Log.md`.



### S-02-C — Empty All Issues



* Show the empty All Issues state with `All` active, `Important` available, and `Create` available.



* Exact empty-state copy is not specified by the source; use concise neutral copy without presenting it as mandatory source wording.



* Keep this state distinct from the approved empty Important state within S-02-B.



Source references: G3 §4; US-02 AC-02.5; D-01.



### M-01 — Create Issue modal



* Open from S-02-A or S-02-B.



* Include exactly two user-entered fields: `Title` and `Description`; both are required.



* Include `Create` and `Cancel`/close.



* Do not present Status, Priority, or Assignee as user-entered creation fields.



* On successful Create, navigate only to S-03 for the newly created issue.



* Cancel/close returns to the previous S-02 list state.



Source references: G3 §5; US-01 AC-01.1–AC-01.5, BR-02–BR-04; D-01, D-04, D-07.



### S-03 — Issue Details



Show these issue fields:



* Issue ID



* Title



* Description



* Status



* Priority (`Not Set` if unset)



* Assignee, only when assigned



* Author



* Created at



Include the assignment, priority, status, comments, and Back controls described in Sections 5–6. Keep these controls on S-03; do not create separate Assignment, Priority, Status, or Comments pages.



Source references: G3 §6; US-02 AC-02.4, US-03–US-07, BR-04–BR-08.1; D-03–D-07.



## 4. Required labels and data



Use the following exact labels and values where applicable:



* List tabs: `All`, `Important`



* Primary list action: `Create`



* Create fields: `Title`, `Description`



* Detail fields: `ID`, `Title`, `Description`, `Status`, `Priority`, `Assignee`, `Author`, `Created at`



* Assignment control: `Assignee`; action: `Apply`



* Priority control: `Priority`; action: `Apply`



* Status control: `Status`; action: `Apply`



* Comment entry: `Comment`; action: `Add Comment`



* Detail navigation: `Back`



* Unset priority: `Not Set`



* Priority values: `Low`, `Normal`, `High`, `Critical`



* Status values: `Open`, `In Progress`, `Resolved`, `Closed`



* Required Title validation copy: `Title is required`



A valid created issue displays a generated-looking fixture ID, the entered Title and Description, `Status = Open`, `Priority = Not Set`, the current fixture user as Author, a creation timestamp, and no Assignee. Treat IDs and timestamps as mock data, not evidence of real persistence.



Source references: G3 §2, §5–6; US-01 AC-01.1–AC-01.5, US-02 AC-02.2–AC-02.4, BR-03–BR-05; D-03, D-04, D-08.



## 5. Controls and permissions



Permission rules apply per issue. “Assignee” means the current user is assigned to that particular issue; Team Lead status alone does not grant permission to change status.



| Context                   | View | Create | Comment | Assign | Change priority | Change status                       |
| ------------------------- | ---- | ------ | ------- | ------ | --------------- | ----------------------------------- |
| Team Member, non-assignee | Yes  | Yes    | Yes     | No     | No              | No                                  |
| Team Member, assignee     | Yes  | Yes    | Yes     | No     | No              | Yes, subject to allowed transitions |
| Team Lead, non-assignee   | Yes  | Yes    | Yes     | Yes    | Yes             | No                                  |
| Team Lead, assignee       | Yes  | Yes    | Yes     | Yes    | Yes             | Yes, subject to allowed transitions |



### Assignment



* Only Team Lead can assign or reassign.



* The assignee selector can offer any of the three predefined users, including Team Lead.



* After successful creation, the issue has no assignee.



* Show the current assignee when assigned.



* Use selection followed by `Apply`; after successful Apply, remain on S-03.



* For non-Team Leads, show the role-forbidden assignment control disabled, not hidden.



### Priority



* Only Team Lead can set or change priority.



* Available choices are exactly `Low`, `Normal`, `High`, `Critical`.



* Before a priority is set, display `Not Set`.



* `Not Set` is a displayed unset state, not a selectable priority; do not provide a reset action.



* Use selection followed by `Apply`; after successful Apply, remain on S-03.



* For non-Team Leads, show the role-forbidden priority control disabled, not hidden.



### Status



* Only the issue's responsible assignee can change its status, and only when an outgoing transition exists.



* Show only the allowed destination(s) for the current status; never offer a forbidden destination.



* For a non-assignee, including a Team Lead who is not the assignee, the Status control is unavailable/disabled.



* If the issue has no assignee, Status is unavailable to everyone.



* For `Closed`, offer no next-status action.



* Use selection followed by `Apply`; after a successful change, remain on S-03.



* A role-forbidden control should be disabled rather than hidden. A forbidden status destination should not be offered in the first place.



### Comments



* Any authenticated team member, including Team Lead, can add a comment; assignment is not required.



* A stored comment shows its text, author, and creation date/time.



* Empty comment submission creates no comment and leaves the user on S-03 in a validation state.



* If comment saving fails, no comment is created, an error is visible, and the user remains on S-03. The exact error wording is unresolved; use neutral placeholder copy and annotate it as non-final rather than asserting a required phrase.



Source references: G3 §6–9, §11; US-03 unnumbered acceptance points, US-04 AC-04.2, US-06 AC-06.1–AC-06.5, US-07 AC-07.4–AC-07.5, BR-02, BR-05–BR-08.1; D-03–D-06.



## 6. Status transitions



The only permitted transitions are:



| Current status | Available destination(s) |
| -------------- | ------------------------ |
| `Open`         | `In Progress`            |
| `In Progress`  | `Resolved`               |
| `Resolved`     | `In Progress`, `Closed`  |
| `Closed`       | None                     |



A dash in the full transition matrix means “same current status”, not a transition. Do not add any other transition. Annotate prototype paths for the permitted transitions; each successful status Apply remains on S-03 and shows the resulting status.



Non-transitions/negative cases must be represented as annotations or compact state examples, not as outgoing transition arrows:



* A Team Member who is not Team Lead cannot assign or change priority.



* A non-assignee cannot change status, even if the user is Team Lead.



* An issue without an assignee has no available status control.



* A forbidden destination is not offered.



* A Closed issue has no next-status action.



Source references: G3 §6.2, §9, Addendum §1–3; US-07 AC-07.4–AC-07.5, BR-08, BR-08.1; D-05, D-06.



## 7. Important predicate



An issue is Important if and only if:



`Priority ∈ {High, Critical} AND Status != Closed`



Therefore:



* `High + Resolved` → Important.



* `Critical + In Progress` → Important.



* `Critical + Closed` → Not Important.



* `Not Set + Open` → Not Important.



* `Normal + Resolved` → Not Important.



`Resolved` is not the same as `Closed`. A Resolved issue remains eligible for Important until it becomes Closed. After `ISS-003` changes from High + Resolved to Closed, annotate that it remains visible on S-03 with `Status = Closed` and no longer appears in S-02-B when Important is revisited. This is a predefined prototype outcome, not proof of production filtering or persistence.



Source references: G3 §3, §10, Addendum TEST-33; US-08 AC-08.1–AC-08.4, BR-09, BR-09.1.



## 8. Required empty, validation, and error examples



Include these examples as canonical states or compact annotations; do not create extra product pages for them.



1. S-02-C — Empty All: no issues; `All` active; `Important` and `Create` remain available. Exact copy is unspecified.



2. S-02-B — Empty Important: no issue satisfies the predicate; show an empty state consistent with S-02. This UX is P-D-01 / APPROVED, but it is not a source-level product requirement.



3. M-01 — Empty Title: do not create an issue; show exact error `Title is required`.



4. M-01 — Empty Description: do not create an issue. Exact error copy is not specified; do not invent mandatory wording.



5. S-03 — Priority Not Set: display `Not Set`; no reset control.



6. S-03 — No Assignee: show no assignee value; Status unavailable to everyone.



7. S-03 — Empty Comment: no comment is created; show validation state.



8. S-03 — Failed comment save: no comment is created; show an error; remain on S-03. Exact copy is unresolved (Q-06); annotate placeholder copy as non-final.



9. S-03 — Non-assignee / Lead non-assignee: Status unavailable/disabled.



10. S-03 — Closed: no next-status destination or outgoing status action.



11. S-03 — Forbidden status destination: destination is not offered; no transition occurs.



Source references: G3 §2–6, §11, Addendum §1–3; US-01 AC-01.2–AC-01.3, US-02 AC-02.5, US-06 AC-06.1/AC-06.5, US-07 AC-07.4–AC-07.5, BR-08.1; D-03, D-05, P-D-01 / APPROVED.



## 9. Navigation intent and prototype annotations



Annotate and link only these relevant paths:



* `T-01`: S-01 → S-02-A.



* `T-02`: S-02-A → S-03 by selecting an issue.



* `T-03`: S-02-B → S-03 by selecting a matching Important issue.



* `T-04`: S-02-A or S-02-B → M-01.



* `T-05`: successful Create → S-03 for the newly created issue only.



* `T-06`: empty Title validation remains on M-01.



* `T-07`: empty Description validation remains on M-01.



* `T-08`: Cancel/close → the previous S-02 state.



* `T-09`: successful assignee Apply remains on S-03 with updated assignee.



* `T-11`: successful priority Apply remains on S-03 with updated priority.



* `T-13`: Open → In Progress, then remain on S-03.



* `T-14`: In Progress → Resolved, then remain on S-03.



* `T-15`: Resolved → Closed, then remain on S-03.



* `T-16`: Resolved → In Progress, then remain on S-03.



* `T-19`: successful Add Comment remains on S-03 with the stored comment.



* `T-20`: empty-comment validation remains on S-03.



* `T-21`: failed comment save remains on S-03 with error visible.



* `T-22`: S-02-A → S-02-B.



* `T-23`: S-02-B → S-02-A.



* `T-24`: Back from S-03 → previous S-02 state; preserve the previous tab.



* `T-25`: S-02-A → S-02-C when there are no issues.



`T-10`, `T-12`, `T-17`, and `T-18` are non-transitions and must not be drawn as navigation arrows. Keep status changes and Apply outcomes on S-03; do not route them to a new screen. At this step, add transition annotations only; do not build coded logic.



Source references: G3 §1–6, Addendum §1–2; D-01, D-06, D-07.



## 10. Sample data and compact state reference



Use fictitious data only. Include at least these three predefined user identities:



* `Member A` — Team Member



* `Member B` — Team Member



* `Team Lead` — Team Lead



Use representative issues to demonstrate:



* An unassigned new issue with `Open` + `Not Set`.



* Ordinary non-Important issues.



* A `High + Resolved` issue that appears in Important.



* A `Critical + Closed` issue that does not appear in Important.



* `ISS-003` with `Priority = High`, `Status = Resolved`, `Assignee = Member A`, used for the Resolved → Closed example.



Outside product frames, add `REF-01` as a compact verification reference with 4 user/assignee contexts × 4 statuses = 16 examples. It is not a product screen and must not become 16 full frames.



Contexts:



1. Team Member non-assignee



2. Team Member assignee



3. Team Lead non-assignee



4. Team Lead assignee



For every context, include all statuses: `Open`, `In Progress`, `Resolved`, `Closed`. Show whether Assign, Priority, and Status controls are available and list the allowed status destination(s). Apply the permission matrix in Section 5 and transition rules in Section 6. Team Lead non-assignee must never receive status permission merely because they are Team Lead. For an assigned user at `Closed`, there is no next status action.



Source references: G3 Addendum §3–4; D-04, D-08.



## 11. Exclusions



Do not design or imply the following capabilities:



* Attachment upload.



* AI categorization or AI priority recommendation.



* Issue deletion.



* User-facing change history or activity log.



* Separate Assignment, Priority, Status, or Comments pages.



* Additional statuses or status transitions.



* Extra Create fields beyond Title and Description.



* A full login/authentication flow.



* Real authentication, backend, database, persistence, or server integration.



* Coded conditional logic or code-based permission enforcement.



* Row selection, bulk actions, page-icon placeholders, or decorative squares.



The mock/prototype cannot verify response time under 1 second, HTTPS-only transport, backup of all data, actual authentication/authorization, real persistence, generated IDs, real author identity/timestamps, server-side transition enforcement, actual comment-save failure handling, or production filtering/query behavior. Do not mark these as passed based on a Figma artifact.



Source references: G3 §0, §14, §16, Addendum §5; BR-10, BR-11, NFR-01–NFR-04; D-08.



## 12. Acceptance checklist



* Only the canonical product surfaces/states are present: S-01, S-02-A, S-02-B, S-02-C, M-01, S-03.



* One S-02 list surface uses All/Important tabs; M-01 is a modal.



* The page is desktop, grayscale, low-fidelity, with editable native layers, reusable local components, appropriate Auto Layout, meaningful names, and one working page.



* Specification notes and REF-01 are outside product frames.



* No page-icon placeholders, decorative squares, row-selection controls, or bulk controls appear.



* List and detail fields match the required data lists; `Not Set` is shown exactly and never silently converted.



* Create has only required Title and Description fields; Create is available to both roles.



* Successful Create opens S-03 for the new issue only; the new issue is Open, Not Set, authored by the current fixture user, timestamped, and unassigned.



* S-02-C represents empty All; empty Important uses the approved P-D-01 treatment without becoming a new product screen.



* Role/assignee permissions match the matrix; forbidden role-dependent controls are disabled, not hidden.



* Assignment and priority selectors use only allowed values and require Apply; no priority reset exists.



* Status offers only allowed destinations and only to the issue assignee; non-assignee, Lead non-assignee, unassigned, and Closed cases are represented correctly.



* All and only the four permitted status transitions are shown; status Apply remains on S-03.



* Important uses exactly `Priority ∈ {High, Critical} AND Status != Closed`; Resolved is included, Closed is excluded.



* Comment success shows text, author, and creation time; empty comment and failed-save cases create no comment and remain on S-03.



* Required navigation annotations preserve the previous list tab on Back; the four non-transition cases are not drawn as arrows.



* REF-01 contains all 16 combinations (4 contexts × 4 statuses) as compact verification examples, not full frames.



* All data is fictitious; prototype uses predefined outcomes and annotations only, with no backend, real authentication, or coded permission logic.



* Excluded capabilities are absent; non-verifiable implementation/NFR items are not marked PASS.



## Source and decision status note



This brief is compiled from `03_Screen_Contracts_v1.0.md` (G3 screen contracts and addendum) and the current `Decision_Log.md`.



* P-D-01 is APPROVED as a UX decision in the current Decision Log, while remaining not a source requirement.



* Q-06 remains unresolved only as to the exact comment-save error wording; show the required error state without presenting invented copy as mandatory.



* D-04 is marked APPROVED in the Decision Log, but that same entry says teacher confirmation is required because post-create/reassignment behavior is not explicitly detailed in the source specification. Preserve this caveat in the working notes; do not rewrite D-04 as a source acceptance criterion.


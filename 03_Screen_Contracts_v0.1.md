Нижче — G3 Screen Contracts із застосуванням DEF-02…DEF-06. **Source requirements не розширюю**; нові UX-рішення позначаю окремо як `PROPOSED D-ID`.



# G3 — Developer-Verifiable Screen Contracts



## 0. Scope and contract rules



Release MVP: **US-01…US-08**.

US-09 — Deferred; SR-10/SR-11 — Future і в G3 не включаються.



Product surfaces:



* **S-01** — Authentication boundary;

* **S-02** — Issues List:



&#x20; * S-02-A All;

&#x20; * S-02-B Important;

&#x20; * S-02-C Empty All;

* **M-01** — Create Issue modal;

* **S-03** — Issue Details.



Assignment, Priority, Status і Comments не є окремими screens.



Approved decisions carried forward: **D-01…D-08**.



Clarifications applied:



* **DEF-02:** Create і Add Comment доступні будь-якому authenticated Team Member, включно з Team Lead. Assignee може бути будь-яким із трьох predefined users, включно з Team Lead.

* **DEF-03:** успішний Create завжди → **S-03 нової issue**.

* **DEF-04:** S-02-C = empty All Issues. Empty Important — окремий **PROPOSED UX state**, не SOURCE.

* **DEF-05:** T-10, T-12, T-17, T-18 — non-transitions / state examples.

* **DEF-06:** status behavior залежить від assignee relation; Team Lead не отримує status permission автоматично.

* **REF-01:** поза продуктовими frames: 4 role/assignee contexts × 4 statuses як компактні verification examples.



---



# 1. Screen / State Contracts



## S-01 — Authentication boundary



### 1.1 Contract



**Purpose:** позначити межу authentication для MVP demonstration.



**Actors:** authenticated Team Member, authenticated Team Lead.



**Entry condition:**



* користувач уже authenticated;

* демонстрація починається після authentication boundary.



**Visible data:**



* жодних login fields;

* жодного вигаданого login form;

* службова annotation: authentication required / already authenticated.



**Controls:** відсутні.



**Default state:** authenticated boundary → S-02-A.



**Empty state:** N/A.



**Validation/error:** authentication implementation не перевіряється продуктовим screen contract.



**Incoming:** зовнішній authentication context.



**Outgoing:**



* `T-01 → S-02-A`.



**Source/D-ID:** BR-01, NFR-02, **D-02**.



### 1.2 Verification



**Макет:** перевіряється лише наявність boundary annotation і відсутність вигаданого login UI.



**Прототип:** перевіряється, що demonstration starts at authenticated context і веде до S-02-A.



**Реалізація:** перевіряється фактична authentication/access control. Це не доводиться Figma-прототипом.



---



# 2. S-02 — Issues List



## 2.1 S-02-A — All Issues



### Purpose



Перегляд усіх доступних issues.



**Actors:** authenticated Team Member, authenticated Team Lead.



**Entry:**



* `T-01` з S-01;

* `T-23` з S-02-B;

* `T-24` після Back із S-03, якщо попереднім state був All;

* після відповідного flow створення/оновлення, коли list state потрібно показати.



### Visible data



| Field    | Required                  | Missing representation                        |
| -------- | ------------------------- | --------------------------------------------- |
| Issue ID | yes                       | не має бути відсутнім для валідної issue      |
| Title    | yes                       | не має бути відсутнім                         |
| Status   | yes                       | одне з Open / In Progress / Resolved / Closed |
| Priority | yes як displayed property | `Not Set`, якщо priority не встановлено       |
| Assignee | conditional               | відсутній/порожній, якщо issue не assigned    |



List **не має перетворювати `Not Set` на Low/Normal**.



### Controls



| C-ID        | Label     | Type                 | Visible / enabled                                                         | Trigger | Result             | Source/D-ID         |
| ----------- | --------- | -------------------- | ------------------------------------------------------------------------- | ------- | ------------------ | ------------------- |
| **C-02-01** | All       | tab                  | visible; active у S-02-A                                                  | click   | залишається S-02-A | D-01                |
| **C-02-02** | Important | tab                  | visible; enabled                                                          | click   | `T-22 → S-02-B`    | US-08, D-01         |
| **C-02-03** | Create    | button               | visible; enabled для будь-якого authenticated team member, включно з Lead | click   | `T-04 → M-01`      | BR-02, D-01, DEF-02 |
| **C-02-04** | Issue row | selectable list item | visible для кожної issue                                                  | click   | `T-02 → S-03`      | US-02 AC-02.3       |



### Default / empty / validation / error



**Default:** All tab selected.



**Empty:** якщо немає issues:



`S-02-A → S-02-C`.



Це **SOURCE-backed empty state** за US-02 AC-02.5.



Точний visual copy empty state не визначений source.



### Incoming / outgoing



**Incoming:** T-01, T-23, T-24.



**Outgoing:** T-02, T-04, T-22, T-25.



### Verification



**Макет:**



* видно ID/Title/Status/Priority/Assignee;

* `Not Set` відображається саме як `Not Set`;

* All/Important tabs присутні;

* Create доступний Team Member і Team Lead;

* empty All має окремий state.



**Прототип:**



* row → S-03;

* Important → S-02-B;

* Create → M-01;

* Back із S-03 повертає попередній tab.



**Реалізація:**



* реальний list retrieval;

* access control;

* актуальність status/priority/assignee;

* persistence.



---



# 3. S-02-B — Important Issues



### Purpose



Показати issues, що задовольняють Important predicate.



**Actors:** authenticated Team Member, authenticated Team Lead.



### Entry



* `T-22` з S-02-A;

* `T-24`, якщо попереднім state був Important.



### Visible data



Ті самі поля, що S-02-A:



ID, Title, Status, Priority, Assignee.



### Important predicate



```text

priority ∈ {High, Critical}

AND

status != Closed

```



Приклади:



| Priority | Status   | Important? |
| -------- | -------- | ---------- |
| High     | Resolved | **Yes**    |
| Critical | Closed   | **No**     |
| Not Set  | Open     | **No**     |



`Resolved` **не дорівнює Closed**.



### Controls



| C-ID        | Label     | Type       | Condition                                 | Trigger | Result          | Source/D-ID   |
| ----------- | --------- | ---------- | ----------------------------------------- | ------- | --------------- | ------------- |
| **C-02-01** | All       | tab        | visible/enabled                           | click   | `T-23 → S-02-A` | D-01          |
| **C-02-02** | Important | tab        | visible/active                            | click   | remains S-02-B  | D-01          |
| **C-02-03** | Create    | button     | enabled for any authenticated team member | click   | `T-04 → M-01`   | BR-02, DEF-02 |
| **C-02-04** | Issue row | selectable | for each matching issue                   | click   | `T-03 → S-03`   | US-02, US-08  |



### Empty



**SOURCE:** US-08 defines which issues are Important but does not define a dedicated Important-empty screen.



Therefore:



**S-02-B with zero matching issues → PROPOSED UX state.**



`PROPOSED D-ID: P-D-01` — represent zero Important results as an empty state visually consistent with S-02, without introducing a new product screen.



This must not be treated as a new product requirement.



### Verification



**Макет:** predicate examples can be represented with fixture data.



**Прототип:** switching All ↔ Important and opening a matching issue.



**Реалізація:** actual filtering must satisfy the exact predicate.



---



# 4. S-02-C — Empty All Issues



### Purpose



Показати empty All Issues state.



**Actors:** authenticated Team Member, authenticated Team Lead.



**Entry:**



* `T-25` from S-02-A when no issues exist.



### Visible data



* empty-state indication;

* All tab active;

* Important tab;

* Create control.



Exact copy is not source-defined.



### Controls



Same navigation controls as S-02:



* `C-02-01` All;

* `C-02-02` Important;

* `C-02-03` Create.



### Verification



**Макет:** перевіряється existence of empty state.



**Прототип:** Create і tab navigation continue to work.



**Реалізація:** actual zero-result condition.



**Source:** US-02 AC-02.5.



---



# 5. M-01 — Create Issue



### Purpose



Створення нової issue через modal.



**Actors:** **будь-який authenticated team member**, включно з Team Lead.



**Entry:** `T-04` from S-02-A or S-02-B.



## 5.1 User-entered vs automatically assigned data



| Data        | Entered by user? | Required | After creation             |
| ----------- | ---------------: | -------: | -------------------------- |
| Title       |              Yes |      Yes | stored                     |
| Description |              Yes |      Yes | stored                     |
| ID          |               No |        — | automatically assigned     |
| Status      |               No |        — | `Open`                     |
| Priority    |               No |        — | `Not Set`                  |
| Author      |               No |        — | current authenticated user |
| Created at  |               No |        — | current creation time      |
| Assignee    |               No |        — | absent after creation      |



### Controls



| C-ID        | Label        | Type            | Condition                                       | Trigger | Result                       | Source/D-ID                         |
| ----------- | ------------ | --------------- | ----------------------------------------------- | ------- | ---------------------------- | ----------------------------------- |
| **C-01-01** | Title        | text input      | visible/enabled                                 | type    | user-entered Title           | US-01 AC-01.1                       |
| **C-01-02** | Description  | text input/area | visible/enabled                                 | type    | user-entered Description     | US-01 AC-01.1                       |
| **C-01-03** | Create       | button          | visible/enabled; requires valid required fields | click   | success → `T-05 → S-03`      | US-01; BR-02–04; **DEF-02, DEF-03** |
| **C-01-04** | Cancel/close | modal control   | visible/enabled                                 | click   | `T-08` → previous S-02 state | D-01                                |



### Validation



**Title empty:**



* issue not created;

* error: `Title is required`.



**Description empty:**



* issue not created.



The source requires Description, but does **not** prescribe its exact error text.



### Successful Create



`T-05`:



**M-01 → S-03 of the newly created issue only.**



Visible:



* generated ID;

* Title;

* Description;

* Status = Open;

* Priority = Not Set;

* Author = current user;

* Created at;

* Assignee absent.



### Verification



**Макет:**



* both user fields visible;

* requiredness represented;

* no Priority/Status/Assignee input is presented as user-entered Create data;

* Team Lead is not excluded from Create.



**Прототип:**



* valid input → S-03 new issue;

* empty Title → validation;

* Cancel → previous list state.



**Реалізація:**



* actual creation;

* unique ID;

* persistence;

* current user as author;

* current creation time;

* Open / Not Set defaults.



---



# 6. S-03 — Issue Details



### Purpose



Єдина detail surface для перегляду issue, assignment, priority, status і comments.



**Actors:**



* Team Member non-assignee;

* Team Member assignee;

* Team Lead non-assignee;

* Team Lead assignee.



Assignee — **відношення конкретного користувача до конкретної issue**, а не глобальна роль.



## 6.1 Visible data



| Field                 | Required                  | Missing representation           |
| --------------------- | ------------------------- | -------------------------------- |
| ID                    | yes                       | —                                |
| Title                 | yes                       | —                                |
| Description           | yes                       | —                                |
| Status                | yes                       | one of four allowed statuses     |
| Priority              | yes as displayed property | `Not Set` before priority is set |
| Assignee              | conditional               | absent if no assignee            |
| Author                | yes                       | —                                |
| Created at            | yes                       | —                                |
| Comment text          | conditional               | no comment if none exists        |
| Comment author        | conditional               | shown for stored comment         |
| Comment creation time | conditional               | shown for stored comment         |



## 6.2 Controls



### Assignment



| C-ID        | Label    | Type          | Condition                                    | Trigger     | Result                       | Source/D-ID |
| ----------- | -------- | ------------- | -------------------------------------------- | ----------- | ---------------------------- | ----------- |
| **C-03-01** | Assignee | select        | Team Lead: enabled; non-Lead: disabled       | select user | selected value pending Apply | US-03, D-05 |
| **C-03-02** | Apply    | action button | enabled when valid assignee selection exists | click       | assignee changes; stays S-03 | D-06        |



Assignee choices may include **all three predefined users**, including Team Lead.



Only Team Lead can assign.



### Priority



| C-ID        | Label    | Type          | Condition                              | Trigger                         | Result                       | Source/D-ID                |
| ----------- | -------- | ------------- | -------------------------------------- | ------------------------------- | ---------------------------- | -------------------------- |
| **C-03-03** | Priority | select        | Team Lead: enabled; non-Lead: disabled | select Low/Normal/High/Critical | selected value pending Apply | US-04, BR-05–06, D-03/D-05 |
| **C-03-04** | Apply    | action button | after valid selection                  | click                           | priority changes; stays S-03 | D-06                       |



Available set:



`Low | Normal | High | Critical`



Displayed missing value:



`Not Set`



`Not Set` is **not** one of the post-selection priority values and no reset is provided.



### Status



| C-ID        | Label  | Type           | Condition                                                                      | Trigger                    | Result                            | Source/D-ID               |
| ----------- | ------ | -------------- | ------------------------------------------------------------------------------ | -------------------------- | --------------------------------- | ------------------------- |
| **C-03-05** | Status | select/control | enabled only for responsible assignee and only when outgoing transition exists | select allowed destination | selected transition pending Apply | US-07, BR-08.1, DEF-05/06 |
| **C-03-06** | Apply  | action button  | after valid allowed destination selected                                       | click                      | status changes; stays S-03        | D-06                      |



The Status control offers **only allowed destinations from the current status**.



Therefore:



| Current     | Available destinations |
| ----------- | ---------------------- |
| Open        | In Progress            |
| In Progress | Resolved               |
| Resolved    | In Progress, Closed    |
| Closed      | none                   |



Special cases:



* non-assignee → status unavailable;

* Team Lead non-assignee → status unavailable;

* issue without assignee → status unavailable to everyone;

* Closed → no outgoing status action.



Team Lead does **not** receive status permission merely from being Team Lead.



### Comment



| C-ID        | Label           | Type              | Condition                                   | Trigger | Result                                                  | Source/D-ID        |
| ----------- | --------------- | ----------------- | ------------------------------------------- | ------- | ------------------------------------------------------- | ------------------ |
| **C-03-07** | Comment         | text input        | enabled for any authenticated team member   | type    | comment text entered                                    | BR-02, US-06       |
| **C-03-08** | Add Comment     | button            | available for any authenticated team member | click   | success → stored comment; failure → error; remains S-03 | US-06, DEF-02      |
| **C-03-09** | Comment display | content component | for stored comments                         | render  | text + author + creation time visible                   | US-06 AC-06.2–06.4 |



### Back



| C-ID        | Label | Type       | Condition | Trigger | Result                            | Source/D-ID |
| ----------- | ----- | ---------- | --------- | ------- | --------------------------------- | ----------- |
| **C-03-10** | Back  | navigation | visible   | click   | `T-24` → previous S-02 list state | D-07        |



---



# 7. S-03 state matrix



| Context                  | View | Create | Comment |  Assign | Priority |  Status |
| ------------------------ | ---: | -----: | ------: | ------: | -------: | ------: |
| Team Member non-assignee |  Yes |    Yes |     Yes |      No |       No |      No |
| Team Member assignee     |  Yes |    Yes |     Yes |      No |       No | **Yes** |
| Team Lead non-assignee   |  Yes |    Yes |     Yes | **Yes** |  **Yes** |      No |
| Team Lead assignee       |  Yes |    Yes |     Yes | **Yes** |  **Yes** | **Yes** |



Important:



* **Team Lead ≠ automatically status-authorized.**

* Status depends on **assignee relation**.

* `Assignee = current user` is what gives the status permission, subject to BR-08.1.

* If issue has no assignee, status is unavailable to everyone.

* Create and Comment are not restricted to non-Lead users.



---



# 8. Permission Matrix



| Action              | TM non-assignee | TM assignee | Lead non-assignee | Lead assignee |
| ------------------- | --------------: | ----------: | ----------------: | ------------: |
| **View**            |             Yes |         Yes |               Yes |           Yes |
| **Create**          |             Yes |         Yes |               Yes |           Yes |
| **Comment**         |             Yes |         Yes |               Yes |           Yes |
| **Assign**          |              No |          No |               Yes |           Yes |
| **Change priority** |              No |          No |               Yes |           Yes |
| **Change status**   |              No |     **Yes** |                No |       **Yes** |



This matrix is per issue. `Assignee` is not a fifth role.



---



# 9. Full status transition matrix



Only BR-08.1 transitions are allowed.



| Current status  |      Open | In Progress |    Resolved |      Closed |
| --------------- | --------: | ----------: | ----------: | ----------: |
| **Open**        |         — | **Allowed** |   Forbidden |   Forbidden |
| **In Progress** | Forbidden |           — | **Allowed** |   Forbidden |
| **Resolved**    | Forbidden | **Allowed** |           — | **Allowed** |
| **Closed**      | Forbidden |   Forbidden |   Forbidden |           — |



`—` means **same current status, not a transition**.



### Resulting control behavior



* Open → offer only **In Progress**.

* In Progress → offer only **Resolved**.

* Resolved → offer **In Progress** and **Closed**.

* Closed → offer **no next status**.



A forbidden destination should not be offered by the Status control.



This is a control/state contract, not an additional transition.



---



# 10. Important verification examples



| Example                | Predicate evaluation                                       | Result            |
| ---------------------- | ---------------------------------------------------------- | ----------------- |
| High + Resolved        | High ∈ {High,Critical}; Resolved ≠ Closed                  | **Important**     |
| Critical + Closed      | Critical qualifies, but Closed violates `status != Closed` | **Not Important** |
| Not Set + Open         | Not Set ∉ {High,Critical}                                  | **Not Important** |
| Normal + Resolved      | Normal not in set                                          | **Not Important** |
| Critical + In Progress | Critical qualifies and status ≠ Closed                     | **Important**     |



---



# 11. Comments contract



A stored comment must preserve/display:



1. comment text;

2. comment author;

3. comment creation date/time.



### Success



`C-03-07 → C-03-08 → C-03-09`



Comment appears on S-03.



### Empty comment



Comment text empty:



* no comment created;

* validation state remains on S-03.



### Failed save



Save failure:



* no comment is created;

* error is shown;

* user remains on S-03.



Exact error text remains **UNRESOLVED Q-06**.



---



# 12. REF-01 — compact verification contexts



REF-01 is **not a product screen and not an additional Figma frame**.



It is a compact verification set:



**4 contexts × 4 statuses = 16 examples**



Contexts:



1. Team Member non-assignee;

2. Team Member assignee;

3. Team Lead non-assignee;

4. Team Lead assignee.



Statuses:



1. Open;

2. In Progress;

3. Resolved;

4. Closed.



Purpose: verify status-control availability and role/assignee relationship without creating 16 full screens.



The same principle applies to fixture examples for Important predicate.



---



# 13. Developer-verifiable tests



## S-01 / authentication



### TEST-01



**Actor/context:** authenticated Team Member

**Precondition:** authentication already completed

**Action:** enter MVP demonstration

**Expected visible result:** S-02-A

**Cannot prove:** actual authentication implementation.



### TEST-02



**Actor/context:** unauthenticated context

**Precondition:** no authentication

**Action:** attempt access

**Expected visible result:** access must be controlled by authentication boundary

**Cannot prove:** exact login mechanism or authentication implementation.



---



## Create



### TEST-03



**Actor/context:** Team Member

**Precondition:** authenticated; S-02-A

**Action:** Create → enter valid Title + Description → Create

**Expected:** S-03 for newly created issue; Open; Not Set; author=current user; ID/created_at present; no assignee

**Cannot prove:** backend persistence implementation.



### TEST-04



**Actor/context:** Team Lead

**Precondition:** authenticated; S-02-A

**Action:** Create issue

**Expected:** Create available and same creation behavior as Team Member

**Cannot prove:** role authorization implementation outside prototype.



### TEST-05



**Actor/context:** authenticated Team Member

**Precondition:** M-01 open

**Action:** Create with empty Title

**Expected:** no issue created; `Title is required`

**Cannot prove:** server-side validation.



### TEST-06



**Actor/context:** authenticated Team Member

**Precondition:** M-01 open

**Action:** Create with empty Description

**Expected:** no issue created

**Cannot prove:** exact Description error copy because source does not specify it.



### TEST-07



**Actor/context:** authenticated Team Member

**Precondition:** M-01 open from S-02-B

**Action:** Cancel

**Expected:** previous list state remains S-02-B

**Cannot prove:** persistent browser/navigation implementation.



---



## Issue viewing



### TEST-08



**Actor/context:** authenticated Team Member

**Precondition:** issue exists

**Action:** select issue row

**Expected:** S-03 shows ID, Title, Description, Status, Priority, Assignee if assigned, Author, Created at

**Cannot prove:** actual data retrieval.



### TEST-09



**Actor/context:** authenticated Team Lead

**Precondition:** issue exists

**Action:** select issue

**Expected:** same issue details are viewable

**Cannot prove:** production authorization middleware.



---



## Assignment



### TEST-10



**Actor/context:** Team Lead

**Precondition:** S-03; issue exists

**Action:** select any of three predefined users → Apply

**Expected:** selected user displayed as current assignee; remains S-03

**Cannot prove:** actual persistence.



### TEST-11



**Actor/context:** Team Member

**Precondition:** S-03

**Action:** inspect assignment control

**Expected:** assignment control visible but disabled

**Cannot prove:** backend authorization.



---



## Priority



### TEST-12



**Actor/context:** Team Lead

**Precondition:** S-03; priority currently Not Set

**Action:** select Low/Normal/High/Critical → Apply

**Expected:** selected priority displayed; remains S-03

**Cannot prove:** backend persistence.



### TEST-13



**Actor/context:** Team Member

**Precondition:** S-03

**Action:** inspect priority control

**Expected:** control disabled

**Cannot prove:** backend authorization.



### TEST-14



**Actor/context:** Team Lead

**Precondition:** priority already set

**Action:** inspect available values

**Expected:** Low/Normal/High/Critical; no reset-to-Not-Set action

**Cannot prove:** database constraint.



---



## Status



### TEST-15



**Actor/context:** Team Member assignee

**Precondition:** issue Open

**Action:** inspect Status control

**Expected:** only In Progress is offered

**Cannot prove:** server-side transition enforcement.



### TEST-16



**Actor/context:** Team Member assignee

**Precondition:** issue In Progress

**Action:** inspect Status control

**Expected:** only Resolved is offered

**Cannot prove:** server-side transition enforcement.



### TEST-17



**Actor/context:** Team Member assignee

**Precondition:** issue Resolved

**Action:** inspect Status control

**Expected:** In Progress and Closed offered

**Cannot prove:** server-side transition enforcement.



### TEST-18



**Actor/context:** Team Member assignee

**Precondition:** issue Closed

**Action:** inspect Status control

**Expected:** no next status action

**Cannot prove:** backend enforcement.



### TEST-19



**Actor/context:** Team Member non-assignee

**Precondition:** issue assigned to another user

**Action:** inspect Status control

**Expected:** status unavailable/disabled

**Cannot prove:** authorization implementation.



### TEST-20



**Actor/context:** Team Lead non-assignee

**Precondition:** issue assigned to another user

**Action:** inspect Status control

**Expected:** status unavailable/disabled despite Lead role

**Cannot prove:** backend authorization.



### TEST-21



**Actor/context:** any authenticated user

**Precondition:** issue has no assignee

**Action:** inspect Status control

**Expected:** status unavailable to everyone

**Cannot prove:** backend enforcement.



### TEST-22



**Actor/context:** Team Member assignee

**Precondition:** issue Resolved

**Action:** choose In Progress → Apply

**Expected:** issue becomes In Progress; remains S-03

**Cannot prove:** persistence beyond prototype.



### TEST-23



**Actor/context:** Team Member assignee

**Precondition:** issue Resolved

**Action:** choose Closed → Apply

**Expected:** issue becomes Closed; remains S-03

**Cannot prove:** actual Important-list recalculation.



---



## Comments



### TEST-24



**Actor/context:** Team Member non-assignee

**Precondition:** S-03

**Action:** enter non-empty comment → Add Comment

**Expected:** comment shown with text, author and creation time

**Cannot prove:** persistence implementation.



### TEST-25



**Actor/context:** Team Lead

**Precondition:** S-03

**Action:** enter non-empty comment → Add Comment

**Expected:** comment can be added

**Cannot prove:** backend authorization.



### TEST-26



**Actor/context:** authenticated team member

**Precondition:** empty comment field

**Action:** Add Comment

**Expected:** no comment created; validation state

**Cannot prove:** exact error copy if not specified by source.



### TEST-27



**Actor/context:** authenticated team member

**Precondition:** simulated save failure

**Action:** Add Comment

**Expected:** comment not created; error visible; remain S-03

**Cannot prove:** actual failure conditions or backend error handling.



---



## Important



### TEST-28



**Actor/context:** Team Member

**Precondition:** issue High + Resolved

**Action:** open Important

**Expected:** issue appears

**Cannot prove:** actual production filtering.



### TEST-29



**Actor/context:** Team Member

**Precondition:** issue Critical + Closed

**Action:** open Important

**Expected:** issue does not appear

**Cannot prove:** actual production filtering.



### TEST-30



**Actor/context:** Team Member

**Precondition:** issue Not Set + Open

**Action:** open Important

**Expected:** issue does not appear

**Cannot prove:** actual database query.



---



## Empty state / navigation



### TEST-31



**Actor/context:** authenticated user

**Precondition:** no issues

**Action:** open All

**Expected:** S-02-C

**Cannot prove:** actual data source.



### TEST-32



**Actor/context:** authenticated user

**Precondition:** previously on S-02-B

**Action:** open issue → S-03 → Back

**Expected:** return to S-02-B

**Cannot prove:** browser-level navigation implementation.



---



# 14. Traceability



| Source ID / пункт                    | UIR-ID | Screen/State      | C-ID / annotation            | Test ID           | Verification level                |
| ------------------------------------ | ------ | ----------------- | ---------------------------- | ----------------- | --------------------------------- |
| US-01 AC-01.1                        | UIR-01 | M-01              | C-01-01, C-01-02             | TEST-03           | Mock + Prototype + Implementation |
| US-01 AC-01.2                        | UIR-02 | M-01              | C-01-01                      | TEST-05           | Mock + Prototype + Implementation |
| US-01 AC-01.3                        | UIR-03 | M-01              | C-01-02                      | TEST-06           | Mock + Prototype + Implementation |
| US-01 AC-01.4                        | UIR-04 | S-03              | issue data annotation        | TEST-03           | Prototype + Implementation        |
| US-01 AC-01.5                        | UIR-05 | M-01/S-03         | Priority annotation          | TEST-03           | Mock + Prototype + Implementation |
| US-02 AC-02.1                        | UIR-06 | S-02-A/B          | list annotation              | TEST-08           | Mock + Prototype + Implementation |
| US-02 AC-02.2                        | UIR-07 | S-02-A/B          | list fields                  | TEST-08           | Mock + Prototype + Implementation |
| US-02 AC-02.3                        | UIR-08 | S-02-A/B          | C-02-04                      | TEST-08           | Mock + Prototype                  |
| US-02 AC-02.4                        | UIR-09 | S-03              | visible-data contract        | TEST-08           | Mock + Prototype + Implementation |
| US-02 AC-02.5                        | UIR-10 | S-02-C            | empty-state annotation       | TEST-31           | Mock + Prototype + Implementation |
| US-03 — choose one assignee          | UIR-11 | S-03              | C-03-01                      | TEST-10           | Mock + Prototype + Implementation |
| US-03 — assignee must be team member | UIR-12 | S-03              | C-03-01                      | TEST-10           | Prototype + Implementation        |
| US-03 — only Team Lead can assign    | UIR-13 | S-03              | C-03-01                      | TEST-11           | Mock + Prototype + Implementation |
| US-03 — current assignee displayed   | UIR-14 | S-03              | Assignee display             | TEST-10           | Mock + Prototype + Implementation |
| US-04 AC-04.1                        | UIR-15 | S-03              | C-03-03                      | TEST-12           | Mock + Prototype + Implementation |
| US-04 AC-04.2                        | UIR-16 | S-03              | C-03-03                      | TEST-12/13        | Mock + Prototype + Implementation |
| US-05 AC-05.1                        | UIR-17 | S-02-A/B          | Status field                 | TEST-08           | Mock + Prototype + Implementation |
| US-05 AC-05.2                        | UIR-18 | S-03              | Status field                 | TEST-15–23        | Mock + Prototype + Implementation |
| US-05 AC-05.3                        | UIR-19 | S-02-A/B          | Status field                 | TEST-08           | Mock + Prototype + Implementation |
| US-06 AC-06.1                        | UIR-20 | S-03              | C-03-07/08                   | TEST-26           | Mock + Prototype + Implementation |
| US-06 AC-06.2                        | UIR-21 | S-03              | C-03-09                      | TEST-24           | Mock + Prototype + Implementation |
| US-06 AC-06.3                        | UIR-22 | S-03              | C-03-09                      | TEST-24           | Mock + Prototype + Implementation |
| US-06 AC-06.4                        | UIR-23 | S-03              | C-03-09                      | TEST-24           | Mock + Prototype + Implementation |
| US-06 AC-06.5                        | UIR-24 | S-03              | error annotation             | TEST-27           | Prototype + Implementation        |
| US-07 AC-07.1                        | UIR-25 | M-01/S-03         | Status annotation            | TEST-03           | Mock + Prototype + Implementation |
| US-07 AC-07.2                        | UIR-26 | S-03              | C-03-05                      | TEST-15–18        | Mock + Prototype + Implementation |
| US-07 AC-07.3                        | UIR-27 | S-03              | C-03-06                      | TEST-22/23        | Prototype + Implementation        |
| US-07 AC-07.4                        | UIR-28 | S-03              | C-03-05                      | TEST-19/20        | Mock + Prototype + Implementation |
| US-07 AC-07.5                        | UIR-29 | S-03              | allowed-destination rule     | TEST-15–18        | Mock + Prototype + Implementation |
| US-08 AC-08.1                        | UIR-30 | S-02-B            | Important predicate          | TEST-28           | Prototype + Implementation        |
| US-08 AC-08.2                        | UIR-31 | S-02-B            | Important predicate          | TEST-28           | Prototype + Implementation        |
| US-08 AC-08.3                        | UIR-32 | S-02-B            | Important predicate          | TEST-29/30        | Prototype + Implementation        |
| US-08 AC-08.4                        | UIR-33 | S-02-B            | Important predicate          | TEST-29           | Prototype + Implementation        |
| BR-01                                | UIR-34 | S-01/S-02         | auth boundary                | TEST-01/02        | Prototype + Implementation        |
| BR-02                                | UIR-35 | M-01/S-03         | Create/Comment controls      | TEST-03/04/24/25  | Mock + Prototype + Implementation |
| BR-03                                | UIR-36 | S-03              | status annotation            | TEST-03           | Mock + Prototype + Implementation |
| BR-04                                | UIR-37 | S-03              | Priority = Not Set           | TEST-03/14        | Mock + Prototype + Implementation |
| BR-05                                | UIR-38 | S-03              | C-03-03                      | TEST-12/14        | Mock + Prototype + Implementation |
| BR-06                                | UIR-39 | S-03              | C-03-03                      | TEST-12/13        | Mock + Prototype + Implementation |
| BR-07                                | UIR-40 | S-03              | C-03-05                      | TEST-15–18        | Mock + Prototype + Implementation |
| BR-08                                | UIR-41 | S-03              | C-03-05                      | TEST-15–23        | Mock + Prototype + Implementation |
| BR-08.1                              | UIR-42 | S-03              | transition matrix            | TEST-15–18/22/23  | Mock + Prototype + Implementation |
| BR-09                                | UIR-43 | S-02-B            | Important predicate          | TEST-28–30        | Prototype + Implementation        |
| BR-09.1                              | UIR-44 | S-02-B/S-03       | Resolved ≠ Closed            | TEST-28/29        | Prototype + Implementation        |
| BR-10                                | UIR-45 | S-03              | no Delete control annotation | —                 | Mock + Prototype + Implementation |
| BR-11                                | UIR-46 | S-03              | no History UI annotation     | —                 | Mock + Prototype                  |
| NFR-02                               | UIR-47 | S-01              | auth boundary                | TEST-01/02        | Implementation                    |
| D-01                                 | UIR-48 | S-02/M-01         | tabs/Create/modal            | TEST-03/07        | Mock + Prototype                  |
| D-02                                 | UIR-49 | S-01              | boundary annotation          | TEST-01           | Mock + Prototype                  |
| D-03                                 | UIR-50 | S-02/M-01/S-03    | Not Set                      | TEST-03/14/30     | Mock + Prototype                  |
| D-04                                 | UIR-51 | S-03              | C-03-01                      | TEST-10           | Mock + Prototype                  |
| D-05                                 | UIR-52 | S-03              | disabled controls            | TEST-11/13/19/20  | Mock + Prototype                  |
| D-06                                 | UIR-53 | S-03              | Apply controls               | TEST-10/12/22/23  | Mock + Prototype                  |
| D-07                                 | UIR-54 | S-03              | C-03-10                      | TEST-32           | Prototype                         |
| D-08                                 | UIR-55 | all demo surfaces | fixture annotation           | all fixture tests | Mock + Prototype                  |



**Note:** US-03 deliberately preserves the original source's **unnumbered acceptance points** rather than inventing AC-03.x IDs.



---



# 15. Business decisions still required



These remain unresolved and are not silently converted into requirements:



### Q-02 — Important wording



The US-08 title contains “Open”, while AC-08.1/08.2 and BR-09.1 establish that **Resolved remains Important until Closed**.



For G3 contract the exact predicate is:



`priority ∈ {High, Critical} AND status != Closed`



This is the interpretation required by the existing AC/BR.



### Q-03 — forbidden transition notification



D-05 defines disabled controls for role-forbidden actions. The exact treatment/message for an attempted forbidden transition is still unresolved.



G3 therefore does **not** introduce a new notification requirement.



### Q-06 — comment save error text



The source requires an error after failed save, but does not define exact copy.



G3 verifies the existence of the error state, not a mandatory wording.



### Q-07 — empty Important



S-02-C is specifically **empty All Issues**.



A dedicated empty Important representation is:



`PROPOSED D-ID: P-D-01`



It is UX only and does not become a source requirement.



### D-04 teacher confirmation note



The source US-03 supports assignment to a team member and display of the current assignee, while the exact post-create/reassignment behavior is captured by the approved design decision D-04. It should not be rewritten as a new source AC.



---



# 16. G3 verification boundary



### Can be verified in the mock



* required visible fields;

* list/detail information architecture;

* Not Set representation;

* allowed priority values;

* role-dependent disabled controls;

* Create/Comment availability;

* status control showing only allowed destinations;

* no status control for non-assignee / unassigned issue;

* no outgoing status control for Closed;

* Important examples;

* All empty state;

* absence of Delete/History/Attachment surfaces;

* desktop grayscale low-fidelity direction.



### Can be verified in the prototype



* S-02 → M-01;

* successful Create → **S-03 new issue only**;

* validation states;

* S-02 → S-03;

* S-03 → previous S-02 state;

* Apply interactions remain on S-03;

* allowed status transitions;

* Resolved → In Progress;

* Resolved → Closed;

* Important filtering examples;

* comment success/error states.



### Can be verified only in implementation



* actual authentication enforcement;

* real authorization;

* actual persistence;

* generated IDs;

* author/current-user identity;

* timestamps;

* database status/priority/assignee values;

* actual transition enforcement;

* actual comment-save failure behavior;

* actual filtering/query behavior;

* response-time and HTTPS/backup NFRs.



No future mock/prototype result should be marked **PASS** before the corresponding artifact exists and is actually checked.



**G3-важливий результат:** після DEF-02…DEF-06 архітектура все ще залишається мінімальною: **S-01 + S-02 + M-01 + S-03**, без нових продуктових screens. `REF-01` — лише компактний verification context, а не 16 додаткових frames.




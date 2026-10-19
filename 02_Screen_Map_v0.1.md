Нижче — мінімальна архітектура Release MVP, побудована **лише на останньому Input Pack і погоджених D-01…D-08**. Deferred US-09 та Future SR-10/SR-11 не включаю.



## 1. Мінімальна система surfaces/states



| Screen/State ID | Name                    | Purpose                                                                 | Actors                               | Entry                                            | Visible data                                                                                 | Inputs                                                                 | Actions                                                         | Role/state conditions                                                                                                                                                                                                                                                                                                              | Errors/empty states                                                                                                                                                                                                                     | Exit                                                                                                                      | Source/D-ID                                                                                                                  |
| --------------- | ----------------------- | ----------------------------------------------------------------------- | ------------------------------------ | ------------------------------------------------ | -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **S-01**        | Authentication boundary | Позначити межу автентифікації без проєктування login screen             | Authenticated Team Member, Team Lead | Перед демонстрацією MVP                          | Лише annotation: користувач уже authenticated                                                | —                                                                      | Увійти в MVP після authentication boundary                      | Authentication required; механізм login не визначений                                                                                                                                                                                                                                                                              | Не проєктується                                                                                                                                                                                                                         | → S-02                                                                                                                    | BR-01, NFR-02, **D-02**                                                                                                      |
| **S-02-A**      | Issues List — All       | Перегляд усіх issues                                                    | Team Member, Team Lead               | Після S-01; також повернення з S-03              | ID, Title, Status, Priority, Assignee якщо assigned                                          | Вибір issue; tab All                                                   | Open issue; перейти Important; відкрити M-01 Create             | All — основний list state                                                                                                                                                                                                                                                                                                          | Якщо issues немає → S-02-C                                                                                                                                                                                                              | → S-03 / M-01 / S-02-B                                                                                                    | US-02 AC-02.1–02.3; **D-01, D-07**                                                                                           |
| **S-02-B**      | Issues List — Important | Перегляд Important issues                                               | Team Member, Team Lead               | Вибір tab Important із S-02                      | ID, Title, Status, Priority, Assignee якщо assigned                                          | Вибір issue; tab All                                                   | Open issue; перейти All; відкрити M-01 Create                   | Important = High/Critical і Status ≠ Closed; Low/Normal не Important; Closed не Important                                                                                                                                                                                                                                          | Якщо відповідних issues немає → S-02-C                                                                                                                                                                                                  | → S-03 / M-01 / S-02-A                                                                                                    | US-08 AC-08.1–08.4; BR-09, BR-09.1; **D-01, D-07**                                                                           |
| **S-02-C**      | Issues List — Empty     | Представити порожній результат списку                                   | Team Member, Team Lead               | S-02-A або S-02-B, коли немає відповідних issues | Empty state / message                                                                        | —                                                                      | Перехід між All / Important; Create                             | Для All — немає issues; для Important — немає Important issues                                                                                                                                                                                                                                                                     | Це окремий empty state, не окрема сторінка                                                                                                                                                                                              | → S-02-A / S-02-B / M-01                                                                                                  | US-02 AC-02.5; **D-01**; Q-07 залишається unresolved щодо точного тексту/варіанта                                            |
| **M-01**        | Create Issue modal      | Створити новий issue без окремої Create page                            | Authenticated Team Member            | Create із S-02                                   | Title, Description                                                                           | Title, Description                                                     | Create; Cancel/close                                            | Title required; Description required; після створення status=Open, priority=Not Set, author=current user, ID/created_at generated                                                                                                                                                                                                  | Порожній Title → issue не створюється, **“Title is required”**. Аналогічна вимога існує для Description, але точний error text source не заданий                                                                                        | Cancel → попередній S-02 state; successful Create → S-03 або відповідний list state залежно від демонстраційного сценарію | US-01 AC-01.1–01.5; BR-02–04; **D-03, D-08**                                                                                 |
| **S-03**        | Issue Details           | Спільні details для перегляду, assignment, priority, status та comments | Team Member, Team Lead               | Вибір issue із S-02                              | ID, Title, Description, Status, Priority, Assignee if assigned, Author, Created at; comments | Comment text; assignee selection; priority selection; status selection | Add comment; Apply assignee; Apply priority; Apply status; Back | Team Lead: assignment/priority controls доступні. Інший користувач: forbidden role-dependent controls **visible disabled**, not hidden. Status change тільки відповідальним assignee. Priority: Not Set або Low/Normal/High/Critical, reset не передбачений. Status: Open/In Progress/Resolved/Closed і лише дозволені transitions | Empty comment → не зберігати. Failed save → comment не створюється + error. Forbidden action → disabled control, переходу немає. Недозволений status transition → action unavailable/closed by D-05; exact notification unresolved Q-03 | Back → **попередній S-02 list state**; successful Apply залишається S-03                                                  | US-02 AC-02.4; US-03; US-04 AC-04.1–04.2; US-05; US-06 AC-06.1–06.5; US-07 AC-07.1–07.5; BR-05–08.1, BR-10–11; **D-03–D-07** |



### Важливе архітектурне рішення



**S-03 є спільним detail surface**, а не набором окремих сторінок:



* Assignment — control на S-03;

* Priority — control на S-03;

* Status — control на S-03;

* Comments — section/component на S-03.



Тобто **не створюються** `Assignment Screen`, `Priority Screen`, `Status Screen` або `Comments Screen`.



---



# 2. Що є screen, state, modal і component state



Щоб не змішувати рівні:



* **Screen:** S-01, S-02, S-03.

* **States одного screen:** S-02-A All, S-02-B Important, S-02-C Empty.

* **Modal/overlay:** M-01 Create Issue.

* **Component states:** disabled role-dependent controls на S-03; success/error стан збереження comment; availability конкретного status transition.

* **Service/annotation:** S-01 — лише boundary/placeholder, а не вигаданий login screen.



Особливо важливо: **S-02-C не є окремим продуктовим екраном** — це empty state S-02.



---



# 3. Таблиця переходів



| T-ID     | From     | Actor                   | Trigger                               | Preconditions                                | To                                | Visible result                                   | Requirement/D-ID                             |
| -------- | -------- | ----------------------- | ------------------------------------- | -------------------------------------------- | --------------------------------- | ------------------------------------------------ | -------------------------------------------- |
| **T-01** | S-01     | Authenticated user      | Authentication boundary completed     | User authenticated                           | S-02-A                            | All issues list                                  | BR-01, NFR-02, **D-02**                      |
| **T-02** | S-02-A   | Team Member / Team Lead | Click issue                           | Issue exists                                 | S-03                              | Issue details                                    | US-02 AC-02.3–02.4                           |
| **T-03** | S-02-B   | Team Member / Team Lead | Click issue                           | Important issue exists                       | S-03                              | Important issue details                          | US-08, BR-09                                 |
| **T-04** | S-02-A/B | Team Member             | Click Create                          | Authenticated                                | M-01                              | Create modal                                     | US-01, BR-02, **D-01**                       |
| **T-05** | M-01     | Team Member             | Create                                | Title + Description non-empty                | S-03 / corresponding list context | New issue: ID, Open, Not Set, author, created_at | US-01 AC-01.1–01.5; BR-03–04; **D-03**       |
| **T-06** | M-01     | Team Member             | Create                                | Title empty                                  | M-01                              | Issue not created; Title required error          | US-01 AC-01.2; negative BDD                  |
| **T-07** | M-01     | Team Member             | Create                                | Description empty                            | M-01                              | Issue not created; Description validation        | US-01 AC-01.3                                |
| **T-08** | M-01     | Team Member             | Cancel/close                          | Modal open                                   | Previous S-02 state               | List preserved                                   | **D-01, D-07**                               |
| **T-09** | S-03     | Team Lead               | Select assignee → Apply               | Selected assignee is a Team Member           | S-03                              | Current assignee displayed                       | US-03; **D-04, D-06**                        |
| **T-10** | S-03     | Non-Team Lead           | Attempt assignment                    | User lacks Team Lead role                    | S-03                              | Assignment control disabled; no change           | US-03; **D-05**                              |
| **T-11** | S-03     | Team Lead               | Select priority → Apply               | Selected value Low/Normal/High/Critical      | S-03                              | New priority displayed                           | US-04 AC-04.1–04.2; BR-05–06; **D-03, D-06** |
| **T-12** | S-03     | Non-Team Lead           | Attempt priority change               | User lacks Team Lead role                    | S-03                              | Priority control disabled; no change             | US-04 AC-04.2; **D-05**                      |
| **T-13** | S-03     | Responsible team member | Select Open → In Progress → Apply     | Current status Open; user is assignee        | S-03                              | Status = In Progress                             | US-07 AC-07.2–07.4; BR-08.1; **D-06**        |
| **T-14** | S-03     | Responsible team member | Select In Progress → Resolved → Apply | Current status In Progress; user is assignee | S-03                              | Status = Resolved                                | US-07; BR-08.1; **D-06**                     |
| **T-15** | S-03     | Responsible team member | Select Resolved → Closed → Apply      | Current status Resolved; user is assignee    | S-03                              | Status = Closed                                  | US-07; BR-08.1; **D-06**                     |
| **T-16** | S-03     | Responsible team member | Select Resolved → In Progress → Apply | Current status Resolved; user is assignee    | S-03                              | Status = In Progress; issue active again         | US-07; BR-08.1, BR-09.1; **D-06**            |
| **T-17** | S-03     | Responsible team member | Select prohibited transition → Apply  | Transition not in BR-08.1                    | **S-03; no transition**           | Status remains unchanged                         | US-07 AC-07.5; BR-08.1                       |
| **T-18** | S-03     | Non-assignee            | Attempt status change                 | User is not responsible/assigned member      | **S-03; no transition**           | Status control unavailable/disabled              | US-07 AC-07.4; **D-05**                      |
| **T-19** | S-03     | Team Member             | Enter comment → Add                   | Comment text non-empty; save succeeds        | S-03                              | Comment, author and date/time visible            | US-06 AC-06.1–06.4                           |
| **T-20** | S-03     | Team Member             | Add comment                           | Comment text empty                           | S-03                              | Comment not created; validation                  | US-06 AC-06.1                                |
| **T-21** | S-03     | Team Member             | Add comment                           | Save fails                                   | S-03                              | Comment not created + error                      | US-06 AC-06.5; Q-06                          |
| **T-22** | S-02-A   | Team Member / Team Lead | Click Important                       | —                                            | S-02-B                            | Only Important issues visible                    | US-08; BR-09; **D-01**                       |
| **T-23** | S-02-B   | Team Member / Team Lead | Click All                             | —                                            | S-02-A                            | All issues visible                               | **D-01**                                     |
| **T-24** | S-02-A/B | Team Member / Team Lead | Return/Back from S-03                 | Previous list state recorded                 | Previous S-02 state               | Same list tab/state restored                     | **D-07**                                     |
| **T-25** | S-02-A   | Team Member / Team Lead | List evaluated                        | No issues                                    | S-02-C                            | Empty All state                                  | US-02 AC-02.5; Q-07                          |
| **T-26** | S-02-B   | Team Member / Team Lead | Important evaluated                   | No High/Critical non-Closed issues           | S-02-C                            | Empty Important state                            | US-08; Q-07                                  |



**Ключове правило:** недопустима дія **не створює переходу**. Наприклад, T-10, T-12, T-17, T-18 залишають користувача на S-03.



---



# 4. User flows за сценаріями



### UF-01 — Створення issue



`S-01 → S-02-A → M-01 → S-03`



Очікуваний результат:



* Title і Description заповнені;

* створено issue;

* `Status = Open`;

* `Priority = Not Set`;

* визначені ID, author, created_at.



Якщо Title порожній:



`S-02-A → M-01 → M-01 [validation error]`



Issue **не створюється**.



**Source:** US-01 AC-01.1–01.5, BR-02–04, D-03.



---



### UF-02 — Перегляд issue



`S-02-A → S-03 → S-02-A`



Або:



`S-02-B → S-03 → S-02-B`



При поверненні зберігається попередній list state.



**Source:** US-02 AC-02.1–02.5, D-07.



---



### UF-03 — Team Lead призначає issue



`S-02-A/B → S-03 → select Assignee → Apply → S-03`



Очікуваний стан:



* assignee змінений;

* current assignee відображається на S-03;

* користувач залишається на S-03.



**Source:** US-03, D-04, D-06.



---



### UF-04 — Team Lead встановлює Priority



`S-03 → Priority select → Apply → S-03`



Допустимі значення:



`Not Set → Low / Normal / High / Critical`



Після встановлення priority не передбачено reset назад у `Not Set`.



**Source:** BR-04–06, US-04, D-03, D-06.



---



### UF-05 — Дозволений статусний workflow



Основний шлях:



`Open → In Progress → Resolved → Closed`



Повернення в роботу:



`Resolved → In Progress → Resolved → Closed`



Усі переходи відбуваються на **S-03**, через погоджений механізм:



`Select → Apply`.



**Source:** US-07, BR-08.1, D-06.



---



### UF-06 — Comment success



`S-03 → enter comment → Add → S-03 [comment visible]`



Після успішного збереження видно:



* текст comment;

* author;

* creation date/time.



**Source:** US-06 AC-06.1–06.4.



---



### UF-07 — Comment error



`S-03 → enter comment → Add → S-03 [error]`



При failed save:



* comment **не створюється**;

* користувач отримує error.



Точний текст помилки поки не визначений.



**Source:** US-06 AC-06.5; Q-06.



---



### UF-08 — Important issues



`S-02-A → Important → S-02-B`



У S-02-B потрапляють:



* High + status ≠ Closed;

* Critical + status ≠ Closed.



Зокрема **Resolved залишається Important**, якщо priority High/Critical, оскільки Resolved ≠ Closed.



`S-02-B → All → S-02-A`



**Source:** US-08 AC-08.1–08.4, BR-09, BR-09.1, D-01.



---



### UF-09 — Заборонена дія



Наприклад, Team Member відкриває:



`S-02-A → S-03`



але не має права змінювати priority.



Control:



`Priority = disabled`



Користувач не може виконати зміну, тому:



`S-03 → S-03`



**без transition** і без зміни даних.



Те саме застосовується до assignment для не-Team Lead та status для не-assignee.



**Source:** US-03, US-04, US-07 AC-07.4, D-05.



---



### UF-10 — Empty list



Для All:



`S-02-A → S-02-C`



якщо немає issues.



Для Important:



`S-02-B → S-02-C`



якщо немає Important issues.



`S-02-C → S-02-A/B`



через відповідний tab.



**Source:** US-02 AC-02.5, US-08, Q-07.



---



# 5. Figma frames ≠ кількість продуктових screens



Для демонстрації мінімального варіанта я б заклав **6 Figma frames**:



1. **S-01** — Authentication boundary placeholder.

2. **S-02-A** — Issues List / All.

3. **S-02-B** — Issues List / Important.

4. **S-02-C** — Empty state.

5. **M-01** — Create modal у контексті list.

6. **S-03** — Issue Details.



При цьому продуктових screens фактично **3**:



* S-01 boundary;

* S-02 Issues List;

* S-03 Issue Details.



`M-01` — modal, а `S-02-A/B/C` — states одного screen.



Для демонстрації role-dependent disabled controls, comment error та різних status transitions **не обов'язково створювати нові Figma frames**: це можуть бути component/state variants або annotation на S-03.



---



# 6. Що НЕ слід додавати



Навіть якщо це типовий функціонал issue tracker:



1. **Окрему сторінку Login / Sign In.**

&#x20;  S-01 погоджений саме як authentication boundary. Механізм authentication не заданий.



2. **Окремі сторінки Assignment, Priority, Status або Comments.**

&#x20;  Усі ці controls належать до S-03.



3. **History / Activity Log / audit trail.**

&#x20;  BR-11 прямо встановлює, що окремої user-facing change history у v1 немає.



Також не слід додавати deletion, file attachments, AI automation чи інші функції з Deferred/Future scope.



---



# 7. Мінімальний Screen Inventory



| ID         | Type              | Назва                   | Figma   |
| ---------- | ----------------- | ----------------------- | ------- |
| **S-01**   | Screen / boundary | Authentication boundary | 1 frame |
| **S-02-A** | Screen state      | Issues List — All       | 1 frame |
| **S-02-B** | Screen state      | Issues List — Important | 1 frame |
| **S-02-C** | Screen state      | Issues List — Empty     | 1 frame |
| **M-01**   | Modal/overlay     | Create Issue            | 1 frame |
| **S-03**   | Screen            | Issue Details           | 1 frame |



**Разом:** 3 основні screens + 1 modal + 3 list states; **6 демонстраційних Figma frames** у мінімальному варіанті.



---



# 8. Unresolved рішення



Вони **не блокують базову архітектуру**:



* **Q-02:** слово `Open` у назві US-08 суперечить ширшій логіці AC/BR; для flow використано AC-08.1/08.2 + BR-09.1, де Resolved до Closed залишається Important за відповідної priority.

* **Q-03:** точний спосіб notification про заборонений transition не визначений; D-05 закриває це на рівні disabled controls.

* **Q-06:** точний текст помилки failed comment save не визначений.

* **Q-07:** точний формат empty state не визначений; тому S-02-C зафіксований як state, але конкретний copy ще unresolved.



Окремо **D-04** погоджене, але в самому source US-03 не деталізує поведінку повторного reassignment настільки явно; це залишається design decision, а не новим source requirement.



---



# 9. Що цей план ще не покриває



План визначає **архітектуру surfaces і переходів**, але ще не визначає:



* конкретний visual layout кожного frame;

* точні тексти всіх labels/placeholders;

* точний empty-state copy;

* точний error copy для failed comment save;

* конкретний спосіб візуального позначення disabled controls;

* деталізацію accessibility;

* responsive/mobile behavior;

* реальну authentication implementation;

* реальну persistence/DB implementation.



Це свідомо залишено поза screen architecture, щоб не перетворювати **PROPOSED** UX-рішення на обов'язкові вимоги.



**Статус:** це мінімальний screen/flow plan на основі погодженого Input Pack і Decision Log; він **не оголошується затвердженим**.




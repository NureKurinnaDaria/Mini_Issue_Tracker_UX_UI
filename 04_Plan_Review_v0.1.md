# 04 — Plan Review (G4)

Date / operator: 06.10.2026 / Daria

Tool: ChatGPT Free, той самий чат; перевірено 03_Screen_Contracts_v0.1 + Addendum



## Відповідь ChatGPT (оригінал)

Примітка: ChatGPT нумерував знахідки DEF-07…DEF-13, але ці ID вже зайняті в Review_Defect_Change_Log.md (CHK-04). Тут вони позначені як G4-F1…G4-F7. Текст нижче не змінено.



Провів повторний аудит із позиції критичного рецензента. **Вихідна `SD_Spec.docx` доступна**, тому відповідність S1 я перевірив за нею, а не за припущеннями. Джерело підтверджує фінальний Release MVP = US-01…US-08, US-09 = Deferred, а також ключові BR-04/05, BR-06, BR-08/08.1, BR-09/09.1, BR-10/11 і NFR-01…04.



## 1. Coverage Matrix — US-01…US-08



Позначення: **COVERED** — у плані є достатня перевірювана відповідність; **AMBIGUOUS** — функціонально покрито, але є невизначеність у трасуванні/формулюванні; **MISSING** — у плані немає достатнього покриття.



| US / AC                                                  | Статус    | Доказ у плані                                                                           |
| -------------------------------------------------------- | --------- | --------------------------------------------------------------------------------------- |
| **US-01 AC-01.1** Title + Description                    | COVERED   | M-01: Title, Description                                                                |
| **AC-01.2** Title required                               | COVERED   | M-01 + T-06; UIR-02                                                                     |
| **AC-01.3** Description required                         | COVERED   | M-01 + T-07; UIR-03                                                                     |
| **AC-01.4** ID, Open, created_at, author                 | COVERED   | T-05 → S-03; S-03 issue metadata                                                        |
| **AC-01.5** priority not set                             | COVERED   | S-03 `Not Set`; D-03; BR-04/05                                                          |
| **US-02 AC-02.1** authenticated member views list        | COVERED   | S-01 → S-02-A; D-02                                                                     |
| **AC-02.2** ID/Title/Status/Priority/Assignee            | COVERED   | S-02-A/B issue row contract                                                             |
| **AC-02.3** select issue → details                       | COVERED   | T-02/T-03 → S-03                                                                        |
| **AC-02.4** detail fields                                | COVERED   | S-03 details contract                                                                   |
| **AC-02.5** empty registered issues                      | COVERED   | S-02-C = Empty All; source permits empty list/message                                   |
| **US-03: one assignee**                                  | AMBIGUOUS | S-03 Assign control + REF-01, але немає окремої numbered AC/source ID                   |
| **US-03: assignee is team member**                       | AMBIGUOUS | REF-01 містить 3 predefined users; D-04 дозволяє Lead як assignee, що є design decision |
| **US-03: only Team Lead assigns**                        | COVERED   | N-01; REF-01; D-05                                                                      |
| **US-03: current assignee displayed**                    | COVERED   | S-03 Assignee; D-04                                                                     |
| **US-04 AC-04.1** Low/Normal/High/Critical               | COVERED   | S-03 Priority; D-03; AC-04.1                                                            |
| **AC-04.2** only Team Lead                               | COVERED   | N-02; REF-01; D-05                                                                      |
| **US-05 AC-05.1** status per issue                       | COVERED   | S-02 row                                                                                |
| **AC-05.2** status on issue page                         | COVERED   | S-03                                                                                    |
| **AC-05.3** status in issue list                         | COVERED   | S-02-A/B                                                                                |
| **US-06 AC-06.1** non-empty comment                      | COVERED   | T-20 / S-03 validation                                                                  |
| **AC-06.2** stored + shown                               | COVERED   | T-19 / S-03 comments                                                                    |
| **AC-06.3** author stored/displayed                      | COVERED   | S-03 comment contract                                                                   |
| **AC-06.4** date/time stored/displayed                   | COVERED   | S-03 comment contract                                                                   |
| **AC-06.5** failed save → no comment + error             | COVERED   | T-21 / S-03 error state; UIR-24                                                         |
| **US-07 AC-07.1** new issue = Open                       | COVERED   | T-05 + S-03                                                                             |
| **AC-07.2** only four statuses                           | COVERED   | REF-01 / S-03 Status                                                                    |
| **AC-07.3** changed status persisted and shown next view | COVERED   | T-13…T-16 + S-03; however mock cannot prove persistence                                 |
| **AC-07.4** non-assignee cannot change                   | COVERED   | N-04/N-05; REF-01; UIR-28                                                               |
| **AC-07.5** outside allowed list prohibited              | COVERED   | N-03 + status destinations; UIR-29                                                      |
| **US-08 AC-08.1** High + status ≠ Closed → Important     | COVERED   | S-02-B; REF-01; UIR-30                                                                  |
| **AC-08.2** Critical + status ≠ Closed                   | COVERED   | S-02-B; REF-01; UIR-31                                                                  |
| **AC-08.3** Low/Normal not Important                     | COVERED   | S-02-B predicate; UIR-32                                                                |
| **AC-08.4** Closed never Important                       | COVERED   | TEST-33 + S-02-B; UIR-33                                                                |



### Висновок по coverage



**Жодного numbered AC US-01…US-08, який був би фактично MISSING, я не знайшов.**



Але є **дві AMBIGUOUS-зони**:



1. **US-03 не має numbered AC у вихідній специфікації.**

&#x20;  Тому не можна штучно називати ці чотири критерії `AC-03.1…03.4`. Їх треба трасувати саме як *US-03 unnumbered criteria*.



2. **AC-07.3 у макеті не доводить реальне persistence.**

&#x20;  План показує, що після Apply S-03 має показувати новий status, але це не є доказом збереження в реальній системі. Для screen/prototype це нормально, якщо статус чітко позначений як **mock/prototype verification**, а не production verification.



Джерело прямо задає AC-07.3 як збереження нового значення і відображення при наступному перегляді; тому не можна представляти один лише mock-state як доказ backend persistence.



---



# 2. Дефекти плану



| DEF-ID     | Severity | Source                  | Plan location        | Проблема                                                                                                                                                                    | Мінімальне виправлення                                                                                                         |
| ---------- | -------- | ----------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **DEF-07** | MEDIUM   | US-03, D-04             | Traceability / US-03 | Чотири критерії US-03 є в S-03/REF-01, але їх не можна залишати без явної source traceability лише тому, що в Spec немає AC-ID.                                             | Додати окремі traceability rows типу `US-03 criterion 1…4`, **без вигаданих AC-ID**.                                           |
| **DEF-08** | MEDIUM   | BR-08, BR-08.1, AC-07.4 | C-03-05 Source       | Попереднє виправлення залишило `US-07 AC-07.4`, хоча погоджене джерельне посилання вимагало `BR-08, BR-08.1, AC-07.4`.                                                      | Замінити Source на **`BR-08, BR-08.1, AC-07.4`**. D-05 залишити лише як джерело disabled presentation, якщо воно там потрібне. |
| **DEF-09** | MEDIUM   | BR-04, BR-05            | S-03 / Priority      | `Not Set` правильно показаний, але це має залишатися саме станом “не встановлено”, а не четвертим значенням Priority.                                                       | Явно позначити `Not Set` як **unset state**, а selectable values — лише Low/Normal/High/Critical.                              |
| **DEF-10** | LOW      | US-03, US-07 AC-07.4    | N-06                 | “Issue без assignee → Status unavailable to anyone” є логічним DERIVED-наслідком, але не дослівним правилом Spec.                                                           | Позначити N-06 як **DERIVED**, а не SOURCE. Не змінювати поведінку.                                                            |
| **DEF-11** | LOW      | BR-08.1                 | N-07 / REF-01        | “Closed → no next status action” випливає з BR-08.1, але в плані це має бути явно source-derived, а не новим правилом UX.                                                   | Позначити як **DERIVED FROM BR-08.1**.                                                                                         |
| **DEF-12** | LOW      | AC-06.5                 | T-21 / S-03          | Точний текст помилки не визначений у Spec. Q-06 це правильно фіксує, але не можна в mock перетворити конкретний invented message на requirement.                            | Залишити generic error або позначити конкретний copy як **PROPOSED**, доки Q-06 не вирішено.                                   |
| **DEF-13** | LOW      | AC-02.5                 | S-02-C               | S-02-C вибирає “empty list” — це допустимо, бо Spec дозволяє **порожній список або повідомлення**. Проблема виникне лише якщо цей варіант буде названий єдино обов'язковим. | Позначити вибір S-02-C як **UX decision / D-01**, а не як джерельну вимогу.                                                    |



### Що **не** є дефектом



* **D-06 Apply beside control** — UX-рішення, не підміна AC.

* **D-05 disabled замість hidden** — погоджене UX-рішення; права при цьому визначаються US/BR.

* **S-02-C** — не зайвий екран: це state одного списку, а не окрема product screen.

* **S-01** — не повноцінний login screen; D-02 прямо визначає його як authentication boundary.

* **S-03** — не дублює Assign/Priority/Status/Comments screens; це одна issue-detail surface.

* **REF-01** — не 16 екранів, а компактна матриця контекстів.

* **BR-10** не означає, що треба показувати Delete control. Навпаки, відсутність Delete — правильна відповідність.

* **BR-11** не означає, що треба створювати History/Activity control. Відсутність такого control правильна.

* **NFR-01/03/04** не повинні ставати UI screens або product controls. Позначення `NOT VERIFIABLE in mock` правильне.

* **US-09 Attachments** не повинен з'являтися в Release MVP. Специфікація чітко встановлює US-09 як Deferred.



---



# 3. Критичні перевірки



### Team Lead non-assignee



**Правильно.**

REF-01 + N-05 не дозволяють Team Lead змінювати status, якщо він не є assignee.



Це важливе розмежування:



> Team Lead має право **Assign/Priority**, але це не означає автоматичного права **Status**.



Status залежить від `assignee`, відповідно до US-07 AC-07.4 / BR-08.



### Create



**Правильно.** M-01 має тільки:



* Title

* Description

* Create

* Cancel/close



Assignee та Priority там бути не повинні.



Після Create → **тільки S-03**, відповідно до D-03.



### Not Set



**Правильно за джерелом.**

Нова issue:



`Priority = Not Set`



а не:



`Priority = Low`



і не окремий selectable priority.



D-03 правильно закриває UX-представлення цього стану.



### Important + Resolved



**Правильно. Це одна з найважливіших перевірок.**



`High + Resolved → Important`



`Critical + Resolved → Important`



оскільки джерело використовує `status != Closed`, а BR-09.1 прямо каже, що Resolved залишається активною до Closed.



Тобто не можна трактувати назву US-08 “Open Issues” буквально всупереч AC-08.1/08.2 і BR-09.1. У плані це зараз враховано.



### Closed



**Правильно.**



`Resolved → Closed`



дозволений.



Після Closed:



* немає наступної status action;

* issue не повинна бути в Important;

* priority не змінює цей факт.



TEST-33 правильно перевіряє саме цю комбінацію.



### Source IDs



Базові джерельні ID реальні:



`US-01…US-08`, `AC-*`, `BR-*`, `NFR-*`, `D-01…D-08`.



`P-D-01` — допустимий **лише як PROPOSED ID**, а не source ID.



`DEF-*`, `N-*`, `TEST-*`, `UIR-*`, `C-*` — внутрішні ID артефакту, їх не можна видавати за ID вихідної специфікації.



---



# 4. Q-ID: що блокує генерацію



| Q-ID     | Статус                                      | Рішення                                                                                                                                                                                                                           |
| -------- | ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Q-02** | Не блокує                                   | AC/BR однозначно сильніші за назву US-08. Resolved + High/Critical входить в Important.                                                                                                                                           |
| **Q-03** | Не блокує                                   | D-05 уже задає disabled presentation. Точний notification/message можна відкласти.                                                                                                                                                |
| **Q-06** | Не блокує                                   | AC-06.5 вимагає сам факт error, але не конкретний copy.                                                                                                                                                                           |
| **Q-07** | Не блокує                                   | AC-02.5 дозволяє empty list **або** message. S-02-C — допустиме UX-рішення.                                                                                                                                                       |
| **Q-04** | **Governance caveat, не технічний blocker** | D-04 закриває Q-04 для плану, але сам D-04 містить примітку “потребує підтвердження викладачем”. Якщо це формальна умова здачі, підтвердження потрібне до фінального затвердження; для самої генерації макету blocker не створює. |
| **Q-01** | Закрито                                     | D-02.                                                                                                                                                                                                                             |
| **Q-05** | Закрито                                     | D-03.                                                                                                                                                                                                                             |
| **Q-08** | Закрито                                     | D-08.                                                                                                                                                                                                                             |



**Отже, жодне Q-ID не блокує саму генерацію макету**, якщо D-01…D-08 справді вважаються чинними погодженими рішеннями. Єдина зовнішня залежність — підтвердження D-04 викладачем, якщо воно обов'язкове за процесом.



---



# 5. PLAN READY / NOT READY



## **NOT READY**



Не через відсутність основного coverage — воно практично повне.



Причина **NOT READY** — залишилися невеликі, але реальні проблеми трасованості:



1. **US-03 criteria не мають власного traceability mapping**, бо у джерелі вони без AC-ID.

2. **C-03-05 має неточне Source mapping**: потрібно саме `BR-08, BR-08.1, AC-07.4`.

3. Деякі derived behaviors — особливо “unassigned → no status” та “Closed → no next action” — потрібно чітко маркувати як **DERIVED**, а не source requirement.

4. D-04 має зовнішню примітку про підтвердження викладачем.



Це **не означає**, що треба перебудовувати screen map або додавати екрани.



---



# 6. Мінімальний перелік змінених пунктів



Без переписування непорушених розділів я б змінив лише:



1. **Traceability — US-03**

&#x20;  Додати 4 окремі записи для чотирьох unnumbered criteria US-03 без вигаданих `AC-03.x`.



2. **C-03-05 Source**

&#x20;  Замінити:

&#x20;  `US-07 AC-07.4`

&#x20;  на:

&#x20;  **`BR-08, BR-08.1, AC-07.4`**.



3. **N-06**

&#x20;  Позначити:

&#x20;  **DERIVED from US-07 AC-07.4 / BR-08**,

&#x20;  а не SOURCE.



4. **N-07**

&#x20;  Позначити:

&#x20;  **DERIVED from BR-08.1**.



5. **S-03 Priority**

&#x20;  Зафіксувати семантику:

&#x20;  **Not Set = unset state; selectable values = Low / Normal / High / Critical.**



6. **T-21 / Q-06**

&#x20;  Не фіксувати конкретний текст failed-save error як requirement; якщо текст є в mock — маркувати його **PROPOSED**.



7. **D-04**

&#x20;  Не змінювати поведінку в плані, але зберегти видимою примітку про **teacher confirmation**.



**Інші частини Screen Contracts, Screen Map, REF-01, TEST-33/34 та NFR-01/03/04 змінювати підстав немає.**



---



## Рішення розробника

Вердикт ChatGPT: NOT READY. Моє рішення: після правок у 03_Screen_Contracts_v1.0.md план прийнятий, блокерів для генерації макета немає.

Контроль B: виконано самостійно (Daria), другого рецензента немає; знахідки звірено зі SD_Spec.docx та 03_Screen_Contracts_v1.0.md.


| Знахідка | Рішення | Що зроблено / чому |
|---|---|---|
| G4-F1 (DEF-07): US-03 traceability | прийнято, вже було | У Traceability є окремі рядки UIR-11…14 для US-03 без вигаданих AC-ID, є примітка про це. Правка не потрібна |
| G4-F2 (DEF-08): Source у C-03-05 | відхилено | Лишила `BR-08, BR-08.1, US-07 AC-07.4`: це та сама вимога, різниця косметична |
| G4-F3 (DEF-09): Not Set | прийнято, вже було | У S-03 вказано, що Not Set не є значенням для вибору і reset немає |
| G4-F4 (DEF-10): N-06 | прийнято | Додано позначку DERIVED у Source |
| G4-F5 (DEF-11): N-07 | прийнято | Додано позначку DERIVED у Source |
| G4-F6 (DEF-12): текст помилки, Q-06 | прийнято, вже було | Конкретного тексту помилки у v1.0 немає, Q-06 лишається UNRESOLVED |
| G4-F7 (DEF-13): S-02-C | відхилено | Spec дозволяє порожній список або повідомлення (AC-02.5). Контракт фіксує лише наявність empty state, а точний текст не визначає |


Окремо: D-04 лишається з приміткою «потребує підтвердження викладачем».




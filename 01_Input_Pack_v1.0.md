# Mini Issue Tracker — UX/UI Input Pack v1.0



**Статус:** Input Pack для планування екранів.

**Release MVP:** **US-01–US-08**.

**US-09:** Deferred. **SR-10, SR-11:** Future.

**Непогоджені рішення:** залишені як UNRESOLVED; відкриті питання нижче не блокують план екранів.



---



# 1. SOURCE requirements



## 1.1 Release scope



**SOURCE — Release Scope §8:**



* Release MVP = **US-01..US-08**.

* Deferred = **US-09**.

* Future = **SR-10, SR-11**. 



---



## 1.2 US-01 — Create Issue



**SOURCE — US-01**



**Actor:** Team Member.



**Мета:** створити issue з title та description, щоб проблема була зареєстрована та могла відстежуватися. 



**AC-01.1:** Користувач може ввести `Title`, `Description`.

**AC-01.2:** `Title` є обов'язковим.

**AC-01.3:** `Description` є обов'язковим.

**AC-01.4:** Після створення issue отримує `ID`, `status = Open`, `created_at`, `author`.

**AC-01.5:** Після створення issue його priority не встановлено. 



**SOURCE — BDD:** користувач має бути authenticated; після Create створюється issue зі статусом Open, унікальним ID та поточним user як author. При порожньому Title issue не створюється, відображається `"Title is required"`. 



---



## 1.3 US-02 — View Issues



**SOURCE — US-02**



**Actor:** Team Member.



**AC-02.1:** Авторизований член команди може переглянути список issues.

**AC-02.2:** Для кожного issue відображаються щонайменше `ID`, `Title`, `Status`, `Priority`, `Assignee`, якщо призначений.

**AC-02.3:** Користувач може вибрати issue зі списку та відкрити сторінку деталей.

**AC-02.4:** На сторінці issue відображаються `ID`, `Title`, `Description`, `Status`, `Priority`, `Assignee`, якщо призначений, `Author`, `Created at`.

**AC-02.5:** Якщо issues немає, система відображає порожній список або повідомлення про відсутність issues. 



---



## 1.4 US-03 — Assign Issue to the responsible team member



**SOURCE — US-03**



**Actor:** Team Lead.



**Мета:** призначити заявку члену команди, щоб було зрозуміло, хто відповідає за її виконання. 



**AC без окремих вихідних ID:**



* можна вибрати одного assignee;

* assignee повинен бути членом команди;

* призначати заявку може лише Team Lead;

* поточний assignee відображається у заявці. 



---



## 1.5 US-04 — Set Issue Priority



**SOURCE — US-04**



**Actor:** Team Lead.



**AC-04.1:** Допустимі значення встановленого Priority: `Low`, `Normal`, `High`, `Critical`.

**AC-04.2:** Встановлювати та змінювати priority може лише Team Lead. 



---



## 1.6 US-05 — Track Issue Progress



**SOURCE — US-05**



**Actor:** Team Member.



**AC-05.1:** Поточний status відображається для кожного issue.

**AC-05.2:** Status відображається на сторінці issue.

**AC-05.3:** Status відображається у списку issues. 



---



## 1.7 US-06 — Add Comment



**SOURCE — US-06**



**Actor:** Team Member.



**AC-06.1:** Comment text cannot be empty.

**AC-06.2:** Added comment is stored and shown for the issue.

**AC-06.3:** Comment author is stored and displayed.

**AC-06.4:** Creation date/time is stored and displayed.

**AC-06.5:** Failed save does not create a comment and produces an error. 



---



## 1.8 US-07 — Update Issue Status



**SOURCE — US-07**



**Actor:** член команди, відповідальний за заявку.



**AC-07.1:** Нова заявка створюється зі статусом `Open`.

**AC-07.2:** Допустимі статуси: `Open`, `In Progress`, `Resolved`, `Closed`.

**AC-07.3:** Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді заявки.

**AC-07.4:** Користувач, який не є виконавцем заявки, не може змінити її статус.

**AC-07.5:** Система не дозволяє встановити значення статусу, якого немає у визначеному переліку. 



**SOURCE — BR-08.1:** дозволені переходи:



* `Open → In Progress`;

* `In Progress → Resolved`;

* `Resolved → Closed`;

* `Resolved → In Progress`.



Інші переходи не дозволені. 



---



## 1.9 US-08 — View Important Open Issues



**SOURCE — US-08**



**Actor:** Team Member.



**AC-08.1:** `priority = High` + status ≠ `Closed` → issue відображається в Important Issues.

**AC-08.2:** `priority = Critical` + status ≠ `Closed` → issue відображається в Important Issues.

**AC-08.3:** `Low` або `Normal` → не відображаються в Important Issues.

**AC-08.4:** `Closed` → не відображається незалежно від priority. 



**SOURCE — BR-09.1:** `Resolved` є вирішеним, але ще не закритим; до `Closed` issue залишається активним і може відображатися серед Important Issues відповідно до priority. 



---



# 2. SOURCE Business Rules / NFR



## Business Rules



**SOURCE — BR-01:** лише authenticated team members; у першій версії — 2 Team Member + 1 Team Lead. 



**SOURCE — BR-02:** будь-який authenticated team member може створити issue. 



**SOURCE — BR-03:** новий issue отримує `status = Open`. 



**SOURCE — BR-04:** новий issue має `priority = Not Set`. 



**SOURCE — BR-05:** встановлений priority — тільки `Low`, `Normal`, `High`, `Critical`; до встановлення допускається `Не встановлено` / пусте значення. 



**SOURCE — BR-06:** priority може встановлювати/змінювати лише Team Lead. 



**SOURCE — BR-07:** status — тільки `Open`, `In Progress`, `Resolved`, `Closed`. 



**SOURCE — BR-08:** status змінює відповідальний за issue член команди.

 

**SOURCE — BR-09:** issue вважається важливим, якщо його priority має значення High або Critical.



**SOURCE — BR-10:** issues не видаляються. 



**SOURCE — BR-11:** окрема користувацька історія змін issue не передбачається. 



---



## NFR



**SOURCE — NFR-01:** Response time `< 1 s`.

**SOURCE — NFR-02:** Authentication required.

**SOURCE — NFR-03:** HTTPS only.

**SOURCE — NFR-04:** Backup of all data. 



---



# 3. APPROVED design decisions



Нижче зафіксовані **тільки рішення зі статусом APPROVED** у наданому Decision Log.



### D-01 — APPROVED



**APPROVED:** Desktop, grayscale, low-fidelity. Один список із вкладками **All / Important**; **Create — modal**.



**Джерело рішення:** Methodical guide §6; погодила Daria, 06.10.2026; version 1.0.



---



### D-02 — APPROVED



**APPROVED:** S-01 — лише межа authentication; демонстрація починається з уже автентифікованого **S-02**.



**Закриває:** Q-01.



**Source requirements:** BR-01, NFR-02.



---



### D-03 — APPROVED



**APPROVED:** Відсутність priority показується як **Not Set**; вибір лише `Low / Normal / High / Critical`; reset не додається.



**Закриває:** Q-05.



**Source requirements:** BR-04, BR-05.



---



### D-04 — APPROVED



**APPROVED:** Після створення assignee відсутній; Team Lead може перепризначати issue.



**Закриває:** Q-04.



**Source requirement:** US-03.



**Примітка:** у Decision Log зазначено, що це потребує підтвердження викладачем, оскільки у специфікації явно не деталізовано. Тому **APPROVED як design decision**, але відповідне джерельне правило щодо можливості перепризначення залишається **не деталізованим SOURCE requirement**.



---



### D-05 — APPROVED



**APPROVED:** Заборонені role-dependent controls показуються **disabled**, а не приховуються.



**Джерело рішення:** Methodical guide §6.



---



### D-06 — APPROVED



**APPROVED:** Для зміни assignee / priority / status: **вибір → Apply** біля контролу; успіх залишає користувача на **S-03**.



**Статус:** UX-рішення, **не вихідний AC**.



**Джерело рішення:** Methodical guide §6.



---



### D-07 — APPROVED



**APPROVED:** Повернення з деталей зберігає попередню вкладку списку.



**Джерело рішення:** Methodical guide §6.



---



### D-08 — APPROVED



**APPROVED:** Макет використовує фіктивні дані; прототип — наперед задані результати; справжній login та БД не використовуються.



**Закриває:** Q-08.



**Джерело рішення:** Methodical guide §6.



---



# 4. UNRESOLVED



Ці питання **не перетворюються на погоджені вимоги або design decisions**.



### Q-02 — UNRESOLVED



Слово **«Open»** у назві US-08 не повністю узгоджується з AC-08.1/08.2 та BR-09.1, оскільки `Resolved` також входить до Important Issues.



**Поточне правило для Input Pack:** AC-08.1/08.2 та BR-09.1 мають перевагу над назвою US-08: `Resolved + High/Critical` входить до Important.  



**Статус:** UNRESOLVED щодо термінології, **не блокує план екранів**.



---



### Q-03 — UNRESOLVED



Спосіб повідомлення про недозволений status transition не визначений у SOURCE requirements.



**D-05** визначає disabled role-dependent controls як approved UX-рішення, тому питання **не блокує план екранів**.



---



### Q-06 — UNRESOLVED



Точний текст помилки failed comment save не визначений.



**Погоджений напрям:** окремий приклад помилки на S-03.



**Статус:** UNRESOLVED щодо конкретного тексту; **не блокує план екранів**.



---



### Q-07 — UNRESOLVED



Для порожнього списку AC-02.5 дозволяє:



* порожній список; або

* повідомлення про відсутність issues. 



**Погоджено:** окремий стан **S-02-C**.



**Статус:** UNRESOLVED щодо точного SOURCE-варіанта empty state; **не блокує план екранів**.



---



# 5. Screen-planning readiness



**DERIVED:** Непогоджених питань, які блокують планування екранів, наразі немає.



План може спиратися на такі **APPROVED** рішення:



* **S-01** — authentication boundary;

* **S-02** — список issues;

* **S-02-C** — empty-list state;

* **S-03** — issue details / interaction state;

* Create — **modal**;

* список має вкладки **All / Important**;

* desktop / grayscale / low-fidelity;

* role-dependent controls — disabled;

* assignee / priority / status — `select → Apply`;

* після успішної зміни залишається S-03;

* повернення до списку зберігає попередню вкладку;

* `Not Set` — окремий стан priority;

* демонстрація працює на фіктивних даних та predefined results.



**Не зафіксовано як APPROVED:** конкретні layout-компоненти, навігаційні патерни понад зазначені рішення, тексти unresolved-помилок, конкретний механізм authentication або будь-які додаткові функції.




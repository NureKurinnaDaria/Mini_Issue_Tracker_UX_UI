# UX/UI Design Input Pack — Mini Issue Tracker



**Статус документа:** аналітичний вхідний пакет для UX/UI, не готовий дизайн і не погоджений UI-рішення.

**Джерело:** `SD_Spec.docx`. Файл містить розділи 0–8 із вимогами, а розділ 9 `UX/UI & Interaction Design` представлений лише заголовком без опису.  



**Маркування:**



* **SOURCE** — прямо зазначено у специфікації.

* **DERIVED** — логічний висновок без додавання нової вимоги; обґрунтований джерелами.

* **PROPOSED** — запропоноване рішення; не є обов'язковою вимогою.

* **UNRESOLVED** — питання/суперечність, для якої специфікація не дає остаточної відповіді.



---



## 1. Product purpose



### Product purpose



**SOURCE** — Mini Issue Tracker — внутрішня вебсистема для невеликої команди розробників, призначена для реєстрації, відстеження та координації вирішення проблем програмного продукту. Бізнес-мета — скоротити час від реєстрації проблеми до її призначення та вирішення. 



**SOURCE** — Перший реліз охоплює реєстрацію, перегляд, призначення відповідального, встановлення priority, відстеження/зміну status, коментарі та визначення Important Issues. Остаточний Release MVP визначено як **US-01…US-08**. 



### Actors



**SOURCE** — Primary users:



* Team Member;

* Team Lead. 



**SOURCE** — У системі передбачено три заздалегідь визначені користувачі: **2 Team Member та 1 Team Lead**; користуватися системою можуть лише автентифіковані члени команди. 



**SOURCE** — Secondary stakeholders: Product Owner / замовник, System Administrator. Вони мають інтерес до результатів або належної роботи системи, але не є основними користувачами та не беруть безпосередньої участі в основних сценаріях MVP. 



### Межі першого релізу



**SOURCE** — Release MVP = **US-01..US-08**. **US-09** перенесено в Deferred. **SR-10 та SR-11** визначено як Future. 



**DERIVED** — У UX/UI Input Pack обов'язковою функціональною основою є US-01–US-08; US-09, SR-10 та SR-11 не повинні трактуватися як функції MVP. Це випливає з остаточного розділу `MVP / Release Scope`, який має пріоритет для визначення меж релізу. 



---



# 2. Scope



## 2.1 Included — Release MVP



**SOURCE** — Остаточний Release MVP: **US-01–US-08**. 



| ID    | Included capability                         |
| ----- | ------------------------------------------- |
| US-01 | Create Issue                                |
| US-02 | View Issues                                 |
| US-03 | Assign Issue to the responsible team member |
| US-04 | Set Issue Priority                          |
| US-05 | Track Issue Progress                        |
| US-06 | Add Comment                                 |
| US-07 | Update Issue Status                         |
| US-08 | View Important Open Issues                  |



**DERIVED** — Candidate MVP раніше включав US-01–US-09, але фінальний Release Scope явно встановлює US-01–US-08, а US-09 переносить у Deferred. Тому для цього Pack використовується **US-01–US-08**, а не Candidate MVP.  



---



## 2.2 Deferred



### US-09 — Attach files to an issue



**SOURCE** — US-09 описує можливість прикріплення файлів до issue та містить AC щодо upload, persistent storage, ліміту 10 MB, antivirus scanning, rejected infected files і download integrity. 



**SOURCE** — Реалізація US-09 оцінена як значно складніша через file storage, file size limits, malware scanning та backup. Рекомендовано перенести US-09 за межі першого релізу. Остаточний Release Scope підтверджує: `Deferred: US-9`.  



---



## 2.3 Future



### SR-10 — Automatic issue categorization



**SOURCE** — Система в майбутньому має допомагати визначати категорію звернення; SR-10 визначено як `Won't for now`, а фінальний Release Scope відносить SR-10 до Future.   



### SR-11 — Automatic Priority recommendation



**SOURCE** — SR-11 визначено як рекомендацію щодо priority; він має статус `Won't for now`, а фінальний Release Scope відносить SR-11 до Future.   



**SOURCE** — Якщо AI-функціональність буде реалізована, рекомендації мають лише рекомендаційний характер: AI не повинен автоматично змінювати category або priority без дії користувача. 



---



## 2.4 Explicitly out of scope



**SOURCE** — На поточному етапі явно виключені:



* Customer support portal;

* Billing;

* Source-code hosting;

* Continuous Integration / Continuous Delivery;

* Time tracking;

* Full project management;

* Mobile application. 



**SOURCE** — У першій версії не планується інтеграція з GitHub, GitLab, Jira чи іншими issue trackers. 



**SOURCE** — Issues не видаляються із системи. Окрема користувацька історія змін issue у першій версії не передбачається. 



---



# 3. Included User Stories та Acceptance Criteria



## US-01 — Create Issue



**SOURCE — US-01**

**Actor:** Team Member.

**Мета:** створити issue з title та description, щоб проблема була зареєстрована й могла відстежуватися. 



### Acceptance Criteria



**SOURCE — AC-01.1:** Користувач може ввести:



* Title;

* Description. 



**SOURCE — AC-01.2:** Title є обов'язковим. 



**SOURCE — AC-01.3:** Description є обов'язковим. 



**SOURCE — AC-01.4:** Після створення заявка отримує:



* ID;

* `status = Open`;

* `created_at`;

* `author`. 



**SOURCE — AC-01.5:** Після створення issue його priority не встановлено. 



**SOURCE — BDD alternative for AC:** за умови автентифікації та перебування на `New Issue` користувач вводить title і description та натискає Create; створюється issue зі статусом Open, унікальним ID та поточним користувачем як author. 



**SOURCE — BDD negative case:** якщо title порожній і користувач натискає Create, issue не створюється, система показує `"Title is required"`. 



---



## US-02 — View Issues



**SOURCE — US-02**

**Actor:** Team Member.

**Мета:** переглядати зареєстровані issues та їх поточні деталі. 



### Acceptance Criteria



**SOURCE — AC-02.1:** Авторизований член команди може переглянути список зареєстрованих issues. 



**SOURCE — AC-02.2:** Для кожного issue у списку відображаються щонайменше:



* ID;

* Title;

* Status;

* Priority;

* Assignee, якщо він призначений. 



**SOURCE — AC-02.3:** Користувач може вибрати issue зі списку та відкрити сторінку його деталей. 



**SOURCE — AC-02.4:** На сторінці issue відображаються:



* ID;

* Title;

* Description;

* Status;

* Priority;

* Assignee, якщо він призначений;

* Author;

* Created at. 



**SOURCE — AC-02.5:** Якщо зареєстрованих issues немає, система відображає порожній список або повідомлення про відсутність issues. 



---



## US-03 — Assign Issue to the responsible team member



**SOURCE — US-03**

**Actor:** Team Lead.

**Мета:** призначити заявку члену команди, щоб було зрозуміло, хто відповідає за її виконання. 



### Acceptance Criteria



**SOURCE — AC-03, вихідні критерії без окремих ідентифікаторів:**



* можна вибрати одного assignee;

* assignee повинен бути членом команди;

* призначати заявку може лише Team Lead;

* поточний assignee відображається у заявці. 



**DERIVED** — Не створюю окремих ID для цих чотирьох критеріїв, оскільки у специфікації вони наведені як ненумерований `Acceptance Criteria 03`. 



---



## US-04 — Set Issue Priority



**SOURCE — US-04**

**Actor:** Team Lead.

**Мета:** встановити пріоритет заявки, щоб команда могла визначити порядок роботи. 



### Acceptance Criteria



**SOURCE — AC-04.1:** Допустимі значення встановленого Priority: `Low`, `Normal`, `High`, `Critical`. 



**SOURCE — AC-04.2:** Встановлювати та змінювати priority може лише Team Lead. 



---



## US-05 — Track Issue Progress



**SOURCE — US-05**

**Actor:** Team Member.

**Мета:** бачити поточний status issue, щоб розуміти поточний прогрес роботи. 



### Acceptance Criteria



**SOURCE — AC-05.1:** Поточний status відображається для кожного issue. 



**SOURCE — AC-05.2:** Status відображається на сторінці issue. 



**SOURCE — AC-05.3:** Status відображається у списку issues. 



---



## US-06 — Add Comment



**SOURCE — US-06**

**Actor:** Team Member.

**Мета:** додавати коментарі до заявки для обговорення проблеми. 



### Acceptance Criteria



**SOURCE — AC-06.1:** Comment text cannot be empty. 



**SOURCE — AC-06.2:** Added comment is stored and shown for the issue. 



**SOURCE — AC-06.3:** Comment author is stored and displayed. 



**SOURCE — AC-06.4:** Creation date/time is stored and displayed. 



**SOURCE — AC-06.5:** Failed save does not create a comment and produces an error. 



---



## US-07 — Update Issue Status



**SOURCE — US-07**

**Actor:** член команди, відповідальний за заявку.

**Мета:** змінювати status заявки, щоб команда бачила поточний стан роботи. 



### Acceptance Criteria



**SOURCE — AC-07.1:** Нова заявка створюється зі статусом `Open`. 



**SOURCE — AC-07.2:** Допустимі статуси: `Open`, `In Progress`, `Resolved`, `Closed`. 



**SOURCE — AC-07.3:** Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді заявки. 



**SOURCE — AC-07.4:** Користувач, який не є виконавцем заявки, не може змінити її статус. 



**SOURCE — AC-07.5:** Система не дозволяє встановити значення статусу, якого немає у визначеному переліку. 



---



## US-08 — View Important Open Issues



**SOURCE — US-08**

**Actor:** Team Member.

**Мета:** швидко бачити важливі відкриті заявки, щоб зосередитися на проблемах, які потребують першочергової уваги. 



### Acceptance Criteria



**SOURCE — AC-08.1:** Заявка з `priority = High` і статусом, відмінним від `Closed`, відображається у списку Important Issues. 



**SOURCE — AC-08.2:** Заявка з `priority = Critical` і статусом, відмінним від `Closed`, відображається у списку Important Issues. 



**SOURCE — AC-08.3:** Заявки з `priority = Low` або `Normal` не відображаються у списку Important Issues. 



**SOURCE — AC-08.4:** Заявки зі `status = Closed` не відображаються незалежно від priority. 



---



# 4. UI-релевантні Business Rules



## 4.1 Права доступу



**SOURCE — BR-01:** користуватися системою можуть лише автентифіковані члени команди; у першій версії — два Team Member та один Team Lead. 



**SOURCE — BR-02:** будь-який автентифікований член команди може створити issue. 



**SOURCE — BR-06:** встановлювати та змінювати priority може тільки Team Lead. 



**SOURCE — US-03 AC:** призначати issue може лише Team Lead; assignee повинен бути членом команди. 



**SOURCE — US-07 AC-07.4:** користувач, який не є виконавцем заявки, не може змінити її status. 



**DERIVED** — UI має відрізняти доступні та недоступні для поточного користувача дії відповідно до цих прав. Це не означає конкретний спосіб візуалізації або конкретний екран.   



---



## 4.2 Початкові значення



**SOURCE — BR-03 / AC-01.4 / AC-07.1:** новий issue автоматично отримує `status = Open`.   



**SOURCE — BR-04 / AC-01.5:** новий issue має `priority = Not Set`, тобто priority не визначений. 



**SOURCE — BR-05:** до встановлення priority допускається стан `Не встановлено` / пусте значення; після встановлення значеннями можуть бути лише `Low`, `Normal`, `High`, `Critical`. 



**DERIVED** — `Not Set` не слід трактувати як окремий рівень priority поряд із Low/Normal/High/Critical: специфікація прямо розрізняє невстановлене значення та встановлений priority. 



---



## 4.3 Status та переходи



**SOURCE — BR-07:** допустимі статуси: `Open`, `In Progress`, `Resolved`, `Closed`. 



**SOURCE — BR-08:** змінювати status може член команди, відповідальний за виконання issue. 



**SOURCE — BR-08.1:** дозволені переходи:



* `Open → In Progress`;

* `In Progress → Resolved`;

* `Resolved → Closed`;

* `Resolved → In Progress`.



Інші переходи не дозволені. 



**SOURCE — AC-07.3:** після зміни status нове значення зберігається та відображається при наступному перегляді issue. 



**SOURCE — AC-07.5:** система не дозволяє встановити status поза визначеним переліком. 



**DERIVED** — `Resolved` не можна прирівнювати до `Closed`: BR-09.1 прямо визначає Resolved як вирішений, але ще не закритий стан; до переходу в Closed issue залишається активним. 



---



## 4.4 Important Issues



**SOURCE — BR-09:** issue вважається важливим, якщо priority = `High` або `Critical`. 



**SOURCE — BR-09.1:** `Resolved` є вирішеним, але ще не закритим; до переходу в `Closed` така заявка залишається активною та може відображатися серед Important Issues відповідно до priority. 



**SOURCE — AC-08.1–AC-08.4:** Important Issues включають High/Critical із будь-яким status, крім Closed; Low/Normal не включаються; Closed не включається незалежно від priority. 



**DERIVED** — Для визначення Important Issues пріоритет має значення лише разом із правилом виключення `Closed`; `Resolved` не є виключенням.  



---



## 4.5 Comments



**SOURCE — AC-06.1:** порожній comment не допускається. 



**SOURCE — AC-06.2:** успішно доданий comment зберігається та відображається для issue. 



**SOURCE — AC-06.3:** author comment зберігається та відображається. 



**SOURCE — AC-06.4:** дата/час створення comment зберігається та відображається. 



**SOURCE — AC-06.5:** якщо збереження comment не вдалося, comment не створюється і система показує помилку. 



---



## 4.6 Заборонені дії



**SOURCE** — Issue не можна видаляти. BR-10: `Issues не видаляються із системи`. 



**SOURCE** — Неавторизований користувач не може користуватися системою згідно з BR-01. 



**SOURCE** — Team Member не може встановлювати/змінювати priority; це право має тільки Team Lead. 



**SOURCE** — Користувач, який не є assignee, не може змінювати status. 



**SOURCE** — Недопустимі status transitions не дозволені. 



**SOURCE** — Окрема користувацька історія змін issue не передбачається в першій версії. 



---



# 5. Relevant NFR



## Вимоги, релевантні UI/UX



**SOURCE — NFR-01:** Response time `< 1 s`. 



**DERIVED** — Вимога є релевантною для UX через очікування швидкої реакції системи, але сама по собі не визначає конкретний layout, компонент чи interaction pattern. 



**SOURCE — NFR-02:** Authentication required. 



**SOURCE — NFR-03:** HTTPS only. 



---



## Вимоги, які не перевіряються макетом



**SOURCE — NFR-04:** Backup of all data. 



**DERIVED** — NFR-03 `HTTPS only` та NFR-04 `Backup of all data` не можуть бути повноцінно перевірені лише через UX/UI макет; це технічні/інфраструктурні властивості системи. 



**DERIVED** — NFR-01 `< 1 s` також не може бути підтверджений самим макетом; макет може враховувати вимогу як constraint для взаємодій, але фактична перевірка потребує працюючої системи та вимірювання response time. 



**UNRESOLVED** — Специфікація не визначає механізм authentication, хоча Authentication required є обов'язковою вимогою. У ризиках окремо зазначено, що спосіб реалізації authentication потребує уточнення.  



---



# 6. Q-ID — конфлікти, прогалини та питання для рішення людини



## Q-01 — Authentication mechanism



**Тип: UNRESOLVED**



**SOURCE:** Authentication required; система має трьох заздалегідь визначених користувачів; спосіб реалізації authentication у ризиках потребує уточнення.   



**Питання:** Яким саме способом користувачі автентифікуються?



**Обмеження:** Не можна самостійно додавати спосіб автентифікації в UI Input Pack.



---



## Q-02 — Meaning of “open” у US-08



**Тип: UNRESOLVED / термінологічна неузгодженість**



**SOURCE:** Назва US-08 — `View Important Open Issues`. Водночас AC-08.1 та AC-08.2 вимагають показувати High/Critical із будь-яким status, крім `Closed`; BR-09.1 окремо підтверджує, що `Resolved` може залишатися серед Important Issues.  



**Висновок:** за формальними AC `Resolved` входить до Important Issues, якщо priority High/Critical.



**Питання для людини:** Чи термін `Open` у назві US-08 означає будь-який status, крім Closed, чи мався на увазі конкретно `Open` / незавершені status?



**Важливо:** не скасовую AC на користь назви Story; суперечність винесена окремо.



---



## Q-03 — Status transitions



**Тип: SOURCE + UNRESOLVED щодо повноти UX-повідомлень**



**SOURCE:** дозволені лише `Open → In Progress`, `In Progress → Resolved`, `Resolved → Closed`, `Resolved → In Progress`; інші переходи не дозволені. 



**Питання:** Яким саме способом користувачу повідомляється про недозволений перехід, якщо така спроба виникає?



**Примітка:** сама заборона є SOURCE; конкретний UI-механізм повідомлення специфікацією не визначений.



---



## Q-04 — Assignment та зміна assignee



**Тип: UNRESOLVED**



**SOURCE:** Team Lead може призначити одного assignee, assignee має бути членом команди, поточний assignee відображається. 



**Питання:** Чи може Team Lead змінити вже призначеного assignee, і чи може issue бути повернуто у стан без assignee?



Специфікація цього явно не визначає.



---



## Q-05 — Priority `Not Set`



**Тип: SOURCE + UNRESOLVED щодо відображення**



**SOURCE:** новий issue має priority `Not Set`; до встановлення priority допускається пусте значення; встановлені значення — Low, Normal, High, Critical. 



**Питання:** Який саме текст/візуальне представлення має використовувати UI для невстановленого priority?



**DERIVED:** не можна трактувати `Not Set` як `Low`, `Normal`, `High` або `Critical`, оскільки специфікація явно розрізняє цей стан від встановленого priority. 



---



## Q-06 — Comment save failure



**Тип: UNRESOLVED**



**SOURCE:** при failed save comment не створюється та виникає error. 



**Питання:** Який конкретний текст повідомлення про помилку та який UX-механізм його відображення погоджено?



Специфікація цього не визначає.



---



## Q-07 — Empty issues list



**Тип: UNRESOLVED**



**SOURCE:** якщо issues немає, система показує порожній список **або** повідомлення про відсутність issues. 



**Питання:** Який із двох варіантів є остаточним для UI?



---



## Q-08 — Authentication scope vs user management



**Тип: UNRESOLVED**



**SOURCE:** у першій версії є три predefined users, але специфікація не описує процес створення, редагування або адміністрування цих користувачів. 



**Питання:** Чи входить будь-яке керування predefined users до першого релізу?



**DERIVED:** додавати user-management UI до обов'язкових вимог не можна, оскільки відповідної US/AC немає.



---



# 7. Важливі аналітичні висновки для UX/UI



**DERIVED — Priority:** новий issue має `Not Set`, а не один із чотирьох priority levels. Це окремий стан, який потрібно зберегти в UX-логіці. 



**DERIVED — Status:** `Resolved` ≠ `Closed`. Resolved може залишатися активним і потрапляти до Important Issues, якщо priority High/Critical. 



**DERIVED — Important Issues:** критерій виключення — `Closed`, а не `Resolved`. High/Critical + Resolved залишається допустимим Important Issue. 



**DERIVED — Permissions:** UI не повинен представляти дії як доступні користувачу, якщо відповідне право прямо заборонене BR/AC: priority для не-Team Lead, status для не-assignee, assignment для не-Team Lead.   



**DERIVED — Comments:** помилка збереження не повинна приводити до появи нового comment, оскільки AC-06.5 прямо вимагає протилежного. 



**DERIVED — Deletion:** UI не повинен передбачати дію видалення issue, оскільки BR-10 прямо забороняє видалення. 



**DERIVED — History:** не слід закладати окрему user-facing історію змін issue як MVP-функцію, оскільки BR-11 її не передбачає. 



**DERIVED — Attachments:** UI для attachment не є частиною Release MVP, оскільки US-09 остаточно перенесено в Deferred. 



---



# 8. Source Check



## 8.1 Кожна Included US → усі AC → місце у Pack



| Included US | Усі вихідні AC / критерії                                  | Місце у Pack |
| ----------- | ---------------------------------------------------------- | ------------ |
| **US-01**   | AC-01.1, AC-01.2, AC-01.3, AC-01.4, AC-01.5 + BDD-сценарії | §3 US-01     |
| **US-02**   | AC-02.1, AC-02.2, AC-02.3, AC-02.4, AC-02.5                | §3 US-02     |
| **US-03**   | 4 ненумеровані критерії `Acceptance Criteria 03`           | §3 US-03     |
| **US-04**   | AC-04.1, AC-04.2                                           | §3 US-04     |
| **US-05**   | AC-05.1, AC-05.2, AC-05.3                                  | §3 US-05     |
| **US-06**   | AC-06.1, AC-06.2, AC-06.3, AC-06.4, AC-06.5                | §3 US-06     |
| **US-07**   | AC-07.1, AC-07.2, AC-07.3, AC-07.4, AC-07.5                | §3 US-07     |
| **US-08**   | AC-08.1, AC-08.2, AC-08.3, AC-08.4                         | §3 US-08     |



**SOURCE:** US-01–US-08 є остаточним Release MVP. 



---



## 8.2 Excluded / не включені у Release MVP



| Вимога / capability                                | Статус                  | Причина                                                                                                         |
| -------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------------------------------------- |
| **US-09 Attach files to an issue**                 | Deferred                | Офіційно перенесена за межі першого релізу через значно вищу складність та залежності storage/scanning/backup.  |
| **SR-10 Automatic issue categorization**           | Future                  | У фінальному Release Scope визначена як Future.                                                                 |
| **SR-11 Automatic Priority recommendation**        | Future                  | У фінальному Release Scope визначена як Future.                                                                 |
| Customer support portal                            | Explicitly out of scope | Прямо виключено на поточному етапі.                                                                             |
| Billing                                            | Explicitly out of scope | Прямо виключено.                                                                                                |
| Source-code hosting                                | Explicitly out of scope | Прямо виключено.                                                                                                |
| CI/CD                                              | Explicitly out of scope | Прямо виключено.                                                                                                |
| Time tracking                                      | Explicitly out of scope | Прямо виключено.                                                                                                |
| Full project management                            | Explicitly out of scope | Прямо виключено.                                                                                                |
| Mobile application                                 | Explicitly out of scope | Прямо виключено; також A-07 зазначає, що mobile app для першої версії не потрібний.                             |
| GitHub/GitLab/Jira/інші issue trackers integration | Explicitly out of scope | У першій версії інтеграція не планується.                                                                       |
| Issue deletion                                     | Explicitly prohibited   | BR-10: issues не видаляються.                                                                                   |
| User-facing change history                         | Explicitly not planned  | BR-11: окрема користувацька історія змін не передбачається.                                                     |



---



## 8.3 Unresolved questions



1. **Q-01:** Який конкретний механізм authentication використовується? 

2. **Q-02:** Що саме означає `Open` у назві US-08, якщо AC включають Resolved, але виключають лише Closed? 

3. **Q-03:** Як UI має повідомляти про недозволений status transition? Специфікація визначає заборону, але не спосіб повідомлення. 

4. **Q-04:** Чи можна змінювати вже призначеного assignee / повертати issue у стан без assignee? 

5. **Q-05:** Яке точне UI-представлення має мати priority `Not Set`? 

6. **Q-06:** Який текст/спосіб відображення error для failed comment save? 

7. **Q-07:** Який варіант empty state є остаточним: порожній список чи повідомлення? 

8. **Q-08:** Чи входить керування трьома predefined users до MVP? Специфікація цього не визначає. 



---



## Підсумковий статус Pack



**SOURCE** — Остаточний Release Scope: **US-01–US-08**; **US-09 — Deferred**; **SR-10/SR-11 — Future**. 



**DERIVED** — Цей Input Pack містить лише вимоги, правила, критерії та висновки, необхідні як вхід для подальшого UX/UI опрацювання; конкретні екрани, layout, компоненти, способи навігації, механізми authentication або інші нові функції тут не визначаються.



**UNRESOLVED** — Pack **не оголошується погодженим**: наведені Q-01–Q-08 потребують рішення людини перед остаточним закріпленням відповідних UX/UI-рішень.




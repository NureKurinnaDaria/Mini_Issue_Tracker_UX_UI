# Фінальна перевірка макета v1.2 (Mini Issue Tracker)

Автор перевірки: Daria. Дата: 07.11.2026. Версія макета: Figma v1.2 (v1.1 — 19.10.2026).
Джерело вимог: SD_Spec.docx. Докази: evidence/ (S-01, S-02-A/B/C, M-01, S-03, REF-01/02/03).
Позначки: **PASS (mock)** — видно в макеті; **PASS (prototype)** — пройдено в Present mode; **STATIC** — показано станом у REF-блоці, окремого шляху в прототипі немає; **NOT VERIFIABLE** — потребує реалізації.

## 1. Результат по кожному AC

| US / AC | Що перевірено | Екран / доказ | Результат |
| --- | --- | --- | --- |
| US-01 AC-01.1 | Введення Title і Description | M-01 | PASS (mock) |
| US-01 AC-01.2 | Title обов'язковий | REF-02, M-01 Empty Title | STATIC |
| US-01 AC-01.3 | Description обов'язковий | REF-02, M-01 Empty Description | STATIC |
| US-01 AC-01.4 | Нова заявка отримує ID, Open, дату, автора | REF-02, M-01 → S-03 New issue (ISS-007) | PASS (prototype): Create відкриває S-03 нової заявки; генерація ID і часу NOT VERIFIABLE |
| US-01 AC-01.5 | Priority не встановлено (Not Set) | REF-02, S-03 New issue (ISS-007) | PASS (prototype) |
| US-02 AC-02.1 | Перегляд списку | S-02-A | PASS (mock) |
| US-02 AC-02.2 | Поля рядка: ID, Title, Status, Priority, Assignee, якщо призначений | S-02-A/B (ISS-006 без Assignee), TEST-38 | PASS (mock) |
| US-02 AC-02.3 | Вибір заявки відкриває деталі | S-02-A → S-03 (T-02) | PASS (prototype) |
| US-02 AC-02.4 | Поля деталей | S-03 | PASS (mock) |
| US-02 AC-02.5 | Порожній список | S-02-C, REF-02 Empty Important | PASS (mock) |
| US-03 (4 пункти без ID) | Один assignee з трьох користувачів; лише Team Lead призначає; поточний assignee показаний | S-03, REF-01, REF-02 Team Lead selectors | PASS (mock); результат Apply — STATIC |
| US-04 AC-04.1 | Значення Low, Normal, High, Critical | REF-02 Team Lead selectors | PASS (mock) |
| US-04 AC-04.2 | Priority змінює лише Team Lead | REF-01, S-03 (disabled для Member) | PASS (mock) |
| US-05 AC-05.1 | Status видно для кожної заявки | S-02-A/B, S-03 | PASS (mock) |
| US-05 AC-05.2 | Status на сторінці заявки | S-03 | PASS (mock) |
| US-05 AC-05.3 | Status у списку | S-02-A/B, TEST-38 | PASS (mock) |
| US-06 AC-06.1 | Порожній коментар не додається | REF-02 Empty Comment | STATIC |
| US-06 AC-06.2 | Доданий коментар показано | S-03, REF-02 Successful comment | STATIC; збереження NOT VERIFIABLE |
| US-06 AC-06.3 | Автор коментаря | S-03 (Member A, Member B) | PASS (mock) |
| US-06 AC-06.4 | Дата й час коментаря | S-03 | PASS (mock); справжній час NOT VERIFIABLE |
| US-06 AC-06.5 | Помилка збереження не створює коментар | REF-02 Failed comment save | STATIC; справжня помилка NOT VERIFIABLE |
| US-07 AC-07.1 | Нова заявка — Open | REF-02 S-03 New issue | STATIC |
| US-07 AC-07.2 | Чотири статуси | REF-01 (Current status) | PASS (mock) |
| US-07 AC-07.3 | Новий статус показується при наступному перегляді | REF-01 (ISS-003 після Closed), TEST-33 | STATIC; збереження NOT VERIFIABLE |
| US-07 AC-07.4 | Статус змінює лише assignee | REF-01, S-03 (Member A assignee), REF-01 Team Lead non-assignee | PASS (mock) |
| US-07 AC-07.5 | Значення поза переліком не пропонується | S-03 Status control, REF-01 Allowed destinations | PASS (mock) |
| US-08 AC-08.1 | High і не Closed — в Important | S-02-B: ISS-003 | PASS (prototype) |
| US-08 AC-08.2 | Critical і не Closed — в Important | S-02-B: ISS-002 | PASS (prototype) |
| US-08 AC-08.3 | Low і Normal — не в Important | S-02-A містить ISS-001 (Low), ISS-005 (Normal); у S-02-B їх немає | PASS (prototype) |
| US-08 AC-08.4 | Closed не в Important | S-02-A містить ISS-004 (Critical, Closed); у S-02-B немає | PASS (prototype) |

Правила BR-08.1 (переходи) і BR-09/BR-09.1 (Resolved залишається в Important): REF-01, S-03, S-02-B.

## 2. Ручна повторна перевірка прототипу (Present mode)

| Сценарій | Очікується | Результат |
| --- | --- | --- |
| Important → ISS-003 → Back | Повернення на Important (D-07) | PASS (prototype), 19.10.2026 |
| All → ISS-003 → Back | Повернення на All (D-07) | PASS (prototype), 19.10.2026 |
| Important → Create → Cancel | Повернення на Important | PASS (prototype), 19.10.2026 |
| All → Create → Cancel | Повернення на All | PASS (prototype), 19.10.2026 |
| All → Create → заповнені Title і Description → Create | Відкривається S-03 нової заявки ISS-007: Open, Not Set, без assignee, без коментарів (T-05), а не ISS-003 | PASS (prototype), 07.11.2026 |

Не підключено як окремі шляхи (показано станами в REF-01/02): T-09, T-11, T-13–T-16, T-19–T-21.

## 3. Зведення тестів TEST-01…TEST-38

Для кожного тесту доказова частина виконана на макеті або в прототипі; частина, що потребує реалізації (збереження, справжня авторизація, серверна фільтрація), позначена NOT VERIFIABLE.

| Тести | Тема | Екран / доказ | Результат |
| --- | --- | --- | --- |
| TEST-01/02 | Межа автентифікації | S-01 | PASS (mock) як анотація; автентифікація NOT VERIFIABLE |
| TEST-03/04 | Створення заявки (Member і Lead) | M-01 → S-03 New issue (ISS-007), REF-02 | PASS (prototype) для Team Member; для Team Lead STATIC; генерація ID NOT VERIFIABLE |
| TEST-05/06 | Порожні Title і Description | REF-02 M-01 | STATIC |
| TEST-07 | Cancel залишає попередню вкладку | M-01 Cancel | PASS (prototype) |
| TEST-08 | Поля сторінки деталей | S-03 | PASS (mock) |
| TEST-09 | Деталі бачить будь-який член команди | S-03, REF-01 | PASS (mock) |
| TEST-10…14 | Призначення і priority | S-03, REF-01, REF-02 | PASS (mock); Apply — STATIC |
| TEST-15…18 | Дозволені цілі статусу | REF-01, S-03 | PASS (mock) |
| TEST-19…21 | Status недоступний (non-assignee, Lead, unassigned) | REF-01 | PASS (mock) |
| TEST-22/23 | Результат переходу статусу | REF-01, REF-02 | STATIC |
| TEST-24/25 | Додавання коментаря | S-03, REF-02 | STATIC |
| TEST-26/27 | Порожній коментар і помилка збереження | REF-02 | STATIC; помилка збереження NOT VERIFIABLE |
| TEST-28…30, 35…37 | Important predicate | S-02-A, S-02-B | PASS (prototype) |
| TEST-31 | Порожній All | S-02-C | PASS (mock) |
| TEST-32 | Back зберігає вкладку (D-07) | S-03, прототип | PASS (prototype) |
| TEST-33 | ISS-003 після Closed зникає з Important | REF-01 (fixtures) | STATIC |
| TEST-34 | Порожній Important | REF-02 | STATIC |
| TEST-38 | Поля рядка списку | S-02-A/B | PASS (mock) |

## 4. Межі

NFR-01 (час відгуку), NFR-03 (HTTPS), NFR-04 (backup), справжня автентифікація й авторизація, збереження даних, серверна фільтрація Important і справжня обробка помилки збереження коментаря в макеті не перевіряються.

Mini Issue Tracker — UX/UI Practical Assignment

Автор: Daria
Дата: 19.10.2026 (версія макета v1.1; v1.0 збережена 16.10.2026)
Специфікація: SD_Spec.docx
Release MVP: US-01–US-08
Deferred: US-09
Future: SR-10, SR-11

Версія макета, що здається: Figma v1.1 (19.10.2026)

Посилання
---------
GitHub repository: https://github.com/NureKurinnaDaria/Mini_Issue_Tracker_UX_UI
Figma (can view): https://www.figma.com/design/VRik3SXtmuwUqx73p4X8vW/Mini-Issue-Tracker---UX-UI---Kurinna-Daria?node-id=0-1&t=fM5yfYWqDbzfHX5C-1

Склад репозиторію
-----------------
- 01_Input_Pack_v0.1.md / 01_Input_Pack_v1.0.md
- 02_Screen_Map_v0.1.md / 02_Screen_Map_v1.0.md
- 03_Screen_Contracts_v0.1.md / 03_Screen_Contracts_v1.0.md
- 04_Plan_Review_v0.1.md
- 05_Figma_Brief_v1.0.md
- 06_Review_Report.md
- AI_interaction_log.md
- Decision_Log.md
- Review_Defect_Change_Log.md (CHK-01...CHK-12)
- SD_Spec.docx
- evidence/ (9 PNG макета: S-01, S-02-A/B/C, M-01, S-03, REF-01/02/03; 2 PNG скриншоти ChatGPT, G1)

Короткий опис роботи
--------------------
Виконано повний маршрут методички G1–G6:

- G1 — UX/UI Input Pack: вибірка вимог зі SD_Spec, 8 US і 33 AC, US-09 Deferred,
  SR-10/SR-11 Future, виявлено Q-01...Q-08.
- G2 — Screen Map: мінімальна архітектура S-01, S-02-A/B/C, M-01, S-03;
  таблиця переходів T-01...T-26; user flows UF-01...UF-10.
- G3 — Developer-Verifiable Screen Contracts: контроль C-IDs, permission matrix,
  status transition matrix, Important predicate, traceability, 34 тести,
  REF-01 verification contexts; Addendum за CHK-04 (N-01...N-07, TEST-33/34).
- G4 — Plan Review: аудит плану, coverage matrix без MISSING, 7 знахідок,
  рішення розробника прийнято; F2 не виконувалась (дефектів не виявлено).
- G5 — Figma Brief: self-contained англомовний brief, скомпільований з
  фінального контракту і Decision Log; контроль C пройдено.
- F1 — одна генерація у Figma Agent: 6 канонічних surfaces + 3 REF-блоки,
  77 instances reusable components, Auto Layout.
- P1 — ручний прототип у Figma Prototype; пройдено 9 переходів у Present mode
  (T-01, T-02, T-03, T-04, T-05, T-08, T-22, T-23, T-24). Після правки D-07
  (CHK-12) Back у S-03 повертає на попередню вкладку списку (All або Important),
  Create відкривається з обох вкладок, Cancel повертає на ту саму вкладку.
- G6 — фінальний аудит на 9 frames: UI design coverage — Complete;
  зауваження DEF-01–06 і UX-01–05 розглянуто в CHK-12 (1 прийнято і виправлено,
  решта відхилено з причинами); implementation-level вимоги залишаються
  outstanding поза межами wireframes.
- Після G6: уточнення анотацій REF-03 і закриття D-04 (CHK-10, CHK-11);
  правка D-07 у прототипі (CHK-12). Макет збережено як v1.1.

Ключові результати
------------------
- Release MVP: US-01–US-08 реалізовано на рівні wireframes.
- Permissions: Team Lead non-assignee не має права на status; non-assignee
  не має права на status; unassigned -> status недоступний; Closed -> немає
  наступного переходу.
- Important predicate: Priority in {High, Critical} AND Status != Closed;
  Resolved входить, Closed виключений.
- Priority: Not Set — стан absence, не selectable рівень; reset відсутній.
- Comments: три стани окремо — success, empty, failed.
- D-04: закрито за SD_Spec (Open Questions, US-03, US-01 AC-01.4, US-02 AC-02.2);
  перепризначення — DERIVED, не окремий AC (Decision_Log, ред. 1.1).

Межі перевірки
--------------
Реалізація, серверна авторизація, persistence, HTTPS, backup та response time
не перевірялися у wireframe і залишаються implementation tests outstanding
(див. 06_Review_Report.md). Apply, статусні переходи та сценарії коментарів
показано станами у REF-02/REF-03, а не окремими прототипними шляхами.
Після аудиту G6 (v1.0) AI-перевірка не повторювалась; правки v1.1 стосуються
анотацій REF-03 і навігації прототипу (CHK-11, CHK-12).
Примітка: DEF-ID у Review_Report (G6) і в Review_Defect_Change_Log (CHK-02…CHK-05) нумеруються окремо і не збігаються за змістом; у журналі зауваження G6 розглянуто в CHK-12.

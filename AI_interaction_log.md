# AI interaction log



## Запис 1

- Date / operator / step: 06.10.2026 / Daria / G1

- Tool / plan / mode: ChatGPT, тариф Free (Безкоштовно); звичайний чат, відповідь супроводжувалась позначкою «Попрацював 17s»; назва моделі в інтерфейсі не показана

- Context / version: SD_Spec.docx (повний файл), новий чат «Mini Issue Tracker - UX/UI»

- Prompt / response: промпт G1 (розділ 5 методички) без змін; результат збережено в 01_Input_Pack_v0.1.md

- Decision / evidence: контроль A пройдено: 8 US і 33 AC на місці, US-09 Deferred, SR-10/SR-11 Future; Q-01...Q-08 залишено відкритими

- Limit / fallback: повідомлень про ліміт не було



## Запис 2

- Date / operator / step: 06.10.2026 / Daria / G1 (контрольне повідомлення, фіксація Decision Log)

- Tool / plan / mode: ChatGPT, тариф Free (Безкоштовно); звичайний чат; назва моделі в інтерфейсі не показана

- Context / version: той самий чат; Input Pack v0.1 + Decision_Log.md (D-01...D-08)

- Prompt / response: контрольне повідомлення з §6 методички; результат збережено в 01_Input_Pack_v1.0.md

- Decision / evidence: перевірила збереження ID і 33 AC; APPROVED-рішення винесені в окрему секцію; Q-02, Q-03, Q-06, Q-07 лишаються UNRESOLVED; знайдено пропуск BR-09, додано вручну

- Limit / fallback: повідомлень про ліміт не було



## Запис 3

- Date / operator / step: 06.10.2026 / Daria / G2

- Tool / plan / mode: ChatGPT, тариф Free (Безкоштовно); звичайний чат; назва моделі в інтерфейсі не показана

- Context / version: той самий чат; Input Pack v1.0 + Decision Log D-01...D-08

- Prompt / response: промпт G2 (розділ 7 методички) без змін; результат збережено в 02_Screen_Map_v0.1.md

- Decision / evidence: прийнято структуру S-01, S-02-A/B/C, M-01, S-03 і 6 Figma frames; знайдено DEF-02...DEF-06, виправлення внесено в 02_Screen_Map_v1.0.md і передано в G3 окремим блоком

- Limit / fallback: повідомлень про ліміт не було



## Запис 4

- Date / operator / step: 06.10.2026 / Daria / G3

- Tool / plan / mode: ChatGPT, тариф Free (Безкоштовно); звичайний чат; назва моделі в інтерфейсі не показана

- Context / version: той самий чат; Input Pack v1.0 і Decision Log D-01...D-08 з попередніх повідомлень; план екранів 02_Screen_Map_v0.1.md з G2; виправлення DEF-02...DEF-06 вставлено текстом у тому самому повідомленні перед промптом

- Prompt / response: промпт G3 (розділ 9 методички) вставлено без змін, але з доданим перед ним блоком із шести виправлень до Screen Map (DEF-02...DEF-06); результат збережено в 03_Screen_Contracts_v0.1.md

- Decision / evidence: виправлення DEF-02...DEF-06 застосовано у відповіді; подальша перевірка (CHK-04) знайшла DEF-07...DEF-10, тому запрошено Addendum (див. Запис 5)

- Limit / fallback: повідомлень про ліміт не було



## Запис 5

- Date / operator / step: 06.10.2026 / Daria / G3 (Addendum за CHK-04)

- Tool / plan / mode: ChatGPT, тариф Free (Безкоштовно); звичайний чат; назва моделі в інтерфейсі не показана

- Context / version: той самий чат; 03_Screen_Contracts_v0.1.md

- Prompt / response: запит на Addendum із п'яти пунктів (N-ID, transitions S-03, REF-01, тести і Traceability, виправлення Source); результат дописано у 03_Screen_Contracts_v1.0.md

- Decision / evidence: прийнято весь Addendum; вручну прибрано сторонній рядок і залишкові DEF-ID у Source

- Limit / fallback: повідомлень про ліміт не було



## Запис 6

- Date / operator / step: 06.10.2026 / Daria / G4

- Tool / plan / mode: ChatGPT, тариф Free (Безкоштовно); звичайний чат; назва моделі в інтерфейсі не показана

- Context / version: той самий чат; SD_Spec.docx, Input Pack, Decision Log, 03_Screen_Contracts_v0.1.md з Addendum (v1.0 з ручними правками в чат не вставлявся; знахідки звірено з v1.0 окремо)

- Prompt / response: промпт G4 (розділ 11 методички) без змін; відповідь: coverage без MISSING, 7 знахідок, вердикт NOT READY; відповідь збережено в 04_Plan_Review_v0.1.md

- Decision / evidence: знахідки звірено зі SD_Spec і з v1.0; 2 прийнято з правкою (DERIVED у N-06/N-07), 3 вже покриті, 2 відхилено; додатково виявлено неточність AC-07.5 (DEF-11); рецензент у тому самому чаті не незалежний, тому контроль B виконано самостійно (робота індивідуальна): знахідки перевірено за першоджерелом SD_Spec.docx, а не за повторним AI-схваленням

- Limit / fallback: повідомлень про ліміт не було



## Запис 7

- Date / operator / step: 09.10.2026 / Daria / G5

- Tool / plan / mode: ChatGPT, тариф Free; той самий чат; назва моделі не показана

- Context / version: 03_Screen_Contracts_v1.0.md + Decision_Log.md, прикріплені файлами; промпт G5 надіслано повторно з уточненням

- Prompt / response: промпт G5 (розділ 12 методички) без змін + коротке уточнення про актуальні файли; результат збережено в 05_Figma_Brief_v1.0.md

- Decision / evidence: контроль C пройдено (весь текст, 4 статуси, Important predicate, рольові обмеження, без квадратних дужок)

- Limit / fallback: повідомлень про ліміт не було



## Запис 8

- Date / operator / step: 09.10.2026 / Daria / F1

- Tool / plan / mode: Figma Agent (Design file); Starter plan; без додаткових AI-запитів

- Context / version: 05_Figma_Brief_v1.0.md прикріплено у Figma Agent; промпт-лаунчер (розділ 13 методички)

- Prompt / response: створено 6 канонічних surfaces + 3 REF-блоки; 77 instances reusable components; Auto Layout

- Decision / evidence: F1 review за розділом 14 — дефектів не виявлено; F2 не виконувалась (ручний маршрут за розділом 16)

- Limit / fallback: повідомлень про ліміт не було



## Запис 9

- Date / operator / step: 09.10.2026 / Daria / P1

- Tool / plan / mode: Figma, вкладка Prototype (ручне зв'язування, без AI)

- Context / version: Figma file після F1; 12 зв'язків за розділом 17 методички

- Prompt / response: —

- Decision / evidence: базовий прототип пройдено в Present mode; 9 маршрутів працюють (T-01, T-02, T-03, T-04, T-05, T-08, T-22, T-23, T-24)

- Limit / fallback: повідомлень про ліміт не було



## Запис 10

- Date / operator / step: 16.10.2026 / Daria / G6

- Tool / plan / mode: ChatGPT, тариф Free; той самий чат

- Context / version: Figma PNG (9 frames) + текстовий опис REF-03 + промпт G6 (розділ 19 методички) + студентський звіт про ручний прохід прототипу

- Prompt / response: повний G6 аудит; результат збережено в 06_Review_Report.md

- Decision / evidence: UI coverage — Complete; prototype — student-reported Present mode; DEF-01–DEF-06 — NOT VERIFIABLE (не дефекти макета); UX-01–UX-05 — UX-рекомендації

- Limit / fallback: повідомлень про ліміт не було


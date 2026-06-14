# BACKLOG_PROMPT_CACHE_MeCat.md — MeCat Local Prompt Cache

Version: 1.0.0 (Deferred Tech Tasks)  
Date: 2026-06-15  
Status: DEFERRED (PR-008 → Phase 1; PR-009 → Phase 2; PR-010 → Phase 3; PR-011 → Phase 4)  

Ez a fájl a MeCat modul **helyi prompt váróterme (Prompt Cache)**. Ide kerülnek azok a magasabb szintű technikai specifikációk és fejlesztési promptok, amelyek a jelenlegi fázisban még nem végrehajthatóak, megakadályozva a kódoló Agentek túl korai implementációs kísérleteit.

---

## 📌 Aktív Fázis Ellenőrzés
*   **Jelenlegi projektfázis:** **Phase 0 (Bootstrap)** — Lásd: `docs/APP_STATE_MeCat.md`.
*   **Ezek a backlog feladatok legkorábban a megjelölt fázisokban aktiválhatók.**

*TILOS ezen feladatok kódolásába kezdeni, amíg az `docs/APP_STATE_MeCat.md` szerint a projekt el nem éri a megfelelő fázist!*

---

## 📥 Eltárolt Fejlesztői Promptok (Cached Prompts)

### 🟢 PR-008 — Gyűjtemény-Lemérés & Legacy Fájlnév-Parszer — Phase 1
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack Developer]
[DEPENDENCY: Phase 0 Completion (repo + katalógus séma kész)]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §2.1-2.2, §4]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. Scan worker (/api/scan): rekurzív gyűjtemény-bejárás, hash/formátum/hossz/meglévő ID3 kiolvasás → media_items feltöltés + hiány-térkép.
2. Legacy parser (/api/legacy/parse): RegEx-motor, amely a régi fájlnév-sémát feltöri a docs/zenei_tag_szotar.md szótár szerint → media_tags (source='legacy_parse').
3. A salsa.txt/bachata.txt kiindulási listák betöltése Golden Data-ként.
FONTOS: ekkor még NINCS enrichment/AI taggelés — csak inventory + determinisztikus parse.
```

### 🟠 PR-009 — Metaadat-Gazdagítás (Web-lookup + Audio-analízis) — Phase 2
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack Developer]
[DEPENDENCY: Phase 1 Completion]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §2.3]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. Web-lookup: hiányzó artist/title/genre/év pótlása külső forrásokból (/api/enrich).
2. Audio-analízis worker (Python: librosa/essentia): BPM, hangnem (key), energia, ritmikai jellemzők; videónál ffmpeg hangsáv-analízis.
3. Eredmények media_items + media_tags frissítése, confidence jelöléssel.
```

### 🔴 PR-010 — Rendszerszintű Minősítő Alkotmány & Hibrid Taggelés — Phase 3
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack Developer]
[DEPENDENCY: Phase 2 Completion]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §1 (két alkotmány), §2.4, §4 (constitutions)]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. constitutions (scope='system') szerkesztő UI + determinisztikus HA->AKKOR taggelő motor (rules_json).
2. AI fallback a bizonytalan tagekre; emberi review képernyő (low confidence kiemelve).
3. Feedback loop: javítás → tag_learned_rules (Golden Data) bővítés.
4. KineLex fogalomtár ↔ MeCat tag-kulcs harmonizáció ([CROSS-MODULE DELEGATION]).
```

### 🔴 PR-011 — Tanári Ízlés Alkotmány, Write-back & Sync/Smart Playlists — Phase 4
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack & Frontend Developer]
[DEPENDENCY: Phase 3 Completion]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §1 (Ízlés Alkotmány), §2.5, §4 (teacher_taste_overlays)]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. constitutions (scope='teacher') + teacher_taste_overlays: tanáronkénti rating/al-tagek/preferencia az objektív réteg fölé rétegezve.
2. Write-back (/api/write-back): jóváhagyott tagek ID3v2/metaadat írása, letisztult fájlnév, eredeti megőrzése.
3. GWS/Drive szinkron a tanári laptopokra; AIMP Smart Playlist export/automatikus frissülés.
4. Multi-dimenziós tag-keresés UI (/api/search) + Smart Playlist szerkesztő.
```

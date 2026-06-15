# BACKLOG_PROMPT_CACHE_MeCat.md — MeCat Local Prompt Cache

Version: 1.2.0 (Phase 5–6: CUE D093, Player D094, Lesson-Matcher D088, Practice D089)  
Date: 2026-06-15  
Status: DEFERRED (PR-008 → Phase 1; PR-009 → Phase 2; PR-010 → Phase 3; PR-011 → Phase 4; PR-015/017 → Phase 5; PR-016 → Phase 6; PR-012 → Phase 6+ haladó)  

Ez a fájl a MeCat modul **helyi prompt váróterme (Prompt Cache)**. Ide kerülnek azok a magasabb szintű technikai specifikációk és fejlesztési promptok, amelyek a jelenlegi fázisban még nem végrehajthatóak, megakadályozva a kódoló Agentek túl korai implementációs kísérleteit.

> **Protokoll SSoT:** `../../DANA/docs/AGENT_PROTOCOL_STANDARD.md`

---

## 📌 Aktív Fázis Ellenőrzés
*   **Jelenlegi projektfázis:** **Brainstorming** → Phase 0 után Bootstrap — Lásd: `docs/APP_STATE_MeCat.md`.
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

### 🔵 PR-012 — Haladó: MIR / Audio-Embedding, Harmonikus Keverés & Ízlés-Vektor Ajánló — Phase 4+ (D083, P43, P46)
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack Developer]
[DEPENDENCY: Phase 4 Completion (két alkotmány + write-back/sync működik)]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §2.3-2.5; DANA P43, P46, D083]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT (haladó képesség — a "világ leghaladóbb elvei" kör):
1. Audio-embedding: önfelügyelt zenei reprezentációs vektorok kinyerése (pl. CLAP/OpenL3 vagy hasonló), tárolás pgvector mezőben → "találj hasonló hangzású számot" szemantikus keresés.
2. Harmonikus keverés: Camelot-kerék (key + BPM) alapú átmenet-ajánló auto-DJ/óraterv-szekvenciához.
3. Tanári ízlés-vektor: a teacher_taste_overlays-ből collaborative-filtering / embedding-alapú személyre szabott ajánló (a kétszintű alkotmány adatvezérelt kiterjesztése).
4. LLMOps (P46): eval-készlet a tag-minőségre, prompt-verziózás, tenant-szintű AI-költség budget.
FONTOS: csak a Phase 1-4 stabil alaprétegre építhető; nem helyettesíti a determinisztikus + emberi review pipeline-t (P43), hanem kiegészíti.
```

### 🟣 PR-015 — CUE-adatbázis & MIR Auto-Match (D093) — Phase 5
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack Developer]
[DEPENDENCY: Phase 4 Completion (kétszintű alkotmány + write-back működik)]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §5.5, §9 (cue_points, cue_learned_rules), §10; docs/MeCat_Music_Tags.md §IX; DANA D093]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. DB migráció: `cue_points` (scope system|teacher, type, theme, time_ms, bar_index, linked_term_ids[], confidence, source, approved_by) + `cue_learned_rules` (match patterns → CUE type/theme).
2. API: CRUD `/api/cues`, batch import, `/api/cues/match` (MIR detektált váltás ↔ DB; match_score, transition_family; auto-insert review queue ha confidence < θ).
3. MIR worker bővítés: break/hit/peak/stop/downbeat → `mir_suggested` CUE-k; tematizált könyvtár keresés (theme, type, genre).
4. KineLex kötés: `linked_term_ids[]` validálás ontológia-export API-val (cross-module inbox CLOSED után).
5. Golden Data: jóváhagyott CUE-párosítások → `cue_learned_rules` bővítés (P43 feedback loop).
FONTOS: system CUE nem írja felül teacher CUE-t — rétegezés (P49).
```

### 🟣 PR-017 — Óraterv-Illesztőmotor & Diák Gyakorló-Kísérő (D088, D089) — Phase 5
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack & Frontend Developer]
[DEPENDENCY: Phase 4 Completion + PR-015 CUE alapréteg (opcionális de ajánlott)]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §5.1-5.3; DANA D088, D089; KineLex/BeatPass INBOX szerződések]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. `/api/playlists/from-lesson` — KineLex óraterv input (fogalom-sorozat + nehézség) → tag-szabály alapú playlist + óra-energia-ív (warmup→peak→cooldown); §4.4 energia-görbe + usage tagek.
2. Hasonló-lista ajánló: Golden Data playlist history + teacher_taste_overlays → collaborative / ízlés-vektor javaslat.
3. `/api/student/today` — BeatPass órarend alapján diák-nézet: mai óra zenéi, scope=student-visible (D092).
4. Gyakorló UI (Frontend): 8-as loop, time-stretch (pitch preserve), számoló overlay (§4.2 „1"-detektálás), spaced repetition emlékeztető (P31).
5. BeatPass domain-események (P44): `PlaylistGenerated`, `StudentPracticeOpened` — webhook/event bus.
```

### 🔵 PR-016 — MeCat Natív Lejátszó & Külső Adapter-réteg (D094) — Phase 6
```markdown
[TARGET_AGENT: MeCat Tech Lead & Full-Stack & Frontend Developer]
[DEPENDENCY: Phase 5 Completion (CUE DB + lesson matcher stabil)]
[SPEC_LINK: docs/MASTER_CONCEPT_MeCat.md §6; DANA D094; docs/MeCat_Music_Tags.md §IX]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
1. Natív web-lejátszó (Next.js + Web Audio / wavesurfer.js): hullámforma, beat-grid, MAP/CUE overlay (system vs teacher szín).
2. CUE-szerkesztő UI: kattintás/gyorsbillentyű → CUE létrehozás; drag finomítás; `@mm:ss` és `bar:N` input.
3. Órai mód: számoló 1–8, 8-as loop, hangmagasság-tartó lassítás (D089 integráció).
4. DJ support mód: Camelot/BPM kompatibilitás, mix-point jelzés, energia-ív preview.
5. Export adapterek: AIMP (Smart Playlist + ID3/MAP COMMENT), Virtual DJ (.m3u/VDJFolder + cue export), VLC (.xspf); watch-folder szinkron; `source=external_sync` visszaolvasás ahol lehetséges.
6. Headless API: `/api/player/session` — playlist + CUE state külső frontendhez (P34/P35).
FONTOS: MeCat DB = SSoT; külső app = projekció — nem „vagy-vagy" (D094).
```

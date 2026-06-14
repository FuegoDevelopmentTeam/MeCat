# APP_STATE_MeCat.md — MeCat Active Project State

Version: 0.1.0  
Date: 2026-06-15  
Current Phase: **Phase 0: Bootstrap & Core Schema**  
Status: IN_PROGRESS (Not started yet)  

Ez a fájl a MeCat modul **aktív rendszer- és fejlesztési állapotát (Source of Truth)** rögzíti. Minden ide belépő Agentnek kötelező ezt elsőként beolvasnia, hogy lássa, hol tart a projekt, és mik a soron következő lépések.

> **Megjegyzés a prototípusról:** A "régi" rendszer (fájlnévbe kódolt információ) működik, de a MeCat azt **nem** folytatja közvetlenül — egy tag-natív, rendszerkonform architektúrát építünk, és a régit migrációs forrásként (legacy parser + Golden Data) használjuk.

---

## 🧭 1. Jelenlegi Fázis és Mérföldkövek (Phase & Milestones)

```
[►] Phase 0: Bootstrap & Core Schema  <-- JELENLEG AKTÍV
[ ] Phase 1: Collection Scan & Legacy Filename Parser (inventory + old-school migráció)
[ ] Phase 2: Metadata Enrichment (web-lookup + audio-analízis: BPM/key/energia)
[ ] Phase 3: Dual-Constitution Tagging (rendszerszintű Minősítő Alkotmány + AI hibrid)
[ ] Phase 4: Tanári Ízlés Alkotmány + Write-back, GWS/Drive Sync & Smart Playlists
```

---

## 🎯 2. Aktív Backlog (Soron következő feladatok - Next Up)

### Task 0.1: Repozitórium Rendberakása & Könyvtárszerkezet
*   **Státusz:** `[PENDING]`
*   **Leírás:** Hozd létre a feldolgozó worker (`/app` vagy `/worker`) és a Next.js control plane (`/app`) szerkezetét; a meglévő `docs/` és `Agents/` érintetlenül marad.
*   **Kimenet:** Sandbox app-mappa + Supabase konfiguráció.

### Task 0.2: Katalógus DB Inicializálása (Core Schema migrations)
*   **Státusz:** `[PENDING]`
*   **Leírás:** Hozd létre a `media_items`, `media_tags`, `constitutions`, `teacher_taste_overlays`, `tag_learned_rules` táblákat.
*   **Kimenet:** Első Supabase migrációs fájlok (`0001_initial_schema.sql`).

### Task 0.3: Környezeti változók & Auth konfiguráció
*   **Státusz:** `[PENDING]`
*   **Leírás:** Next.js + Supabase összeköttetés (.env.local), alap autentikáció, GWS/Drive hozzáférés előkészítése.

---

## 🧊 3. Elnapolt / Jövőbeli Feladatok (Deferred Backlog)

A magasabb fázisok specifikációit biztonságosan eltároltuk:
*   `docs/BACKLOG_PROMPT_CACHE_MeCat.md`

---

## 📜 4. Fejlesztési Napló (Changelog)
*   **2026-06-15 (v0.1.0):** Rendszerkonform struktúra felépítése; koncepció kivonatolva a korábbi `docs/MASTER_CONCEPT.md` (v9.0) és `Ask_Agent` fejlesztésből. Két alkotmány (Minősítő + Ízlés) rögzítve. (Szőnyi Levente / DANA Stratéga)

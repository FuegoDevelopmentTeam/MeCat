# APP_STATE_MeCat.md — MeCat Active Project State

Version: 0.4.0  
Date: 2026-06-15  
Current Phase: **Brainstorming (Koncepció-fázis)**  
Status: CONCEPT — D086-D094 rögzítve; cross-module delegációk **ELKÜLDVE**.

Ez a fájl a MeCat modul **aktív rendszer- és fejlesztési állapotát (Source of Truth)** rögzíti. Minden ide belépő Agentnek kötelező ezt elsőként beolvasnia.

> **Fázis-figyelmeztetés:** A projekt **brainstorming** állapotban van. A `docs/BACKLOG_PROMPT_CACHE_MeCat.md` technikai promptjai (PR-008+) **csak a koncepció jóváhagyása és a Phase 0 elindítása után** aktiválhatók. Most a cél a koncepció (D086-D092) véglegesítése és a cross-module szerződések (KineLex/BeatPass) egyeztetése.

> **Megjegyzés a prototípusról:** A "régi" rendszer (fájlnévbe kódolt információ) működik, de a MeCat azt **nem** folytatja közvetlenül — tag-natív, rendszerkonform architektúrát építünk, a régit migrációs forrásként (legacy parser + Golden Data) használjuk.

---

## 🧭 1. Jelenlegi Fázis és Mérföldkövek (Phase & Milestones)

```
[►] Brainstorming: Koncepció + DANA-integráció + cross-module szerződések  <-- JELENLEG AKTÍV
[ ] Phase 0: Bootstrap & Core Schema (repo, katalógus DB, auth)
[ ] Phase 1: Collection Scan + Akusztikus Ujjlenyomat + Legacy Parser (inventory + old-school migráció, P51)
[ ] Phase 2: Metadata Enrichment (web-lookup + MIR: BPM/key/energia/szerkezet/energia-ív)
[ ] Phase 3: Kétszintű Alkotmány Tagging (rendszerszintű Minősítő + AI hibrid, P43; KineLex ontológia-harmonizáció D084)
[ ] Phase 4: Tanári Ízlés Alkotmány + Write-back + GWS/Drive Sync + Smart Playlists
[ ] Phase 5: Zenei-Pedagógiai Illesztőmotor (D088) + Diák Gyakorló-Kísérő (D089) + **CUE-adatbázis** (D093)
[ ] Phase 6: **MeCat Lejátszó + külső adapter** (D094) + haladó tartalom-intelligencia (embedding, Camelot, P50)
[ ] Inspirációs Tár (D086 §6.1) + Jogtisztaság-réteg (D090) — fázisba sorolandó a koncepció jóváhagyásakor
```

---

## 🎯 2. Aktív Backlog (Brainstorming — Next Up)

### Task B.1: Koncepció jóváhagyatása a Userrel
*   **Státusz:** `[IN_PROGRESS]`
*   **Leírás:** A `docs/MASTER_CONCEPT_MeCat.md` v2.0.0 + a DANA D086-D092 / P49-P51 döntések áttekintése, finomítása, jóváhagyása. Nyitott kérdések: lásd a MeCat koncepció §12 (M-ICE-01..04).

### Task B.2: Cross-Module Szerződések — **ELKÜLDVE** ✅
*   **Státusz:** `[DELEGATED — OPEN]`
*   **Fájlok:**
    *   `../KineLex/docs/INBOX_CROSS_MODULE_DELEGATION_MeCat.md`
    *   `../BeatPass/docs/INBOX_CROSS_MODULE_DELEGATION_MeCat.md`
*   **Várakozás:** KineLex + BeatPass Tech Lead válasz / szerződés-tervezet.

### Task B.3: Tag-taxonomia & CUE-koncepció — **KÉSZ** ✅
*   **Státusz:** `[DONE]`
*   **Kimenet:** `docs/MeCat_Music_Tags.md` v1.0.0; `MASTER_CONCEPT_MeCat.md` v2.1.0 (CUE D093, Player D094, ZITA legacy §1.3).

### Task B.4: Fázis-terv véglegesítése & Backlog frissítés — **KÉSZ** ✅
*   **Státusz:** `[DONE]`
*   **Kimenet:** `BACKLOG_PROMPT_CACHE_MeCat.md` v1.2.0 — PR-015 (CUE D093), PR-017 (D088/D089), PR-016 (Player D094); MASTER index frissítve; `AGENT_PROTOCOL_STANDARD.md` ökoszisztéma SSoT.

---

## 🧊 3. Elnapolt / Jövőbeli Feladatok (Deferred Backlog)
A magasabb fázisok specifikációi: `docs/BACKLOG_PROMPT_CACHE_MeCat.md`.

---

## 📜 4. Fejlesztési Napló (Changelog)
*   **2026-06-15 (v0.4.0):** BACKLOG v1.2.0 (PR-015/016/017); ökoszisztéma agent-protokoll harmonizáció (`AGENT_PROTOCOL_STANDARD.md`). (DANA Stratéga)
*   **2026-06-15 (v0.3.0):** ZITA=legacy név tisztázás; D093 CUE-adatbázis + D094 lejátszó/adapter; `MeCat_Music_Tags.md` kanonikus taxonomia (12 domain); cross-module delegációk elküldve (KineLex, BeatPass inbox); agentek + `.cursorrules` frissítve Top-Down kontextussal. DANA v1.33.0. (DANA Stratéga)
*   **2026-06-15 (v0.2.0):** Top-Down integrációs kör (D086-D092, P49-P51). `MASTER_CONCEPT_MeCat.md` v2.0.0.
*   **2026-06-15 (v0.1.0):** Rendszerkonform struktúra felépítése; koncepció kivonatolva a korábbi `docs/MASTER_CONCEPT.md` (v9.0) és `Ask_Agent` fejlesztésből. Két alkotmány (Minősítő + Ízlés) rögzítve.

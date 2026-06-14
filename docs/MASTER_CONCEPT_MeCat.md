# MASTER_CONCEPT_MeCat.md — MeCat Technical Specification

Version: 1.0.0 (Media Categorizer — Collection Scan, Metadata Enrichment & Dual-Constitution Tagging)  
Date: 2026-06-15  
Status: RELEASED  
Repo: https://github.com/FuegoDevelopmentTeam/MUSIC.git (modul-márkanév: **MeCat**)  
Tech Stack: Python feldolgozó worker (audio/video analízis, ID3/metaadat írás), Supabase/PostgreSQL (katalógus DB), Next.js + Tailwind (vezérlő dashboard, Alkotmány-szerkesztő), pluggable AI (tag-javaslat), GWS/Drive szinkron a tanárok felé.

> **Forrásdokumentumok (a korábbi fejlesztésből kivonatolva):** `docs/MASTER_CONCEPT.md` (v9.0 — LPDPC + zenei tag rendszer), `docs/zenei_tag_szotar.md`, `docs/kiindulasi_docs/*`, `Agents/Ask_Agent_Master_Concept_Builder.md`. Ezek megmaradnak referenciaként; ez a fájl a rendszerkonform, AI-ready szintézis.

---

## 1. Rendszer-architektúra és Filozófia

A **MeCat (Media Categorizer)** a DANA ökoszisztéma média-feldolgozó thin-slice modulja. Lemér egy zenei/videó gyűjteményt, pótolja a hiányzó metaadatokat, és **kereshető tagekkel** látja el a médiát, hogy a tánctanárok (és más felhasználók) pillanatok alatt megtalálják a megfelelő darabot (pl. „összes 5 csillagos Bachata Sensual, BPM szerint növekvő").

### Paradigmaváltás: Old-School → Tag-natív
| Szempont | Régi prototípus (működik) | MeCat (új) |
| :--- | :--- | :--- |
| **Információ tárolása** | Mindent a **fájlnévbe** kódolva | Letisztult fájlnév (`Előadó - Cím.mp3`) + gazdag **ID3/metaadat tagek** |
| **Példa** | `1,90-14 {o6} MEJOR . [mu 4 melody on2 descarga show] Joe Cuba - Quinto Sabroso.mp3` | `Joe Cuba - Quinto Sabroso.mp3` + tagek: `genre:salsa`, `bpm:190`, `salsa_on2`, `rating:6`, `density:show`… |
| **Keresés** | Fájlnév-string, törékeny | Strukturált, multi-dimenziós tag-keresés / Smart Playlist |
| **Minősítés** | Egy ember feje / kézi | **Két alkotmány**: rendszerszintű Minősítő + tanári Ízlés Alkotmány |
| **Bővítés** | Manuális | Web-lookup + audio-analízis + AI-javaslat + Golden Data feedback |

### Két Alkotmány (a modul egyedi magja)
1. **Rendszerszintű Minősítő Alkotmány (System Qualification Constitution):**
   *   Objektív, mérnöki és táncszakmai **sztenderd** tag-taxonómia és minősítési szabályok (BPM, hangnem, energia, ritmika, műfaj-hierarchia, MAP/ütemtérkép, hangminőség).
   *   Egy van belőle, az egész gyűjteményre érvényes „igazság-alap".
2. **Tanárhoz Rendelhető Ízlés Alkotmány (Teacher Taste Constitution):**
   *   **Szubjektív** preferencia-réteg tanáronként: saját értékelés (rating), kedvelt/kerülendő stílusok, saját al-tagek, óratípushoz illő válogatás-súlyozás.
   *   Ugyanaz a szám **több tanárnál más-más** ízlés-tageket/ratinget kaphat, miközben a rendszerszintű objektív tagek közösek maradnak.
   *   A tanári réteg az objektív réteg fölé **rétegződik**, nem írja felül azt.

---

## 2. Fő Funkciók (End-to-End Pipeline)

### 2.1. Gyűjtemény-Lemérés (Collection Scan / Inventory)
*   Rekurzív végigjárás a zenei/videó tárhelyen; fájlonként hash, formátum, hossz, meglévő ID3/metaadat kiolvasása.
*   Hiány-térkép: mely fájlokból hiányzik az előadó/cím/műfaj/BPM/tag.

### 2.2. Legacy Fájlnév-Parszer (Old-School Migráció)
*   RegEx-motor, amely a régi, fájlnévbe kódolt sémát (`[Sebesség]-[index] {[értékelés]} [STÍLUS] . ([tagek]) Előadó - Cím`) feltöri, és a `docs/zenei_tag_szotar.md` szótár szerint **strukturált tagekké** alakítja.
*   A `salsa.txt`, `bachata.txt` stb. kiindulási listák betáplálása Golden Data-ként.

### 2.3. Metaadat-Gazdagítás (Enrichment)
*   **Web-lookup:** hiányzó előadó/cím/műfaj/év kinyerése külső forrásokból.
*   **Audio-analízis:** BPM, hangnem (key), energia, ritmikai jellemzők kinyerése a hangfájlból (pl. Python: librosa/essentia; videónál ffmpeg + hangsáv-analízis).

### 2.4. Alkotmány-Vezérelt Címkézés (Dual-Constitution Tagging)
*   Hibrid (FuBi-mintájú) eljárás: **determinisztikus szabály** + **AI-javaslat** + emberi jóváhagyás.
*   Először a rendszerszintű Minősítő Alkotmány tölti ki az objektív tageket; majd opcionálisan a kiválasztott tanári Ízlés Alkotmány adja a szubjektív réteget.
*   Bizonytalan AI-tagek emberi review-ra jelölve; a javítás Golden Data-ba / tanult szabályba kerül (feedback loop).

### 2.5. Írás, Szinkron és Smart Playlists
*   A jóváhagyott tagek visszaírása ID3v2 mezőkbe (és/vagy videó-metaadatba), letisztult fájlnévvel.
*   GWS/Drive szinkron a tanári laptopokra; AIMP Smart Playlist-ek automatikus frissülése (pl. „latin_dance Salsa Footwork, Top Rated, Lassú→Gyors").

---

## 3. Tag-Taxonómia (kivonat a tag-szótárból)

A teljes szótár: `docs/zenei_tag_szotar.md`. Fő dimenziók:
*   **Alaptulajdonságok:** `tempo` (BPM + módosítók), `genre` / `genre_group`, `artist`, `title`, `remixer_dj`.
*   **DMC (Dance/Music Category) jelzők:** `dance_style_timing` (pl. `salsa_on2`), al-műfajok (`salsa_dura`…), `origin` (cuban/new_york…), `listening_difficulty` (beginner/advanced).
*   **Hangulati/strukturális ("common"):** `mood`, `energy_level`, `hardness`, `density`, `rhythm`, `instrument`/`theme`, `audio_quality`, `source_era`, `authenticity`.
*   **Műfaj-specifikus:** `vocal_language`, `rhythm_signature`, `beat_clarity`, `musical_flow`, **MAP** (nyolcas-térkép, pl. `map:[i4! v88 r4!4]`), `part`, `musicality`.

### ID3v2 Leképezés (kivonat)
| Funkció | ID3v2 mező | Példa |
|---|---|---|
| Fő műfaj | `GENRE` | `Salsa` |
| Tempó | `BPM` | `120` |
| Előadó / Cím | `ARTIST` / `TITLE` | `Joe Cuba` / `Quinto Sabroso` |
| Műfaj-csoport | `GROUPING` | `latin_dance` |
| Tánc stílus/időzítés | `CUSTOM_DANCE_STYLE` | `salsa_on2` |
| Hangulat | `MOOD` | `energetic` |
| MAP / ütemtérkép | `COMMENT` | `map:[i4! v88 r4!4]` |
| (Tanári) értékelés | `RATING` / `POPULARIMETER` | tanáronként eltérő |

> A **tanári Ízlés Alkotmány** tagjei dedikált, tanár-prefixelt custom mezőkbe vagy a katalógus DB tanár-scope táblájába kerülnek, hogy ne ütközzenek a rendszerszintű tagekkel.

---

## 4. Adatbázis Séma (Supabase / PostgreSQL — katalógus control plane)

```sql
-- 1. Médiatételek (a gyűjtemény leképezése)
CREATE TABLE media_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    file_path TEXT NOT NULL,
    file_hash VARCHAR(64),
    media_type VARCHAR(10) NOT NULL,        -- 'audio' | 'video'
    artist VARCHAR(200),
    title VARCHAR(300),
    duration_sec INT,
    bpm INT,
    music_key VARCHAR(10),
    clean_filename VARCHAR(300),            -- generált, letisztult név
    legacy_filename TEXT,                   -- eredeti old-school név (migrációs forrás)
    scan_status VARCHAR(20) DEFAULT 'scanned', -- scanned | enriched | tagged | synced
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. Rendszerszintű objektív tagek (Minősítő Alkotmány kimenete)
CREATE TABLE media_tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    media_id UUID NOT NULL REFERENCES media_items(id),
    tag_key VARCHAR(40) NOT NULL,           -- pl. 'genre', 'dance_style_timing', 'energy_level'
    tag_value VARCHAR(120) NOT NULL,
    source VARCHAR(20) DEFAULT 'rule',      -- 'rule' | 'ai' | 'human' | 'legacy_parse'
    confidence VARCHAR(10),                 -- 'high' | 'low'
    UNIQUE (media_id, tag_key, tag_value)
);

-- 3. Alkotmányok (rendszerszintű + tanári ízlés)
CREATE TABLE constitutions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    scope VARCHAR(20) NOT NULL,             -- 'system' | 'teacher'
    teacher_id UUID,                        -- NULL, ha scope='system'
    name VARCHAR(120) NOT NULL,
    rules_md TEXT,                          -- minősítési/ízlés szabályok (ember-olvasható)
    rules_json JSONB,                       -- determinisztikus HA->AKKOR taggelő szabályok
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 4. Tanári ízlés-réteg (szubjektív tagek + rating, az objektív fölé rétegezve)
CREATE TABLE teacher_taste_overlays (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    media_id UUID NOT NULL REFERENCES media_items(id),
    teacher_id UUID NOT NULL,
    rating SMALLINT,                        -- pl. 1-6 (régi {oN} értékelés)
    taste_tags JSONB,                       -- tanár-specifikus al-tagek
    preference VARCHAR(20),                 -- 'favorite' | 'avoid' | 'neutral'
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (media_id, teacher_id)
);

-- 5. Golden Data / tanult tag-szabályok (feedback loop)
CREATE TABLE tag_learned_rules (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    constitution_id UUID REFERENCES constitutions(id),
    source_example TEXT,
    rule_text TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## 5. Rendszerszintű API / Worker Végpontok

*   `POST /api/scan` — gyűjtemény-lemérés indítása (worker), `media_items` feltöltése + hiány-térkép.
*   `POST /api/legacy/parse` — old-school fájlnevek RegEx-feltörése `media_tags`-be (`source='legacy_parse'`).
*   `POST /api/enrich/:mediaId` — web-lookup + audio-analízis (BPM/key/energia).
*   `POST /api/tag/:mediaId` — alkotmány-vezérelt címkézés (system → opcionális teacher overlay), AI fallback.
*   `GET|PUT /api/constitutions/:id` — Minősítő / Ízlés Alkotmány szerkesztése.
*   `POST /api/write-back/:mediaId` — jóváhagyott tagek ID3/metaadatba írása + letisztult fájlnév + Drive sync.
*   `GET /api/search` — multi-dimenziós tag-keresés / Smart Playlist lekérdezés.

---

## 6. DANA SSoT Illeszkedés (Top-Down ▼)

*   **KineLex (↔):** a táncelméleti fogalom-taxonómia (műfajok, stílusok, dialektusok) forrása lehet a KineLex fogalomtár — cross-module egyeztetés a tag-kulcsok harmonizálására.
*   **BeatPass (↔):** óratervhez/rendezvényhez kapcsolt zenei válogatás (Smart Playlist) átadása.
*   **Kettős névtér elv (D028 analógia):** a fájlnév a katalógus (`media_items` + `media_tags`) veszteséges projekciója; az SSoT a katalógus DB.
*   A felhasználói érték-fókusz és a Golden Data + AI hibrid a DANA modern alapelveihez (determinizmus elsőbbsége, feedback-vezérelt tanulás) illeszkedik.

> **[CROSS-MODULE DELEGATION] előkészítés:** a KineLex fogalomtár → MeCat tag-taxonómia kulcs-harmonizáció a Phase 3 előtt egyeztetendő.

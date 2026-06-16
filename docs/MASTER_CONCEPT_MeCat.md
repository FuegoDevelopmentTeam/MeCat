# MASTER_CONCEPT_MeCat.md — MeCat Modul-Alkotmány (Média-Katalógus & Tartalom-Sík)

Version: 2.3.0 (ZITA legacy integráció + CUE-adatbázis D093 + MeCat Lejátszó & Adapter-réteg D094 + `MeCat_Music_Tags.md` kanonikus taxonomia + DANA v1.36.0 Kulturális Mozgalom adopció: zene-dramaturgiai CUE & forgatókönyv→draft D107, generatív repertoár D105, közönség-társalkotó D102 — §14 + DANA v1.37.0 Kooperatív Szakmai Ív adopció: DJ kurátori kredencial D110, csendes hozzájáruló D114 — §15)
Date: 2026-06-15
Status: **BRAINSTORMING (Max-Concept)** — koncepció-fázis; az adatmodell és az API-k illusztratív tervezetek, nem véglegesítve.
Repo: https://github.com/FuegoDevelopmentTeam/MeCat (korábbi remote: MUSIC.git; korábbi munkanév: **ZITA**)
DANA SSoT: `../DANA/docs/MASTER_CONCEPT.md` — kanonikus döntés: **D083**, **D084**, **D086–D094**, **P49–P51**.

> **Forrásdokumentumok (megőrizve referenciaként):** `docs/MASTER_CONCEPT.md` (v9.0), `docs/zenei_tag_szotar.md`, `docs/kiindulasi_docs/music_tags.md`, **`docs/MeCat_Music_Tags.md`** (új, kanonikus tag-hierarchia). A régi `music_tags.md` **nem** SSoT — csak brainstorming-forrás.

> **DANA Platform-elv Adoptáció (v1.34.0 — P52–P62, D095):** A MeCat több új platform-elv **referencia-megvalósítása**: **P58** (a Minősítő ↔ Ízlés Alkotmány a **scoped-overlay kaszkád** kanonikus esete, a P49 általánosításaként); **P57** (a `constitutions.rules_json` determinisztikus taggelő szabályok a **közös Rules-as-Data policy-engine**-en); **P60/P50** (a komparatív ízlés-aggregátor a **közös reputáció-/Bradley–Terry-motoron**, nem külön); **P51** (tartalom-címezhető média + proveniencia — már beépítve). **Kötelező:** **P56/D095** (a média-scope [`shared`/`teacher`/`student-visible`, D092] a tenant-RLS + meta-org-buborék mentén; láthatóság ⟂ funkció); **P53** (jövőbeli jogdíj-bizonylat Money value object). 
> **Optimalizációs és Skálázási Paradigmaváltás (P63-P70):** **P65 (Graph-Native Knowledge):** A KineLex `term_id` hivatkozások és a CUE-adatbázis (D093) mély gráf-lekérdezéseket igényelnek, így a MeCat is a gráf adatbázis elvekre (Neo4j/AGE) támaszkodik a JSONB/relációs hivatkozások helyett.
> **Senior Review II Adoptáció (DANA v1.35.0 — P71–P82, D096):** **P72/P78** (komplexitás-költségvetés + szelektív ES): a média-katalógus **sima Postgres CRUD + audit-log**, **NEM** Event Sourcing (ott over-engineering); a `pgvector` szemantikus keresés (§4.1) és a Neo4j/AGE (P65) **alapból OFF**, csak mért triggerre (gráf-méret/latencia) aktiválódik — MVP-ben relációs `term_id` + JSONB elég. **P71** (Shapley): a komparatív ízlés-aggregátor (§4.10, P50) Bradley–Terry/Elo motorja a **közös** axiomatikus elosztó-/reputáció-családba tartozik (P60), nem külön. **P77** (Bounded Context): a MeCat Media-Plane egy Bounded Context; a KineLex→MeCat ontológia-kötés **Customer-Supplier** reláció verziózott data contracttal (P45), a tag-proveniencia (P51) az Anti-Corruption határ. **P79** (RLS): a média-scope (D092) buborék-RLS-e (P56) mellé app-réteg guard + izolációs teszt. (A pénzügyi P74/P75/P76/P80 a MeCat-ot csak a jövőbeli jogdíj-bizonylat eseménynél érinti, DrBill felé.)

---

## 0. Hogyan változott a DANA tér a MeCat megjelenésével (Top-Down ▼)

A DANA ökoszisztéma eddig négy „síkot” kezelt: **Pénz** (DrBill/WorkMan), **Emberek** (Core), **Foglalás/Erőforrás** (BeatPass) és **Tudás** (KineLex). A MeCat egy ötödik, eddig hiányzó síkot kanonizál:

> **A MeCat a DANA Média & Tartalom Sík (Media & Content Plane) kanonikus modulja.** Minden médiatétel (zene, ötlet-videó, koreográfia-referencia, hangminta) itt él, gazdagodik metaadattal, kap kereshető tageket, és innen szolgálja ki a többi modult.

Ez a sík **felszívja és egységesíti** a korábban szétszórt média-jellegű koncepciókat (lásd DANA D086):

> **Névtörténet:** A modul korábbi munkaneve **ZITA** (Zenei és Inspirációs Tár Asszisztense, D030) volt. A **MeCat** a kanonikus modulnév; minden ZITA-koncepció elem itt valósul meg — **nem** épül külön ZITA alkalmazás, és a „ZITA" **nem** belső almodul-neve, hanem **legacy alias** (migrációs kompatibilitás, commit-üzenetek, régi dokumentumok).

*   **D030 ZITA** teljes mandátuma → MeCat (lásd §1.3).
*   **ICE-003** (Zene-licencelési könyvtár) → MeCat **Jogtisztaság-réteg** (§7.2, D090).
*   **ICE-005 / ICE-006** → MeCat **Inspirációs Tár** (§7.1).
*   **D022 Choreo Workspace** és **D026 KineLex óraterv** → MeCat **média-backbone**.

**Integrációs irány (mit ad / mit kap a MeCat):**
| Modul | MeCat ad neki | MeCat kap tőle |
| :--- | :--- | :--- |
| **KineLex** (D084) | zenei „bizonyítékot" a fogalmakhoz (mely zenei szerkezet illik mely mozdulathoz) | a kanonikus tánc-ontológiát (műfaj/stílus/dialektus `term_id`-k) |
| **BeatPass** (D026/D042/D070/D058) | óraterv-/rendezvény-illesztett okos playlistet; hangosság-normalizált listát a terem-limiterhez | óraterv-kontextust, órarendet (diák-hozzáférés), rendezvény-igényt |
| **Core / Talent Graph** (D085) | a tanár „ízlés-ujjlenyomatát" mint hordozható kurátori kredenciált | tanár-identitást, RBAC/ReBAC scope-ot, careerStage-et |
| **DrBill / WorkMan** | (jövő) licenc-/jogdíj-bizonylat eseményt | — |

---

## 1. Rendszer-architektúra és Filozófia

A **MeCat (Media Categorizer)** lemér egy zenei/videó gyűjteményt, pótolja a hiányzó metaadatokat, és **kereshető, többdimenziós tagekkel** látja el a médiát, hogy a tánctanárok (és más felhasználók) pillanatok alatt megtalálják a megfelelő darabot (pl. „összes 5-csillagos haladó Bachata Sensual, BPM szerint növekvő, breaks-heavy musicality-vel"), illetve **automatikusan playlistet** kapjanak egy konkrét óratervhez vagy buli-ívhez.

### Paradigmaváltás: Old-School → Tag-natív → Tartalom-intelligencia
| Szempont | Régi prototípus (működik) | MeCat (új) |
| :--- | :--- | :--- |
| **Információ tárolása** | Mindent a **fájlnévbe** kódolva | Letisztult fájlnév (`Előadó - Cím.mp3`) + gazdag **metaadat-tagek** a katalógus DB-ben (SSoT) |
| **Példa** | `1,90-14 {o6} MEJOR . [mu 4 melody on2 descarga show] Joe Cuba - Quinto Sabroso.mp3` | `Joe Cuba - Quinto Sabroso.mp3` + tagek: `genre:salsa`, `bpm:190`, `dance:salsa_on2`, `rating(teacher):6`, `density:show`, `musicality:breaks_heavy`… |
| **Keresés** | Fájlnév-string, törékeny | Strukturált, multi-dimenziós tag-keresés + **szemantikus** (hangzás-vektor) keresés / Smart Playlist |
| **Minősítés** | Egy ember feje / kézi | **Két alkotmány**: rendszerszintű Minősítő + tanári Ízlés (P49) |
| **Bővítés** | Manuális | Web-lookup + audio-analízis (MIR) + AI-javaslat + **Golden Data** feedback (P43) |
| **Felhasználás** | „Megtalálom és lejátszom" | „A rendszer **összerakja nekem** az órám/bulim zenéjét és a diák **gyakorol** vele" |

### A modul két egyedi magja (a DANA-szintű újdonság)

#### 1.1. Két Alkotmány (Dual-Constitution) — kanonikus minta (DANA P49)
1.  **Rendszerszintű Minősítő Alkotmány (System Qualification Constitution):**
    *   Objektív, mérnöki és táncszakmai **sztenderd** tag-taxonómia és minősítési szabályok (BPM, hangnem, energia, ritmika, műfaj-hierarchia, MAP/ütemtérkép, hangminőség).
    *   Egy van belőle, az egész gyűjteményre érvényes „igazság-alap". Forrása táncfogalmilag a **KineLex ontológia (D084)**.
2.  **Tanárhoz Rendelhető Ízlés Alkotmány (Teacher Taste Constitution):**
    *   **Szubjektív** preferencia-réteg tanáronként: saját értékelés (rating), kedvelt/kerülendő stílusok, saját al-tagek, óratípushoz illő válogatás-súlyozás.
    *   Ugyanaz a szám **több tanárnál más-más** ízlés-tageket/ratinget kaphat, miközben az objektív réteg közös marad.
    *   A tanári réteg az objektív réteg fölé **rétegződik, nem írja felül** azt.

> **P49 (új DANA-elv):** *Objektív Kánon + Szubjektív Overlay.* A „rendszerszintű igazság + személyes preferencia-réteg" minta nem MeCat-specifikus: ugyanez a KineLex kánon ↔ tanári dialektus, a BeatPass rendszerár ↔ tanári override, és minden értékelés viszonya. A MeCat ennek a referencia-megvalósítása.

#### 1.2. A három értékelési dimenzió (a régi `{XY}` kiterjesztve)
A régi `{kezdő-haladó}` rating-et három, egyenként **0–6** skálájú dimenzióvá bontjuk (a user kérése szerint), és a Talent Graph-hoz (D085) kötjük:
*   **`rating_beginner` (0–6):** mennyire tetszik a **kezdő** táncosoknak (populáris/laikus fül).
*   **`rating_advanced` (0–6):** mennyire értékes a **haladó** táncosoknak/szakmának.
*   **`rating_student` (0–6):** a tanár tanítványai által adott közösségi tetszés-index (megosztott zenetár esetén).
*   **`+` (banger/STAR módosító):** kiemelt fénypont (óra/buli csúcspontja) — a peak-detektálás bemenete (§4.4, rendezvény D042/D070).

> A `rating_beginner` / `rating_advanced` közvetlenül illeszkedik a **diák skill-szintjéhez** (D074): a Gyakorló-Kísérő (§5.3) a tanuló szintjének megfelelő ratinget súlyozza.

#### 1.3. ZITA (legacy) örökség — teljes integrációs checklist (D030 → MeCat)

A D030 ZITA összes koncepcionális eleme és integrációs pontja az alábbi MeCat-funkcióban él (✅ = már lefedve ebben a dokumentumban):

| ZITA / D030 elem | MeCat megvalósítás | § |
| :--- | :--- | :--- |
| Zenei rendszerezés, metaadat, keresés | Tag-natív katalógus + Smart Playlist | §2, §3 |
| BPM detektálás | MIR enrichment | §2.3, §4.2 |
| Hangulat-alapú címkézés | `affect.*` taxonomia (kiterjesztett affekt-tér) | §3, `MeCat_Music_Tags.md` VI |
| Inspirációs gyűjtés | Inspirációs Tár (link/embed) | §7.1 |
| **API-First** belső/külső integráció | REST/event API + adapter-réteg | §9, §6 |
| D026 óraterv zenei javaslatok | Zenei-pedagógiai illesztőmotor | §5.1 (D088) |
| D022 koreó timeline zenei sávok | Playlist + CUE-pontok a timeline-hoz | §5.5, §7.1 |
| D022 zenevágás B-szint (mini editor) | MeCat Lejátszó vágási pont + export | §6.2 |
| D022 zenevágás C-szint (licencelt könyvtár) | Jogtisztaság-réteg + cleared catalog | §7.2 (D090) |
| D027 SafeFloor esemény zenei profil | Rendezvény/buli energia-ív + profil | §5.1, §4.4 |
| D006-D007 Marketplace önálló SaaS | MeCat mint önálló thin-slice termék (DJ-k, külső stúdiók) | §0, D086 |
| Önálló értékesíthető termék DJ-knek | Ugyanaz — API-first, headless (P34/P35) | §6.3 |

---

## 2. Fő Funkciók (End-to-End Pipeline)

### 2.1. Gyűjtemény-Lemérés (Collection Scan / Inventory)
*   Rekurzív végigjárás a zenei/videó tárhelyen; fájlonként **hash**, formátum, hossz, meglévő metaadat kiolvasása.
*   **Akusztikus ujjlenyomat (audio fingerprint)** a tartalom-alapú deduplikációhoz — nem csak a fájl-hash, hanem a *hangzás* alapján is felismeri a duplikátumokat, remixeket, gyorsított verziókat (lásd §4.5).
*   Hiány-térkép: mely fájlokból hiányzik az előadó/cím/műfaj/BPM/tag.

### 2.2. Legacy Fájlnév-Parszer (Old-School Migráció → Golden Data)
*   RegEx-motor, amely a régi, fájlnévbe kódolt sémát (`[Sebesség]-[index] {[értékelés]} [STÍLUS] . ([tagek]) Előadó - Cím`) feltöri, és a `docs/zenei_tag_szotar.md` szótár szerint **strukturált tagekké** alakítja (`source='legacy_parse'`).
*   A `salsa.txt`, `bachata.txt` stb. kiindulási listák a **Golden Data** magja (a tanár évek alatt felhalmozott tudása) → ez a tanuló-AI few-shot példatára (P43/P46).

### 2.3. Metaadat-Gazdagítás (Enrichment)
*   **Web-lookup:** hiányzó előadó/cím/műfaj/év kinyerése külső forrásokból.
*   **Audio-analízis (MIR — Music Information Retrieval):** BPM, hangnem (key), energia, ritmikai jellemzők (lásd a haladó motor, §4).

### 2.4. Alkotmány-Vezérelt Címkézés (Dual-Constitution Tagging — P43 hibrid pipeline)
*   **Determinisztikus szabály (Rules-as-Data) → AI-javaslat (csak a hiányzó/bizonytalan mezőkre) → emberi review → Golden Data feedback loop.** Determinizmus elsőbbsége az auditálhatóságért; az AI olcsó kiegészítő.
*   Először a rendszerszintű Minősítő Alkotmány tölti ki az objektív tageket; majd opcionálisan a kiválasztott tanári Ízlés Alkotmány adja a szubjektív réteget.
*   Bizonytalan AI-tagek emberi review-ra jelölve; minden tag **proveniencia-nyomot** kap (forrás + confidence + jóváhagyó), mint a DrBill/FUBI bizonylat-genealógia (D019 analógia).

### 2.5. Írás, Szinkron és Smart Playlists
*   A jóváhagyott tagek visszaírása metaadat-mezőkbe, letisztult fájlnévvel; az **eredeti fájlnév megőrizve** (visszaállíthatóság).
*   GWS/Drive szinkron a tanári laptopokra; AIMP Smart Playlist-ek automatikus frissülése; **offline-first tanári készlet** (P22 analógia: a stúdióban internet nélkül is működik).
*   Multi-dimenziós + szemantikus keresés (§4.1).

---

## 3. Tag-Taxonómia (14 dimenzió → 12 tematikus domain)

> **Kanonikus SSoT:** `docs/MeCat_Music_Tags.md` — 12 egyenlő távolságú tematikus domain (Identity, Tempo, Taxonomy, Taste, Vocal, Affect, Rhythm, Structure, **CUE**, Production, Contains, Usage). Az alábbi a user 14 pontjának **gyors indexe**; részletes értékkészlet és skálák ott.

A legacy források (`zenei_tag_szotar.md`, `kiindulasi_docs/music_tags.md`) **csak migrációs referencia**. A kanonikus tánc-fogalmak a **KineLex ontológiából** (`term_id`, D084) származnak.

### 3.A. Azonosító & alaptulajdonságok
1.  **Gyorsaság / Tempó** → `tempo` (BPM + módosítók: `altering`/`speedup`/`speeddown`), valamint **8-ütem-másodperc** (a régi „1,90" = egy 8-as ütem hossza mp-ben) derivált mező.
2.  **Válogatás-index** → `selection_index` (mennyire új a feldolgozott gyűjteményben; index vagy dátum).
6.  **Előadó – Cím (Feldolgozó DJ)** → `artist`, `title`, `remixer_dj` — **ez a letisztult fájlnév alapja** (`Előadó - Cím (DJ. Xy).mp3`).
4.  **Táncműfaj / zenei műfaj** → `genre` (`term_id` KineLex), `genre_group` (`latin_dance`/`ballroom`/`street`), `dance_style_timing` (`salsa_on1`/`salsa_on2`).
5.  **Alműfaj** → `subgenre` (`salsa_dura`, `salsa_romantica`, `bachata_sensual`…), `origin` (`cuban`/`new_york`/`puerto_rican`), `listening_difficulty` (`beginner`/`advanced`).

### 3.B. Minősítés (a két alkotmány találkozása)
3.  **Tetszés/Popularitás** → három dimenzió: `rating_beginner`, `rating_advanced`, `rating_student` (0–6) + `+` banger. → **Tanári Ízlés Alkotmány** (overlay), nem objektív.

### 3.C. Hangulati & strukturális („common") jelzők
7.  **Solo/Vokál performansz** → `vocal_melody` (`female`/`male`/`chorus`/`instrumental`), `vocal_language` (`en`/`sp`/`hu`/`instr`), `melody_quality`.
8.  **Intenzitás & Érzésvilág** → `energy_level` (very_low→fire), `mood` (a `music_tags.md` skálái: relax/calm/average/vivid/excited/fire + pozitív/negatív jelentés).
9.  **Keménység** → `hardness` (soft→extreme).
10. **Zsúfoltság** → `density` (a_capella→crazy).
11. **Ritmusjelző** → `rhythm` (marching/uniform/syncopated/floating/pulsing/monotone/bass) + `rhythm_signature` (3/4, 7/8, „strange").

### 3.D. Musicality (a dance-specifikus „korona-dimenzió" — §4.2-4.3)
12. **Musicality** →
    *   **Ütemtérkép (MAP):** a 8-as szerkezet okos elnevezése (`4/4-alapú` / `3/3-alapú` / `vegyes` / `strange`), illetve egyszerű/közepes/összetett a tört nyolcasok aránya szerint; kódolt forma: `map:[i4! v88 r4!4 4x4]` (intro/verse/refren/bridge…).
    *   **Ütem-ritmus változatosság:** a 8/4-es vagy 6/4-es csoportosításban az ütemeken belüli **irregularitás** — hányféle 8-as ritmust használ és mennyire változatosak (`homogen`→`difficult`/`hard`; `parts_diversity §`; `in_beat_qa`).
13. **Hangszerek & stílusjegyek** → `instrument` (piano/guitar/bass/percussion/synth/symphonic), `theme` (xmas/new_year…), `style_marker`.
14. **Felhasználás** → `usage` (warmup/footwork-descarga/show-revue/flat-practice/breaks-heavy/DJ-tools…) — **a pedagógiai cél**, ez köti a tageket az óratervhez (§5, D088).

### 3.E. Rendszer/minőség
*   `audio_quality` (old_record/low/poor_bitrate), `source_era` (retro/classic/modern/remix), `authenticity` (authentic/commercial), `loudness_lufs` (hangosság-normalizáláshoz, §4.6).

> **Metaadat-leképezés (illusztratív):** `GENRE`, `BPM`, `ARTIST`/`TITLE`, `GROUPING`, `MOOD`, `COMMENT`(MAP), `RATING`/`POPULARIMETER`(tanári), valamint custom mezők. A tanári Ízlés-tagek **tanár-prefixelt** custom mezőbe / a katalógus DB tanár-scope táblájába kerülnek, hogy ne ütközzenek a rendszerszintű tagekkel.

---

## 4. Haladó Motor — „a világ leghaladóbb szemlélete" (Brainstorming-jelöltek)

*Ezek a funkciók emelik a MeCat-ot a kategorizálóból tartalom-intelligencia platformmá. Mindegyik a P43 (hibrid) pipeline-ra és a Golden Data-ra épül; nem helyettesítik az emberi review-t, hanem skálázzák.*

### 4.1. Szemantikus & szöveg-alapú keresés (audio-embedding)
*   Önfelügyelt **hangzás-vektorok** (pl. CLAP/OpenL3-szerű) `pgvector` mezőben → „találj ehhez **hasonló hangzású**, de lassabb / Bachata számot".
*   **Text-to-music keresés:** „energikus mambo erős rezessel, shine-okhoz" — természetes nyelvű kérés → tag + vektor hibrid találat.

### 4.2. Ütem-rács & „az EGYES" detektálás (Beat-grid + downbeat)
*   Nem csak BPM: a **downbeat** és a páros-tánc **„1"-es** (on1/on2) felismerése → a zene rácsra illesztése a **tánc-számoláshoz**, nem csak a zenei ütemhez. Ez a dance-specifikus alap, amelyre minden más épül.

### 4.3. Musicality- & szerkezet-motor (a 12. dimenzió automatizálása)
*   **Szerkezet-szegmentálás** (intro/verse/chorus/bridge/break/solo) ön-hasonlósági mátrixból → automatikusan kitölti a **MAP**-et.
*   **Break/hit/kiállás-detektálás** (a régi „mu/emu" tudás): hol vannak a kiállások, mennyire váratlanok/erősek → `musicality` és `usage:breaks_heavy`.
*   **Irregularitás-mérés:** a 8-asokon belüli ritmus-változatosság számszerűsítése → a user 12-es „ütem-ritmus változatosság" pontja.

### 4.4. Energia-ív & csúcs-detektálás
*   **Ütem-szinkron energia-görbe** a szám hosszában → vizuális ív; a **csúcs(ok)** automatikus jelölése → `+` banger-jelölt; rendezvény-buli ív (D042/D070) és óra-ív (D088) építéséhez.

### 4.5. Tartalom-alapú dedup & verzió-leszármazás
*   Akusztikus ujjlenyomattal: ugyanaz a szám több formátumban/remixben/gyorsított verzióban → **verzió-fa** (eredeti ↔ DJ-edit ↔ slowed), mint a bizonylat-genealógia (D019).

### 4.6. Hangosság-normalizálás (LUFS) — BeatPass-integráció
*   A playlist **LUFS-normalizálása** → egyenletes hangerő az órán; közvetlen kapocs a BeatPass **intelligens hangerő-limiterhez (D058)**: a MeCat tudja, melyik szám „tolja meg" a termet, és a lista eleve normalizált.

### 4.7. Forrás-szeparáció (Source separation) — opcionális, haladó
*   Stem-leválasztás (pl. Demucs): a **clave/ütős** kiemelése shine-gyakorláshoz, vagy a **vokál** kiemelése musicality-feladathoz (a régi „melody on2 / descarga" igények digitálisan).

### 4.8. Harmonikus keverés (Camelot) — auto-DJ
*   Key + BPM (Camelot-kerék) alapú **átmenet-ajánló** óraterv-szekvenciához vagy buli-mixhez (auto-DJ asszisztens).

### 4.9. Tanult auto-taggelő + LLMOps-kormányzás (P46)
*   A Golden Data-ra (legacy listák) finomhangolt modell automatikusan javasol tageket; **eval-készlet** méri a tag-minőséget, **prompt-verziózás**, **tenant-szintű AI-költség budget**, hallucináció-monitoring.

### 4.10. Komparatív (páronkénti) ízlés-aggregátor — cross-module újrahasznosítás (P50)
*   A szubjektív tagek (pl. „melyik energikusabb, A vagy B?") feloldása a **D076 (Jack & Jill) komparatív bírálati motorral** (Bradley–Terry/Elo). Az emberek megbízhatóbban mondják meg az „A vagy B jobb"-at, mint az abszolút pontot → manipuláció-rezisztens, finomabb ízlés-térkép. **Ugyanaz a motor, más domain.**

---

## 5. Zenei-Pedagógiai Illesztőmotor & Diák Gyakorló-Kísérő (a koronaékszer-integráció)

### 5.1. Óraterv-illesztett playlist (DANA D088 — MeCat ↔ KineLex ↔ BeatPass)
A KineLex óraterv (D026) egy **fogalom-/mozdulat-sorozat** nehézséggel. A MeCat ehhez **automatikusan playlistet** generál, ahol minden szám zenei tulajdonsága illik a pedagógiai célhoz:
*   alaplépés tanulása → `usage:flat-practice`, lassú, `beginner` rating;
*   shine/footwork → `usage:descarga`, ütős, tiszta clave;
*   musicality-óra → `musicality:breaks_heavy`, MAP-gazdag;
*   levezetés → `usage:warmup/cooldown`, romantikus, alacsony energia.
Az eredmény egy **óra-energia-ív** (warmup → felépítés → csúcs → levezetés), a §4.4 energia-görbe és a tanári Ízlés Alkotmány súlyozásával.

### 5.2. Hasonló-lista ajánló (Golden Data alapján)
*   A tanár korábban bevált óratervi playlistjeihez **hasonló listákat ajánl** (a user kérése): a Golden Data (mit játszott + mi működött) + collaborative filtering / ízlés-vektor (§4.1) alapján — „a hozzád hasonló tanárok ezt is szeretik".

### 5.3. Diák Gyakorló-Kísérő (DANA D089 — MeCat ↔ BeatPass órarend)
A diákok a **BeatPass órarendjén keresztül** (D026/D070) hozzáférnek a tanár által az órán használt/elhangzott zenékhez, a gyakorlásuk támogatására:
*   „A mai órám zenéi" lista; szám-szintű **loop egy adott 8-asra**, **lassítás hangmagasság-tartással** (time-stretch), **számoló overlay** (a §4.2 „1"-detektálás alapján);
*   **spaced repetition** (P31): a rendszer emlékeztet a nehéz 8-as / musicality-feladat gyakorlására;
*   a diák szint-súlyozott ajánlást kap (a `rating_beginner/advanced` + D074 skill-szint alapján).

### 5.4. Megosztási hatókör-modell (DANA D092)
A médiatételeknek **scope**-juk van (a Core organization_id + RBAC D051 + ReBAC P04 mentén):
*   **`shared`** — klub / tánciskola / tanári szövetkezet szinten közös;
*   **`teacher`** — tanári hatáskörben saját (privát) rész;
*   **`student-visible`** — a tanár által megosztott, a diákok által (gyakorláshoz) elérhető részhalmaz.
Ugyanaz a fájl lehet közös objektív tagekkel, de **tanáronként eltérő Ízlés-overlay-jel** (§1.1).

### 5.5. CUE-adatbázis — Musicality-oktatás & DJ átmenetek (DANA D093)

A **zenei CUE-pont** a musicality-oktatás és a hangulat-menedzsment (DJ support) atomja: egy időben rögzített, **tematizált, kereshető** pillanat a számon belül.

**Két szint (P49 minta):**
1.  **Rendszer-szintű CUE-k (`scope=system`):** a Minősítő Alkotmány / MIR által jóváhagyott, objektív pillanatok (break, hit, downbeat, peak…) — az egész szervezetre érvényes „kánon".
2.  **Tanári CUE-k (`scope=teacher`):** szubjektív, pedagógiai megjegyzéssel („itt shine", „itt partnercsere") — az objektív CUE-k fölé rétegezve, nem felülírva.

**Funkciók:**
*   **Létrehozás & tárolás:** a MeCat Lejátszóban (§6) `@mm:ss` vagy `bar:N` alapján; `cue.type`, `cue.theme`, opcionális KineLex `linked_term_ids[]` (mozdulat/musicality fogalom).
*   **Tematizálás:** CUE-k csoportosítása témák szerint (pl. „salsa on2 break library", „bachata sensual texture shift") — kereshető könyvtár tanári és DJ munkafolyamathoz.
*   **Automatikus feldolgozás (MIR → CUE javaslat):** új zene enrichment után break/hit/peak/stop detektálás → `mir_suggested` CUE-k emberi review-ra.
*   **Hasonlóság-illesztés (CUE matching):** az automatikusan detektált zenei váltások összevetése a tárolt CUE-adatbázissal → **egyező** (magas match_score) vagy **hasonló** (`transition_family`) CUE **automatikus felvétele** a DB-be (alacsony confidence → review queue, P43).
*   **Golden Data feedback:** jóváhagyott CUE-párosítások → `cue_learned_rules` (a tag_learned_rules analógia).
*   **KineLex / Choreo integráció:** CUE-pontok hivatkozhatnak óraterv-szekcióra és koreó-timeline időkódjára (D022/D026) — ugyanaz a pillanat, három nézet (zene / mozdulat / óraterv).

Részletes CUE-típusok és mezők: `docs/MeCat_Music_Tags.md` §IX.

---

## 6. MeCat Lejátszó & Külső Adapter-réteg (DANA D094)

A CUE-rögzítés, musicality-oktatás, diák gyakorló-loop és a tanári munkafolyamat **saját lejátszót** igényel — ugyanakkor a meglévő DJ/tanári toolchain (Virtual DJ, AIMP, VLC…) **nem cserélhető le** egyik napról a másikra.

### 6.1. MeCat Natív Lejátszó (Primary Player)

*   **Web-alapú** (Next.js + Web Audio API / wavesurfer.js): hullámforma, beat-grid overlay, MAP/CUE jelölők vizuálisan.
*   **CUE-szerkesztő:** kattintás / gyorsbillentyű → CUE létrehozás; drag → finomítás; színezés system vs. teacher scope szerint.
*   **Órai mód:** számoló overlay (1–8), 8-as loop, hangmagasság-tartó lassítás (diák gyakorló, D089).
*   **DJ support mód:** Camelot/BPM kompatibilitás jelzés, mix-point (`cue.type=dj_mix_point`) ajánlás, energia-ív preview.
*   **Offline-first cache:** letöltött/készlet zenék stúdióban internet nélkül (P22).

### 6.2. Kompromisszumos Külső Adapter-réteg (External Player Bridge)

A MeCat **nem** próbálja helyettesíteni a DJ szoftvert teljes mélységben — **szinkronizál és exportál** hozzájuk, azok **nyelvezetével**:

| Külső app | Adapter képesség | MeCat ↔ külső kommunikáció |
| :--- | :--- | :--- |
| **AIMP** | Smart Playlist export (.xspf / AIMP playlist format), ID3 write-back, `COMMENT`=MAP | Fájl + meta szinkron Drive-on; playlist szabály export |
| **Virtual DJ** | VDJFolder / .m3u playlist, BPM/key mezők, CUE → VDJ cue point export (ahol formátum engedi) | Watch-folder / export könyvtár |
| **VLC** | .xspf / .m3u, egyszerű lejátszási lista | Fájl-hivatkozás + meta |
| **Rekordbox / Serato** | (Phase 6+) cue/export, ha API/formátum elérhető | ICEBOX |

**Elv:** A MeCat katalógus DB marad az **SSoT**; a külső app a **projekció/lejátszási végpont**. Kétirányú szinkron ahol lehetséges (pl. AIMP-ben módosított rating visszaolvasható — `source=external_sync`).

### 6.3. Headless / API-first (ZITA örökség, P34/P35)

*   A lejátszó UI **opcionális réteg** — a MeCat API-k (playlist, CUE, tag search) önállóan is fogyaszthatók külső frontenddel vagy marketplace integrációval (D006-D007).
*   Webhook/event: `PlaylistGenerated`, `CueApproved`, `MediaTagged` (P44).

---

## 7. Inspirációs Tár & Jogtisztaság (Média-sík kiterjesztés)

### 7.1. Inspirációs Tár (DANA: ICE-005/006 itt valósul meg)
*   **Link/embed-natív** ötlet- és koreográfia-videó gyűjtemény: a videó a forrásplatformon marad (embed), MeCat csak **linket + metaadatot + tageket** tárol → elkerüli a letöltés jogi kockázatát (DANA F6).
*   Saját feltöltésű ötlet-videók (a tanár sajátja) **mozdulat-tagekkel** (KineLex `term_id`) → a Choreo Workspace (D022) és az óraterv (D026) média-forrása.
*   (Jövő) auto-szegmentálás: videóban felismert mozdulatok → KineLex fogalom-javaslat („inspiráció → tananyag" pipeline).

### 7.2. Jogtisztaság & Licenc-réteg (DANA D090 — ICE-003 itt valósul meg)
*   Médiatételenként **licenc-státusz** (saját/jogtiszta-könyvtár/ismeretlen/korlátozott) és **felhasználási kör** (csak óra / rendezvényen is játszható).
*   Rendezvény-jegyértékesítésnél (D070) és nyilvános eseménynél a rendszer **figyelmeztet** a nem tisztázott jogállású zenére; jövőbeli jogdíj-bizonylat → DrBill esemény.

---

## 8. Tehetség-Gráf integráció — Kurátori Kredencial (DANA D091)

A tanári Ízlés Alkotmány + a bevált playlistjei egy **hordozható, igazolható kurátori képesség** (DJ/válogatói kredencial), amely a **Tehetség-Gráf (D085)** része:
*   a tanár „zenei ujjlenyomata" mint szakmai eszköz (endorsementtel, P30 verifiable credential);
*   a belső marketplace-en (D080) érték: „ehhez az eseményhez ilyen ízlésű kurátor kell";
*   utódlásnál (D079) a roster-tag **átveheti/tanulhatja** a mentor ízlés-alkotmányát (a „kockás papír" helyett strukturált átadás, D077 analógia).

---

## 9. Adatbázis Séma (illusztratív tervezet — control plane)

> Brainstorming-fázis: a séma a §1–8 koncepciót tükrözi, **nem véglegesített**. Tag-kulcsok: `MeCat_Music_Tags.md`. A tánc-fogalmak `term_id`-ja a KineLex ontológiára (D084) hivatkozik.

```sql
-- 1. Médiatételek (a gyűjtemény leképezése; SSoT)
CREATE TABLE media_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_id UUID,                   -- megosztási scope gyökere (D092)
    media_type VARCHAR(10) NOT NULL,        -- 'audio' | 'video' | 'inspiration_link'
    file_path TEXT,                         -- NULL ha link/embed (inspirációs tár)
    external_url TEXT,                       -- embed/link (ICE-006)
    file_hash VARCHAR(64),
    acoustic_fingerprint TEXT,              -- tartalom-alapú dedup (§4.5)
    artist VARCHAR(200), title VARCHAR(300), remixer_dj VARCHAR(200),
    duration_sec INT, bpm NUMERIC, music_key VARCHAR(10),
    loudness_lufs NUMERIC,                  -- D058 hangosság-normalizálás
    audio_embedding VECTOR(512),            -- szemantikus keresés (§4.1, pgvector)
    clean_filename VARCHAR(300),
    legacy_filename TEXT,
    license_status VARCHAR(20),             -- D090: own|cleared|unknown|restricted
    usage_scope VARCHAR(20),                -- class_only | public_ok
    scan_status VARCHAR(20) DEFAULT 'scanned', -- scanned|enriched|tagged|synced
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. Rendszerszintű objektív tagek (Minősítő Alkotmány kimenete; proveniencia)
CREATE TABLE media_tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    media_id UUID NOT NULL REFERENCES media_items(id),
    tag_key VARCHAR(40) NOT NULL,           -- genre|dance_style_timing|energy_level|musicality...
    tag_value VARCHAR(120) NOT NULL,
    term_id UUID,                            -- KineLex ontológia hivatkozás (D084), ha fogalmi
    source VARCHAR(20) DEFAULT 'rule',      -- rule|ai|human|legacy_parse
    confidence VARCHAR(10),                 -- high|low (review-jelölés)
    approved_by UUID,                        -- proveniencia (D019 analógia)
    UNIQUE (media_id, tag_key, tag_value)
);

-- 3. Struktúra-térkép (MAP / szegmensek; §4.2-4.3)
CREATE TABLE media_structure (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    media_id UUID NOT NULL REFERENCES media_items(id),
    map_code TEXT,                          -- pl. [i4! v88 r4!4 4x4]
    segments JSONB,                          -- [{type:'break', bar:33, strength:5}, ...]
    downbeat_grid JSONB,                     -- '1'-detektálás (on1/on2) §4.2
    energy_curve JSONB                       -- ütem-szinkron energia-ív §4.4
);

-- 4. Alkotmányok (rendszerszintű + tanári ízlés) — P49
CREATE TABLE constitutions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    scope VARCHAR(20) NOT NULL,             -- 'system' | 'teacher'
    teacher_id UUID,                        -- NULL ha scope='system'
    name VARCHAR(120) NOT NULL,
    rules_md TEXT,                          -- ember-olvasható szabályok
    rules_json JSONB,                       -- determinisztikus HA->AKKOR taggelő szabályok (P43)
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 5. Tanári ízlés-réteg (3 rating-dimenzió + szubjektív tagek; az objektív fölé rétegezve)
CREATE TABLE teacher_taste_overlays (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    media_id UUID NOT NULL REFERENCES media_items(id),
    teacher_id UUID NOT NULL,
    rating_beginner SMALLINT, rating_advanced SMALLINT, rating_student SMALLINT, -- 0-6
    is_banger BOOLEAN DEFAULT FALSE,        -- '+' fénypont
    taste_tags JSONB, preference VARCHAR(20), -- favorite|avoid|neutral
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (media_id, teacher_id)
);

-- 6. Playlistek (óraterv-illesztett, smart, golden) — D088
CREATE TABLE playlists (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    owner_teacher_id UUID, organization_id UUID,
    kind VARCHAR(20),                       -- lesson|event|smart|golden
    lesson_plan_ref UUID,                    -- KineLex óraterv (D026) hivatkozás
    rules_json JSONB,                        -- smart playlist szabály (auto-frissül)
    energy_arc JSONB                         -- óra/buli energia-ív (§4.4/§5.1)
);

-- 7. CUE-adatbázis (D093 — system + teacher scope)
CREATE TABLE cue_points (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    media_id UUID NOT NULL REFERENCES media_items(id),
    scope VARCHAR(20) NOT NULL,             -- system | teacher | organization
    teacher_id UUID,
    cue_time_sec NUMERIC NOT NULL,
    cue_bar INT,
    cue_type VARCHAR(40) NOT NULL,          -- break|hit|downbeat|dj_mix_point|...
    cue_theme VARCHAR(120),
    label VARCHAR(200),
    linked_term_ids UUID[],                 -- KineLex term_id[]
    strength SMALLINT,                      -- 0-6
    teaching_note TEXT,
    transition_family UUID,
    source VARCHAR(20) DEFAULT 'human',     -- human|mir_auto|mir_suggested|import_legacy|external_sync
    confidence VARCHAR(10),
    match_score NUMERIC,                    -- auto-match eredmény
    approved_by UUID,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 8. Golden Data / tanult CUE-szabályok (feedback loop)
CREATE TABLE cue_learned_rules (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    source_cue_id UUID REFERENCES cue_points(id),
    rule_text TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 9. Golden Data / tanult tag-szabályok (feedback loop; P43)
CREATE TABLE tag_learned_rules (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    constitution_id UUID REFERENCES constitutions(id),
    source_example TEXT, rule_text TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## 10. Rendszerszintű API / Worker Végpontok (illusztratív)

*   `POST /api/scan` — gyűjtemény-lemérés + hash + akusztikus ujjlenyomat + hiány-térkép.
*   `POST /api/legacy/parse` — old-school fájlnevek RegEx-feltörése → `media_tags` (`legacy_parse`) + Golden Data.
*   `POST /api/enrich/:mediaId` — web-lookup + MIR (BPM/key/energia/szerkezet/energia-ív).
*   `POST /api/tag/:mediaId` — alkotmány-vezérelt címkézés (system → opcionális teacher overlay), AI fallback, review-jelölés.
*   `GET|PUT /api/constitutions/:id` — Minősítő / Ízlés Alkotmány szerkesztése.
*   `POST /api/playlists/from-lesson` — **óraterv→playlist** generálás (D088, KineLex óraterv input).
*   `GET /api/search` — multi-dimenziós tag + szemantikus (vektor) keresés / Smart Playlist.
*   `POST /api/write-back/:mediaId` — jóváhagyott tagek metaadatba írása + letisztult fájlnév + Drive sync (eredeti megőrzve).
*   `GET|POST /api/cues/:mediaId` — CUE-pontok CRUD (D093); batch import MIR javaslatból.
*   `POST /api/cues/match` — automatikus CUE-illesztés (detektált váltás ↔ CUE DB).
*   `POST /api/export/playlist/{format}` — külső lejátszó export (aimp|vdj|m3u|xspf, D094).
*   `GET /api/player/session` — natív lejátszó session (beat-grid, CUE overlay, loop config).

*   `GET /api/student/today` — diák gyakorló-nézet a BeatPass órarend alapján (D089).

---

## 11. DANA SSoT Illeszkedés & Döntés-térkép (Top-Down ▼)

| DANA döntés/elv | Mit jelent a MeCat-nak |
| :--- | :--- |
| **D083** | MeCat kanonizálva mint média-katalógus, kétszintű címkézés. |
| **D084** | A tag tánc-fogalmai a KineLex ontológiából (`term_id`), nem duplikálva. |
| **D086** | MeCat = Média & Tartalom Sík; **ZITA (D030) legacy név, teljes realizálás**. |
| **D088** | Zenei-pedagógiai illesztőmotor (óraterv→playlist, energia-ív). |
| **D089** | Diák gyakorló-kísérő a BeatPass órarendhez. |
| **D090** | Média jogtisztaság & licenc-réteg (ICE-003). |
| **D091** | Kurátori kredencial a Tehetség-Gráfban (D085). |
| **D092** | Megosztási hatókör-modell (shared/teacher/student-visible). |
| **D093** | CUE-adatbázis: system+teacher CUE-k, tematizálás, auto-match. |
| **D094** | MeCat natív lejátszó + külső adapter-réteg (AIMP/VDJ/VLC). |
| **P43/P46** | Hibrid determinisztikus-AI pipeline + LLMOps-kormányzás. |
| **P49** | Objektív Kánon + Szubjektív Overlay (tag + CUE két szinten). |
| **P50** | Komparatív ízlés-aggregátor — D076 motor újrahasznosítva. |
| **P51** | Tartalom-címezhető média + proveniencia. |
| **D058** | Hangosság-normalizált playlist a terem-limiterhez. |
| **D022/D026/D027/D042/D070** | Média-backbone: koreó, óraterv, SafeFloor, rendezvény. |

---

## 12. Cross-Module Delegáció (ELKÜLDVE — 2026-06-15)

> A delegációk **közvetlenül** a célmodulok inbox-fájljaiba kerültek (User engedélyével). Státusz: **OPEN**, válasz a célmodul Tech Lead-jétől várható.

### MeCat → KineLex
*   **Fájl:** `../KineLex/docs/INBOX_CROSS_MODULE_DELEGATION_MeCat.md`
*   **Tárgy:** ontológia-export API (P45), MAP/musicality harmonizáció, CUE↔fogalom kötés (D093), óraterv zenei slot-séma (D088).

### MeCat → BeatPass
*   **Fájl:** `../BeatPass/docs/INBOX_CROSS_MODULE_DELEGATION_MeCat.md`
*   **Tárgy:** óraterv-playlist kötés (D088), diák gyakorló API (D089), LUFS→limiter (D058), esemény zenei profil, scope (D092), domain-események (P44).

---

## 13. Megnyitott kérdések (ICEBOX — brainstorming)
*   **♥ M-ICE-01:** A diák gyakorló-loop hangmagasság-tartó lassítása szerveroldali render vagy kliens-oldali (WebAudio) legyen-e?
*   **♥ M-ICE-02:** A kurátori kredencial (D091) mennyire legyen nyilvános a marketplace-en (privacy vs. felfedezhetőség, P06)?
*   **♥ M-ICE-03:** Auto-szegmentált videó-mozdulatfelismerés (§7.1) — saját modell vagy külső szolgáltatás; mikor reális (Phase 5+)?
*   **♥ M-ICE-04:** A három rating-dimenzió súlyozása az óraterv-illesztésnél tanáronként hangolható legyen-e (Ízlés Alkotmány paraméter)?
*   **♥ M-ICE-05:** Rekordbox/Serato adapter prioritása (D094 Phase 6+)?
*   **♥ M-ICE-06:** CUE auto-match küszöb (θ) — mennyire legyen agresszív az automatikus felvétel?

---

## Függelék A. Legacy környezet (megőrzött kontextus — LPDPC Master Gép)
A korábbi prototípus (`docs/MASTER_CONCEPT.md` v9.0) egy dedikált **LPDPC Master gépen**, AIMP + mp3tag + Virtual DJ + GWS/Drive sync köré épült, a tagek az ID3v2 mezőkben, a Smart Playlistek a tanári laptopokon. A MeCat ezt **nem közvetlenül folytatja**: a katalógus DB lesz az SSoT, az ID3/fájlnév annak (a régivel kompatibilis) projekciója; az AIMP/Drive-sync mint **kimeneti adapter** marad támogatva. A salsa.txt/bachata.txt listák Golden Data-ként migrálva.

---

## 14. DANA v1.36.0 Adopció — Zene-Dramaturgia & Generatív Repertoár (Top-Down ▼, D097-D109)

A MeCat a „Kulturális Mozgalom" kör (DANA D097-D109) **tartalom-intelligencia** vonatkozásait adoptálja. A MeCat e körben több döntés **referencia-megvalósítása**.

### 14.1. D107 — Zene-Dramaturgiai (Hangulati) Térkép & Drámai CUE-pontok `[Érintett: KineLex, BeatPass]`
A meglévő objektív szerkezet- (§4.3) és energia-térkép (§4.4) mellé a MeCat **dramaturgiai (hangulati) réteget** is épít:
*   **Drámai CUE-típusok (a D093 CUE-DB bővítése):** a `cue_type` kiegészül drámai/affekt-osztállyal — `tension_build`, `climax`, `release`, `mood_shift`, `catharsis`, `suspense`. A forrás az affekt-térkép (`affect.*`, `MeCat_Music_Tags.md` VI) + az energia-ív (§4.4).
*   **Kétszintű (P49):** rendszer-szintű drámai CUE (MIR-detektált, objektív) + tanári drámai CUE (szubjektív, pedagógiai/művészeti megjegyzéssel) — az objektív réteg fölé rétegezve.
*   **Zene-dramaturgiai térkép:** a szám teljes hosszán a feszültség→feloldás ív vizualizálva a Lejátszóban (§6.1), a peak-/break-detektálással (§4.4) összekötve.

### 14.2. D107 — Forgatókönyv→Draft Pipeline (KineLex ↔ MeCat ↔ belső vágó)
A KineLex **koreográfia-forgatókönyvéhez** (dramaturgiai ív + szekciók, cél-affekt + `term_id` mozdulat-igény) a MeCat **automatikusan felajánl** illő zenei részeket ÉS Inspirációs Tár videó-mozdulatokat (§7.1):
*   **Dramaturgia-vezérelt szekvenálás (P83):** az illesztőmotor (D088) kiterjesztése — nemcsak `usage`/nehézség, hanem **cél-affekt és drámai hatás** szerint illeszt (a forgatókönyv szekcióinak affektjeihez).
*   **Draft-generálás (belső videóvágó):** a MeCat Lejátszó (§6.1) + a D022 zenevágás B-szint kiterjesztése összefűzi a zenei + videó-szegmenseket bemutatható **draft**-tá (2D timeline + videó-collage); jogtisztaság a D090 (§7.2) szerint.
*   **3D-roadmap (Phase-gate, P72):** a teljes 3D színpadi koreografálás a **KineLex** §15 (Kreatív Koreográfia-Tervező & 3D Show) hatásköre; a MeCat a zenei/CUE/affekt-sávot szolgáltatja hozzá. A 3D-render explicit innovációs-zseton mögött, késői fázisban.

### 14.3. További érintett döntések
*   **D105 (Generatív Repertoár):** több tanár ízlés-alkotmányából (P49) + dialektusából (KineLex D077) + bevált playlistjeiből (Golden Data, §5.2) show-koncepció-javaslat; LLMOps-kormányzással (P46).
*   **D102 (Közönség mint Társalkotó):** a komparatív ízlés-aggregátor (§4.10, P50) megnyitva a diákoknak/közönségnek; `rating_student` (D086) + gráf-reputáció (P60).
*   **D098 (On-Ramp):** a `rating_beginner` dimenzió + Gyakorló-Kísérő (D089) a laikus-barát első élményhez.

> **Felszólítás (a User közvetíti):** a dramaturgiai CUE-réteg és a forgatókönyv-illesztő implementációja a MeCat Tech Lead hatásköre; a koreográfia-forgatókönyv séma + 3D-koreografáló a KineLex-szel egyeztetendő (`[CROSS-MODULE DELEGATION]`), a draft-vágó jog-tisztasága a D090 mentén.

---

## 15. DANA v1.37.0 Adopció — DJ Kurátori Kredencial & Csendes Hozzájáruló (Top-Down ▼, D110-D120)

A MeCat a kooperatív szakmai ív **kurátori/médiakurátori** vonatkozásait adoptálja (a kurátori kredencial D091 és az Ízlés Alkotmány P49 gazdája).

*   **D110 (Rendezvényi DJ kulcs-szerep):** a rendezvényre **előre lefoglalt DJ** zenei alkalmasságát/illesztését a **kurátori kredencial** (D091) + az esemény energia-/dramaturgiai ív (§4.4 / §14.1) adja; a MeCat szolgáltatja a rendezvény-profilhoz illő, LUFS-normalizált (D058) playlistet, és a foglaló BeatPass felé az „ehhez az eseményhez ilyen ízlésű kurátor kell" jelet (§8). A DJ így a rendezvény kritikus, kereshető és foglalható erőforrása.
*   **D114 (Csendes Hozzájáruló):** a Tanári/kurátori Ízlés Alkotmány (P49/D087) + kredencial (D091) **brand-független** — a hozzájáruló kurátor a Brand-Tárcsát (D114) „csendes"-re állítva is gyűjti az értéket (royalty/equity), a neve elrejtésével (privacy vs. felfedezhetőség, vö. M-ICE-02).

> **Felszólítás (a User közvetíti):** a DJ-kredencial foglalási integrációja a MeCat Tech Lead hatásköre; a rendezvényi előre-foglalás a BeatPass-szal egyeztetendő (`[CROSS-MODULE DELEGATION]`).

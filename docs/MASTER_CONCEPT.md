# MUSIC Zenei Tag Generálás - Összevont Kontextus Napló

**Dátum:** 2026. május 29.
**Verzió:** 9.0 (Részletes Tag Rendszer és AI Koncepció)
**Tulajdonos / Szervezet:** FUEGO KULTURALIS MUEVESZETI TANC ES SPORT EGYESUELET (Nonprofit Google Workspace)
**Cél:** Dedikált videóvágó, programozó, AI hanggeneráló munkaállomás (LPDPC) felépítése, valamint a Fuego KMTSE táncstúdió zenei könyvtárának modernizálása, metaadat-alapú rendszerezése és GWS szinkronizációja a tánctanárok felé.

---

## 1. Hardver és Szoftver Alapok (LPDPC Master Gép)

### 1.1. Hardver Specifikációk (BTO PC)

- **Alaplap:** ASUS ROG STRIX B860-I GAMING WIFI
- **Processzor (CPU):** Intel Core Ultra 9 285K 3700 1851 BOX
- **Processzor hűtés:** ASUS ROG STRIX LC II 280 ARGB
- **Videókártya (GPU):** ASUS 16GB D7 RTX 5080 OC PROART
- **Memória (RAM):** 64GB (2x32) DDR5 6600-32 Corsair Vengeance
- **Tárhely:** 2 db 4TB Samsung 990 PRO M.2 SSD (Összesen 8TB)
- **Tápegység:** CORSAIR SF1000
- **Számítógépház:** Sharkoon REBEL C20 ITX

### 1.2. Perifériák és Megjelenítők

- **Elsődleges kijelző:** Samsung QE65S95F (65" OLED TV)
- **Hangkártya / Interfész:** Audient iD14 (stúdió minőségű D/A konverzió)
- **Beviteli eszközök:** Logitech MX Keys S billentyűzet, Logitech MX Ergo egér

### 1.3. Szoftveres Környezet (Alapok)

- **Operációs Rendszer:** Windows 11
- **Alap Szoftverek:** Norton 360, Veeam Agent, NVIDIA Driver (596.36), Intel RST, Audient USB Audio Driver (v5.5.1).
- **Munka / Kreatív:** DaVinci Resolve 21.0, VirtualDJ 2026, Sidify, AIMP (Elsődleges zenelejátszó), Spotify, Adobe CC.
- **Fejlesztés:** MS Windows Desktop Runtime (8.x, 9.x), OpenSSL v3.0.0, Power Automate, Terminal.

---

## 2. Fuego KMTSE Zenei Rendszer - Projekt Áttekintés

A projekt célja a korábbi, Virtual DJ és fájlnév-alapú zenei rendszerezés továbbfejleszése, esetleg leváltása egy AIMP-alapú, stúdió minőségű, metaadatokon (ID3v2 TAG-eken) nyugvó intelligens adatbázisra, amelyet Google Workspace (GWS) Drive-on keresztül szinkronizálunk a tánctanárok laptopjaira.

### 2.1. A Rendszerezés Evolúciója

**Régi "Old School" Rendszer:**
Minden adat a fájlnévbe volt kódolva: `[Sebesség/8-ütem mp-ben]-[Válogatás index] {[Kezdő][Haladó értékelés]} [STÍLUS] . ([Tag-ek]) [Előadó] - [Cím].mp3`
*(Példák: `1,90-14 {o6} MEJOR . [mu 4 melody on2 descarga show] Joe Cuba - Quinto Sabroso.mp3` vagy `3,63-22 {o3} BACHATA . (LQ flat marching) Luciano Pereyra - Si No Es Muy Tarde.mp3`)*

**Új Koncepció:**
A fájlnevek letisztulnak (`Előadó - Cím.mp3`), a komplex szakmai, táncpedagógiai és mérnöki információk pedig bekerülnek a kereshető meta TAG-ekbe.

---

## 3. Táncszakmai és Mérnöki Tagging Rendszer

### Alapelvek:
*   Az "induló TAG készlet" egyértelmű, egyszavas kulcsszavakat preferál, ahol lehetséges.
*   Ahol szükséges, ott a specifikusabb információk zárójelben, vagy al-tagekként jelennek meg.
*   Az AI-val történő finomítás és a "golden data" segítségével történő manuális asszisztencia a rendszer kulcsfontosságú része lesz.

### 3.1. Fő, nyilvántartott alaptulajdonságok
Ezek azok az alaptulajdonságok, amelyek minden zeneszám esetében rögzítésre kerülnek és a legfontosabbak az azonosításhoz és kereséshez.

*   **Tempó (`tempo`):**
    *   Formátum: `BPM` (pl. `120`).
    *   Módosítók: `tempo_altering`, `tempo_speedup`, `tempo_speeddown`.
*   **Összetett tánc/zenei műfaj kategóriák (DMC dance / music categories):**
    *   **Fő műfajok (Primary Genres):** `salsa`, `chachacha`, `mambo`, `guajira`, `cumbia`, `jazz`, `slowfox`, `waltz`, stb.
    *   **Műfaj csoportosítás (Genre Grouping):** `latin_dance`, `ballroom_dance`, `street_dance`.
    *   Például: `genre:chachacha`, `genre_group:latin_dance`.
*   **Előadó - Cím (`artist_title`):**
    *   Külön tag: `artist`, `title`.
*   **DJ neve, ha feldolgozásról van szó (`remixer_dj`):**
    *   Tag: `remixer_dj` (pl. `(DJ. Xy)`).

### 3.2. DMC kategória jelzők (hierarchia és mellérendelés)

*   **Tánc stílus/időzítés (`dance_style_timing`):**
    *   Például: `salsa_on1`, `salsa_on2`, `salsa_on1on2`.
*   **Tánc műfaji alcsoportok (Sub-genres):**
    *   Példák: `salsa_dura`, `salsa_romantica`, `chachacha_boogaloo`, `jazz_standard`, `jazz_modern`.
*   **Kulturális/regionális eredet (`origin`):**
    *   Példák: `origin:cuban`, `origin:new_york`, `origin:puerto_rican`.
*   **Nehézségi szint (a hallgató fülének) (`listening_difficulty`):**
    *   `level:beginner` (B)
    *   `level:advanced` (A)

### 3.3. DMC független "hangulati" és "strukturális" ("common") jelzők

Ezek olyan általános attribútumok, amelyek nem kötődnek szorosan egy adott táncstílushoz.

*   **Hangulat (`mood`):**
    *   Példák: `mood:relax`, `mood:energetic`, `mood:romantic`, `mood:depressive`.
*   **Strukturális jelzők (`structure`):**
    *   **Melódia/Vokál (`vocal_melody`):** `melody_quality:bad`, `melody_quality:excellent`, `vocal_gender:female`, `vocal_type:chorus`, `vocal_type:instrumental`.
    *   **Intenzitás/Energia (`energy_level`):** `energy_level:very_low` - `energy_level:high/fire`.
    *   **Keménység (`hardness`):** `hardness:soft` - `hardness:extreme`.
    *   **Sűrűség (`density`):** `density:a_capella` - `density:crazy`.
    *   **Ritmika (`rhythm`):** `rhythm:marching`, `rhythm:syncopated`, `rhythm:monotone`, `rhythm:bass`.
    *   **Zenei elemek (`instrument`, `theme`):** `instrument:piano`, `instrument:synth`, `theme:christmas`.
    *   **Hangminőség (`audio_quality`):** `quality:old_record`, `quality:low`, `quality:poor_bitrate`.
    *   **Forrás/Korszak (`source_era`):** `era:retro`, `source:remix`.
    *   **Autenticitás (`authenticity`):** `authenticity:authentic`, `authenticity:commercial`.

### 3.4. DMC tánc/zene műfaj alá tartozó specifikus jelzők

Ezek a fő műfajokhoz kötött, finomabb részleteket tartalmazzák.

*   **Tánc specifikus strukturális elemek:**
    *   `vocal_language`: `lang:english`, `lang:spanish`, `lang:instrumental`.
    *   `rhythm_signature`: `rhythm_signature:3_4` (waltz time), `rhythm_signature:7_8`.
    *   `beat_clarity`: `beat_clarity:clear_beat`.
    *   `musical_flow`: `flow:stream`, `flow:floating`, `flow:pulsing`, `flow:marching`.
    *   **MAP (nyolcas térkép):** `map:[i4! v88 r4!4]`.
    *   **Zenei részek jelölése (`part`):** `part:intro`, `part:verse`, `part:refrain_accented_ending`.
*   **Musicalitás (`musicality`):** `musicality:complex`, `musicality:simple`, `musicality:in_beat_qa`, `musicality:inhomogeneity`, `musicality:overall_complexity`, `musicality:parts_diversity`.

---

## 4. Véglegesített Metaadat Leképezés (Tag Szótár)

Az új rendszerben a fenti kódrendszer az alábbi ID3v2 mezőkbe kerül automatizálásra:

| Funkció                       | ID3v2 Mező          | Példa / Formátum                             | Leírás                                                               |
| :---------------------------- | :------------------ | :------------------------------------------- | :------------------------------------------------------------------- |
| **Fő műfaj / Tánc stílus**    | `GENRE`             | `Salsa`, `Chachacha`, `Jazz`                 | A zene alapvető tánc vagy zenei műfaja.                              |
| **Tempó (BPM)**               | `BPM`               | `120`                                        | A zene sebessége Beats Per Minute-ben.                               |
| **Előadó**                    | `ARTIST`            | `Joe Cuba`                                   | A zeneszám előadója.                                                 |
| **Cím**                       | `TITLE`             | `Quinto Sabroso`                             | A zeneszám címe.                                                     |
| **Remixelő DJ**               | `REMIXER`           | `DJ. Xy`                                     | Remixelő DJ neve, ha van.                                            |
| **Műfaj Csoportosítás**       | `GROUPING`          | `latin_dance`, `ballroom_dance`              | A fő műfaj tágabb csoportja.                                         |
| **Tánc Stílus/Időzítés**      | `CUSTOM_DANCE_STYLE`| `salsa_on2`, `chachacha_boogaloo`            | Specifikus táncstílus és időzítés.                                   |
| **Regionális eredet**         | `CUSTOM_ORIGIN`     | `origin:cuban`, `origin:new_york`            | A zene kulturális/regionális eredete.                                |
| **Nehézségi szint**           | `CUSTOM_LEVEL`      | `level:beginner`, `level:advanced`           | Nehézségi szint (hallgató fülének).                                  |
| **Hangulat**                  | `MOOD`              | `energetic`, `romantic`, `depressive`        | A zene érzelmi töltése.                                              |
| **Energia szint**             | `CUSTOM_ENERGY`     | `energy_level:high`                          | A zene intenzitása és energiája.                                     |
| **Hangminőség**               | `CUSTOM_QUALITY`    | `quality:old_record`, `quality:low`          | A felvétel minősége.                                                 |
| **Korszak/Forrás**            | `CUSTOM_ERA`        | `era:retro`, `source:remix`                  | A zene korszaka vagy forrása (pl. remix).                            |
| **Autenticitás**              | `CUSTOM_AUTH`       | `authenticity:authentic`, `authenticity:commercial`| A zene autenticitása.                                                |
| **Vokál nyelv**               | `LANGUAGE`          | `lang:spanish`, `lang:instrumental`          | A vokál nyelve.                                                      |
| **Ritmika**                   | `CUSTOM_RHYTHM`     | `rhythm:syncopated`, `rhythm:marching`       | A zene ritmikai jellegzetességei.                                    |
| **MAP / Ütemtérkép**          | `COMMENT`           | `map:[i4! v88 r4!4]`                         | A zene szerkezeti térképe és egyéb kódolt információk.                |
| **Zenei részek**              | `CUSTOM_PARTS`      | `part:intro`, `part:refrain_accented_ending` | A zeneszám specifikus részei.                                        |
| **Musicalitás**               | `CUSTOM_MUSICALITY` | `musicality:complex`, `musicality:parts_diversity`| A zene komplexitása és diverzitása.                                  |

---

## 5. Munkafolyamat és Akcióterv

### Tanári Lejátszási Logika (Workflow)

1. A tanár a bal oldali sávban (Grouping Tree) kiválasztja az útvonalat (pl. `latin_dance` -> `Salsa` -> `salsa_on2`).
2. Az AIMP kilistázza a megfelelő zenéket, **automatikusan BPM szerint növekvő** sorrendben.
3. A `COMMENT` mező vizuális kijelzésével a tanár lejátszás közben azonnal látja a zene mérnöki energiaszintjeit és az ütemtérképét (MAP).
4. A **Válogatás index** (`CUSTOM1` vagy `PUBLISHER` mező) és a **Rating (csillagok)** oszlopként jelennek meg a nézeten, hogy a tanár egy pillantásra lássa, melyik a bevált, magas minőségű darab.

### Smart Playlists (Előre definiált Okos Listák)

Az AIMP "Smart Playlist" funkciójával előre bekonfiguráljuk a legtöbbet használt kombinációkat:

- **Pl.:** *"Összes 5 csillagos Bachata Sensual, BPM szerint növekvő sorrendben"*
- **Pl.:** *"latin_dance Salsa Footwork, Top Rated, Lassú → Gyors"*
- **Automatikus frissülés:** Amint a Master gépen egy új fájl megkapja a megfelelő TAG-eket, a Drive szinkronon keresztül **automatikusan megjelenik** a tanári laptop megfelelő okos listájában, a helyes sebességpozícióban.

### Következő Lépések a LPDPC Gépen

1.  **RegEx Script:** Egy Reguláris Kifejezés (RegEx) motor vagy mp3tag akciócsoport megírása, amely a meglévő (salsa.txt, bachata.txt stb.) régi fájlneveket feltöri, és az információkat a fenti táblázatnak megfelelően a TAG-ekbe írja.
2.  **AIMP Sablon (Skin/View):** Olyan egyedi felület és nézet kialakítása az LPDPC-n, amely a `GROUPING`, `BPM`, `MOOD` és legfőképp a `COMMENT` mezőt tökéletesen láthatóvá teszi.
3.  **GWS Szinkronizáció:** Az AIMP Music Library adatbázisfájljának és a zenéknek a felhős szinkronizációs tesztje.
4.  **Fejlesztői Környezet / AI Kiegészítés:** Az eredeti céloknak megfelelően (ha a zenei rendszer stabil) az AI keretrendszerek és az IDE-k konfigurálása az LPDPC-n.
    *   **AI alapú tag generálás (Jövőkép):** Egy tool fejlesztése, amely képes FLAC fájlból alapvető zenei jellemzőket (BPM, kulcs, energia) kinyerni, webes keresés alapján előadó/cím/műfaj metaadatokat gyűjteni.
    *   **"Golden Data" Finomítás:** Az emberi beavatkozással létrehozott "golden data" adatok alapján finomítani egy gépi tanulási modellt a táncoktatási specifikus tag-ek automatikus javaslatához.

---

*LPDPC Master Context - Utoljára frissítve: 2026.05.29. v9.0 (Részletes Tag Rendszer és AI Koncepció)*
*Előző verziók: v1.0–v5.0 (Music Organisator logok), v6.0 (2026.05.10. Összevont Szintézis), v8.0 (2026.05.29. Bővített: Jazztánc TAG-ek alapjai, finomított hierarchia, AI koncepció)*
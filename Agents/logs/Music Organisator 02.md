# Fuego KMTSE - Zenei Rendszer és Lejátszási Architektúra (V2)

**Dátum:** 2026. május 10.
**Verzió:** 2.0 (Bővített leíró és tanári kategória rendszer)
**Kapcsolódó dokumentum:** LPDPC Setup Thread - v1.0
**Projekt:** Zenei könyvtár modernizálása, metaadat-alapú rendszerezés és táncszakmai specifikáció.

---

## 1. Hardver és Szoftver Alapok (LPDPC Master Gép)
* **Környezet:** Intel Core Ultra 9 285K, 64GB RAM, 8TB SSD NVMe.
* **Audio Interfész:** Audient iD14 (stúdió minőségű lejátszás).
* **Lejátszó:** AIMP (Master) + AIMP/Laptop (Tanárok).
* **Szinkronizáció:** Google Workspace Drive.

## 2. Táncszakmai Tagging Rendszer (ÚJ)
A cél, hogy a tánctanár pillanatok alatt megtalálja az óra adott részéhez (pl. bemelegítés vs. koreográfia) és tematikájához (pl. izoláció vs. zeneiség) illő zenét.

### 2.1. Táncóra Részei (Strukturális Tag-ek)
Javasolt TAG mező: `CONTENT GROUP` vagy `OCCASION` (AIMP-ben egyéni oszlopként megjeleníthető).
* **Warm-up (Bemelegítés):** Folyamatos lüktetésű, nem túl agresszív, közepes tempójú zenék.
* **Isolation (Izoláció):** Erős ritmusszekcióval rendelkező, jól elkülöníthető hangszerekkel bíró zenék.
* **Technique / Footwork (Technika):** Tiszta ütemű, jól számolható, fokozatosan gyorsuló vagy stabil tempójú számok.
* **Choreography (Koreográfia):** Dramaturgiai ívvel rendelkező, hangsúlyos kiállásokkal (breaks) tarkított darabok.
* **Social / Practice (Gyakorlás):** Hosszabb, táncolható, "bulis" hangvételű zenék.
* **Cool-down (Levezetés):** Lassú, lágyabb hangzású levezetők.

### 2.2. Nemzetközi Szakmai Leírók (Descriptive Tags)
Javasolt TAG mező: `GENRE` és `MOOD`.

#### Salsa / Mambo fókusz:
* **Dura:** Erőteljes, rézfúvós hangsúlyos, "kemény" salsa.
* **Romantica:** Dallamos, ének-központú, lágyabb hangszerelés.
* **Guaguancó:** Rumbás elemek, hangsúlyos konga és clave.
* **Descarga:** Improvizatív, jam-szerű, gyakran gyors tempó.
* **Boogaloo:** Soul és Salsa keveréke, jellegzetes lüktetés.
* **Mambo / On2:** Specifikusan a New York-i stílushoz illeszkedő phrasing.

#### Bachata fókusz:
* **Dominican:** Tradicionális, gyors gitárjáték, hangsúlyos bongó.
* **Moderna:** Pop-osabb, tisztább stúdióhangzás.
* **Sensual:** Lassabb, érzelmesebb, szintetizátoros és basszus-hangsúlyos elemek.

### 2.3. Ütemtérkép és Zeneiség (Musicality)
Javasolt TAG mező: `COMMENT`.
* **Intro:** Hossz (pl. 4x8).
* **Breaks / Mambo section:** Specifikus váltások helye.
* **Energy Level:** 1-5 skálán (milyen intenzitású az adott szám).

---

## 3. A Zenei Könyvtár Hierarchiája

### Fájlnév szintaxisa (Letisztult):
`[Előadó] - [Cím].mp3` (Minden egyéb infó a metaadatokba kerül).

### Metaadat Leképezés (Hivatalos mezők):
| Információ | ID3v2 Mező | Példa |
| :--- | :--- | :--- |
| **Sebesség** | `BPM` | 185 (vagy 2,12 mp / 8 ütem) |
| **Stílus** | `GENRE` | Salsa Dura |
| **Tanári kategória** | `GROUPING` | Footwork / Isolation |
| **Hangulat / Energia** | `MOOD` | High Energy / Melodic |
| **Értékelés (Kezdő/Haladó)** | `RATING` | 5 csillag (vagy {36} a Commentben) |
| **Ütemtérkép / Info** | `COMMENT` | Intro: 2x8, Break at 1:20, Male Vocal |

---

## 4. Akcióterv a Modernizációhoz
1. **AIMP Sablon Készítése:** Olyan nézet és okos lejátszási listák létrehozása, amelyek a fenti `GROUPING` és `GENRE` alapján szűrnek (pl. egy kattintásra az összes "Bemelegítés" zene).
2. **Standardizált Szótár:** Egy fix lista készítése a leíró jelzőkből, hogy a tanárok ne használjanak szinonimákat (pl. ne legyen "lágy" és "soft" egyszerre, csak az egyik).
3. **Minta TAG-elés:** 10-10 referencia szám kézi felcímkézése az LPDPC-n az új rendszer szerint.
4. **GWS Automatizáció:** Beállítani, hogy a Master gépen végzett TAG módosítások azonnal frissüljenek a tanári laptopok AIMP adatbázisában.

---
*Utoljára frissítve: LPDPC Setup Thread - v2.0*

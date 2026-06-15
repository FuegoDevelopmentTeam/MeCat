# Ask_Agent LOG

## 2026. május 27.

*   **Kezdő kérdés:** Milyen TAG rendszert fejlesszünk ki?
*   **Döntés:** Táncóra stílusa, azon belül az óra típusa (páros, solo/footwork, warmup) a kulcsfontosságú.
*   **Hierarchia javaslat:** Fő tánc stílus (Main Dance Style) -> Óra fázisa / tevékenység (Class Part / Activity).
*   **Megerősítés:** A felhasználó megerősítette a hierarchia sorrendjét (A).
*   **Új követelmény:** Egy zeneszám több táncóra stílushoz és órarészhez is hozzárendelhető redundánsan.
*   **TAG elnevezések módosítása:**
    *   `GROUPING` -> `CLASS_STRUCTURE`
    *   `Main Style` -> `DANCE_STYLE`
*   **Módosított hierarchia:**
    1.  Tánc Stílus (DANCE_STYLE)
    2.  Óra Struktúra (CLASS_STRUCTURE)
    3.  Metrum & Ritmusalap (Rhythm Basis) - DANCE_STYLE alapján szűrve.
    4.  Dance Tempo (új elem)
    5.  Musicality (új elem)
    6.  Hangmérnöki / felépítési szempontok (új elem)
    7.  Mood
    8.  Genre

## 2026. május 28.

*   **Feladat:** `zenei_tag_szotar.md` átalakítása (szimbólumcsere, TAG magyarázat, formázás javítása).
*   **Végrehajtott módosítások:**
    *   `$\rightarrow$` → `→` csere.
    *   `TAG Kategória Előtagok Magyarázata` szekció beillesztve.
    *   Markdown formázási hibák javítása (pl. `**[I: Instrumental]`** → `**[I: Instrumental]**`).
    *   `{XY}` és `{+}` jelölések visszaállítva az eredeti formájukra a felhasználó kérésére.
*   **Kérdés:** Van esély AI alapú zene kategorizáló fejlesztésére, ami ritmus szerkezet térképet készít?
*   **Válasz:** Igen, abszolút van esély. Részletes kifejtés az AI képességekről (autó-tagelés, ritmikai analízis), kihívásokról (adatgyűjtés, szubjektivitás kezelése) és megoldási javaslatokról (hibrid megközelítés, feature extraction, gépi tanulás, mélytanulás, vizualizáció).

## 2026. május 29.

*   **Feladat:** AI alapú zene kategorizáló és ritmus szerkezet térképező rendszer fejlesztésének megkezdése.
*   **Döntés:** Nem váltunk Tervezési módba, hanem folytatjuk az aktuális módban.
*   **Projekt fázisok vázlata:**
    1.  Adatmodell Finomítása és "Golden Data" Előkészítése (Jelenlegi fókusz: `zenei_tag_szotar.md` és Jazztánc TAG-ek).
    2.  Félautomata Eszközök és Prototípusok.
    3.  AI Komponensek Fejlesztése.
    4.  Integráció és Felhasználói Felület.
*   **DJ program javaslat:** AIMP mint master zenei könyvtár rendszerező és AI-hub a VirtualDJ kiegészítéseként.
*   **Jazztánc TAG-ek beolvasztása:** A `zenei_tag_szotar.md` fájl bővítése az "golden data" fájlnevekből kinyert Jazztánc specifikus TAG-ekkel és pontosításokkal.
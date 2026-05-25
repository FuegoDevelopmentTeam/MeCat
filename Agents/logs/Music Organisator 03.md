# Fuego KMTSE - Zenei Rendszer és Lejátszási Architektúra (V3)

**Dátum:** 2026. május 10.
**Verzió:** 3.0 (Hierarchikus szűrés és sebesség-alapú automatizálás)
**Kapcsolódó dokumentum:** LPDPC Setup Thread - v1.0
**Projekt:** Intelligens zenei könyvtár, metaadat-alapú szűrés és stúdió minőségű lejátszás.

---

## 1. Rendszer Architektúra Összefoglaló
* **Master Gép:** LPDPC (Intel Core Ultra 9 285K, Audient iD14).
* **Szoftver:** AIMP (Zenei Könyvtár / Music Library modul használatával).
* **Adattárolás:** Google Workspace Drive (Master-Slave szinkronizáció).
* **Rendezési Elv:** Minden szakmai információ (stílus, órarész, hangulat, sebesség) ID3v2 tagekbe költözik a fájlnévből.

---

## 2. Hierarchikus és Kombinált Szűrési Logika
A rendszer alapja az AIMP *Music Library* adatbázisa, amely lehetővé teszi a többszempontú szűrést anélkül, hogy a fájlokat fizikailag mozgatni kellene.

### 2.1. A "Grouping Tree" (Csoportosítási fa) felépítése
A tanárok az alábbi logikai sorrendben böngészhetnek a bal oldali sávban:
1. **Fő stílus (Genre):** pl. Salsa, Bachata.
2. **Órarész / Funkció (Grouping):** pl. Bemelegítés, Izoláció, Koreográfia.
3. **Szakmai jelleg (Mood/Style):** pl. Romantica, Dura, Sensual.

### 2.2. Automatikus Sebesség (BPM) szerinti rendezés
* **Logika:** Minden szűrt nézethez és lejátszási listához hozzárendelünk egy "Sorting Template"-et.
* **Működés:** A szoftver a kiválasztott alkategórián belül **növekvő sorrendben** (Lassú -> Gyors) listázza ki a számokat.
* **Mértékegység:** A régi 8-ütem másodperc értéke (pl. 1,90) helyett a zenei BPM mezőt használjuk a matematikai sorbarendezéshez (BPM = 480 / [8-ütem mp]).

### 2.3. "Smart Playlists" (Okos listák)
Előre definiált szűrések a leggyakoribb tanári igényekre:
* **Példa:** "Összes Top Rated (5*) Bachata Sensual, sebesség szerint rendezve".
* **Frissülés:** Amint a Master gépen egy új fájl megkapja a megfelelő tageket a Drive-on, az automatikusan bekerül a tanári laptop okos listájába a megfelelő sebesség-pozícióba.

---

## 3. Véglegesített Metaadat Leképezés (Tag Szótár)

| Mező | Tartalom | Példa |
| :--- | :--- | :--- |
| **BPM** | Számolt sebesség (sorrendhez) | 120 (vagy 2,10) |
| **GENRE** | Fő stílus + Alkategória | Salsa Dura |
| **GROUPING** | Táncóra rész / Felhasználás | Warm-up / Isolation |
| **MOOD** | Zenei hangulat / Karakter | Melodic / Aggressive |
| **RATING** | Minőségi besorolás | 1-5 csillag |
| **COMMENT** | Ütemtérkép és DJ infók | Intro: 4x8, Break: 1:45, Male Vocal |

---

## 4. Frissített Akcióterv
1. **BPM Konverzió:** A meglévő "mp/8-ütem" értékek átszámítása BPM formátumra a pontos sorbarendezéshez.
2. **AIMP UI Sablon:** A "Grouping Tree" és az oszlopnézetek (BPM, Mood, Grouping) fixálása az LPDPC-n.
3. **Adatbázis Export:** A beállított nézetek és okos listák átmásolása a tanári laptopokra.
4. **Validálás:** Teszt szűrés futtatása: "Salsa -> Footwork -> 4 csillag felett -> Sebesség növekvő".

---
*Utoljára frissítve: LPDPC Setup Thread - v3.0*

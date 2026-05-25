# LPDPC - Zenei Rendszer és Lejátszási Architektúra (V5)

**Dátum:** 2026. május 10.
**Verzió:** 5.0 (Mérnöki szintű zenei analitika és kódolt metaadat rendszer)
**Projekt:** Fuego KMTSE - Professzionális táncstúdió zenei adatbázis és AIMP automatizáció.

---

## 1. Rendszer Alapvetés
Ez a dokumentum rögzíti a zenei könyvtár "mérnöki" szintű dekonstrukcióját, amely a GSheet alapú leíró nyelv (Hardness, Density, Intensity, Score {BA+}, MAP) digitális leképezését szolgálja az AIMP környezetben.

## 2. Definíciós Szótár & Paraméterek

### 2.1. Akusztikai és Energetikai Skálák (Mérnöki adatok)
Ezeket az adatokat kódolva tároljuk, hogy a tanár azonnal lássa a zene "fizikai" profilját.
- **Keménység (#1-6):** #1 (Soft/Lágy) -> #6 (Hard/Kemény/Harsh).
- **Sűrűség (÷1-6):** ÷1 (Thin/Vékony) -> ÷6 (Congested/Sűrű/Thick).
- **Energia (=1-6):** =1 (Apathetic/Lapos) -> =6 (Excited/Tüzes).
- **Ritmus Komplexitás (%1-6):** %1 (Marching/Monoton) -> %6 (Pulsing syncopated).

### 2.2. Pedagógiai és Strukturális Jelzők
- **Értékelés {BA+}:** Kettős "fül" rendszer. {B} Kezdők számára is élvezhető/értelmezhető, {A} Csak haladóbb fülnek való.
- **Komplexitás (o1-6):** Általános zenei bonyolultság.
- **Diverzitás (mu1-6):** Zenei részek változatossága (Parts diversity).
- **MAP (Ütemtérkép):** A zene szerkezeti leírása (pl. `[i4! v88 r4!4 4x4]`).

---

## 3. TAG Hierarchia és AIMP Konfiguráció

A bal oldali **Grouping Tree** felépítése a tanári munkafolyamat szerint:

1. **Metrum & Ritmusalap (Rhythm Basis)**
   - 4/4 (Standard Beats)
   - 3/4 - 6/8 (Triple / Compound)
   - Amorf (Ambient / No Beat)
2. **Fő stílus (Main Style)**
   - Salsa, Bachata, Jazz & Contemporary, Fitness.
3. **Óra fázisa (Class Part)**
   - Pre-class, Warm-up, Footwork, Choreo, Practice, Drill (Abs/Leg), Relax.

### Metaadat Leképezési Táblázat

| Funkció | ID3v2 Mező | Példa / Formátum |
| :--- | :--- | :--- |
| **Sebesség** | `BPM` | 145 (Rendezési alap) |
| **Stílus** | `GENRE` | 4/4 Salsa On2 |
| **Óra fázis** | `GROUPING` | Footwork / Warm-up |
| **Hangulat** | `MOOD` | Vivid / Fire / Romantic |
| **{BA+} Érték** | `CUSTOM1` | B+ (Kezdő barát) / A (Haladó) |
| **Mérnöki Kód** | **`COMMENT`** | `=5 #4 ÷4 %6 | mu4 o5 | MAP: [i4 v8]` |

---

## 4. Munkafolyamat (Workflow)

1. **Szűrés:** A tanár kiválasztja a fában a Metrumot, a Stílust és az Óra fázist.
2. **Rendezés:** Az AIMP automatikusan BPM szerint növekvő listát ad.
3. **Beolvasás:** A tanár a lista nézetben vagy a lejátszó "Comment" mezőjében látja a mérnöki kódot (pl. `=5 #3`), amiből tudja, hogy a zene sűrűsége vagy energiája passzol-e az adott pillanathoz.
4. **MAP követés:** Lejátszás közben a tanár látja a `COMMENT` végén a nyolcas térképet, így tudja, mikor jön kiállás vagy szóló rész.

## 5. Következő Lépések
- **RegEx Script:** A régi fájlnevekből a mérnöki kódok (pl. #, ÷ jelek) automatikus kinyerése a `COMMENT` mezőbe.
- **AIMP Skin/View:** Olyan nézet beállítása, ahol a `COMMENT` mező kiemelt helyen és jól olvashatóan szerepel.

---
*Utoljára frissítve: LPDPC Setup Thread - v5.0*

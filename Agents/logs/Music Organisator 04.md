# LPDPC - Zenei Rendszer és Lejátszási Architektúra (V4)

**Dátum:** 2026. május 10.
**Verzió:** 4.0 (Metrum-centrikus hierarchia és táncpedagógiai struktúra)
**Projekt:** Professzionális táncstúdió zenei adatbázis és AIMP automatizáció.

---

## 1. Hardver & Alap szoftverkörnyezet
* **Gép:** LPDPC (Ultra 9 285K / RTX 5080 / 64GB RAM).
* **Audio:** Audient iD14 (Master minőség).
* **Lejátszó:** AIMP (Zenei Könyvtár fókusszal).

## 2. Javasolt TAG Hierarchia (Grouping Tree)
A tánctanárok és DJ-k számára az alábbi fa-szerkezetet konfiguráljuk az AIMP-ben:

1. **Metrum & Ritmusalap (Rhythm Basis)**
   - 4/4 (Standard Beats)
   - 3/4 - 6/8 (Triple / Compound)
   - Amorf (Ambient / No Beat)
2. **Fő stílus (Main Style)**
   - Salsa, Bachata, Jazz & Contemporary, stb.
3. **Óra fázisa (Class Part)**
   - Pre-class, Warm-up, Footwork, Choreo, Practice, Drill (Abs/Leg), Relax.
4. **Zenei alstílus (Sub-style / Mood)**
   - Pl. Mambo, Pachanga, Dominican, Sensual, On2.

## 3. Dinamikus Lista Kezelés
* **Rendezés:** Minden szűrt lista automatikusan BPM szerint növekvő sorrendbe rendeződik.
* **Kiválasztás:** A tanár kiválasztja a stílust és fázist -> rákattint a kezdő sebességre -> a lejátszó lineárisan gyorsuló sorrendben adja a számokat.
* **Minőség & Újdonság:** A Rating (csillagok) és a Selection Index (Válogatás száma) oszlopként jelenik meg a gyors vizuális szűréshez.

## 4. Metaadat Leképezés
* **Metrum + Stílus:** `GENRE` (Pl. "4/4 Salsa")
* **Óra fázisa:** `GROUPING`
* **Alstílus:** `MOOD`
* **Sebesség:** `BPM` (Számolt érték: 480 / [8-ütem mp])
* **Válogatás index:** `PUBLISHER` vagy `CUSTOM1` mező

---
*Utoljára frissítve: LPDPC Setup Thread - v4.0*
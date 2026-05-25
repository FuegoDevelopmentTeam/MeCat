# LPDPC Master Rendszer és Fuego KMTSE Zenei Architektúra - Összevont Kontextus Napló

**Dátum:** 2026. május 10.
**Verzió:** 6.0 (Összevont Master Dokumentum)
**Tulajdonos / Szervezet:** FUEGO KULTURALIS MUEVESZETI TANC ES SPORT EGYESUELET (Nonprofit Google Workspace)
**Cél:** Dedikált videóvágó, programozó, AI hanggeneráló munkaállomás (LPDPC) felépítése, valamint a Fuego KMTSE táncstúdió zenei könyvtárának modernizálása, metaadat-alapú rendszerezése és GWS szinkronizációja a tánctanárok felé.

---

## 1. Hardver és Szoftver Alapok (LPDPC Master Gép)

### 1.1. Hardver Specifikációk (BTO PC)
* **Alaplap:** ASUS ROG STRIX B860-I GAMING WIFI
* **Processzor (CPU):** Intel Core Ultra 9 285K 3700 1851 BOX
* **Processzor hűtés:** ASUS ROG STRIX LC II 280 ARGB
* **Videókártya (GPU):** ASUS 16GB D7 RTX 5080 OC PROART
* **Memória (RAM):** 64GB (2x32) DDR5 6600-32 Corsair Vengeance
* **Tárhely:** 2 db 4TB Samsung 990 PRO M.2 SSD (Összesen 8TB)
* **Tápegység:** CORSAIR SF1000
* **Számítógépház:** Sharkoon REBEL C20 ITX

### 1.2. Perifériák és Megjelenítők
* **Elsődleges kijelző:** Samsung QE65S95F (65" OLED TV)
* **Hangkártya / Interfész:** Audient iD14 (stúdió minőségű D/A konverzió)
* **Beviteli eszközök:** Logitech MX Keys S billentyűzet, Logitech MX Ergo egér

### 1.3. Szoftveres Környezet (Alapok)
* **Operációs Rendszer:** Windows 11
* **Alap Szoftverek:** Norton 360, Veeam Agent, NVIDIA Driver (596.36), Intel RST, Audient USB Audio Driver (v5.5.1).
* **Munka / Kreatív:** DaVinci Resolve 21.0, VirtualDJ 2026, Sidify, AIMP (Elsődleges zenelejátszó), Spotify, Adobe CC.
* **Fejlesztés:** MS Windows Desktop Runtime (8.x, 9.x), OpenSSL v3.0.0, Power Automate, Terminal.

---

## 2. Fuego KMTSE Zenei Rendszer - Projekt Áttekintés

A projekt célja a korábbi, Virtual DJ és fájlnév-alapú zenei rendszerezés leváltása egy AIMP-alapú, stúdió minőségű, metaadatokon (ID3v2 TAG-eken) nyugvó intelligens adatbázisra, amelyet Google Workspace (GWS) Drive-on keresztül szinkronizálunk a tánctanárok laptopjaira.

### 2.1. A Rendszerezés Evolúciója
**Régi "Old School" Rendszer:**
Minden adat a fájlnévbe volt kódolva: `[Sebesség/8-ütem mp-ben]-[Válogatás index] {[Kezdő][Haladó értékelés]} [STÍLUS] . ([Tag-ek]) [Előadó] - [Cím].mp3`
*(Példák: `1,90-14 {o6} MEJOR . [mu 4 melody on2 descarga show] Joe Cuba - Quinto Sabroso.mp3` vagy `3,63-22 {o3} BACHATA . (LQ flat marching) Luciano Pereyra - Si No Es Muy Tarde.mp3`)*

**Új Koncepció:**
A fájlnevek letisztulnak (`Előadó - Cím.mp3`), a komplex szakmai, táncpedagógiai és mérnöki információk pedig bekerülnek a kereshető meta TAG-ekbe.

---

## 3. Táncszakmai és Mérnöki Tagging Rendszer

A zenei anyag mérnöki és táncpedagógiai dekonstrukción esik át. Ezt az AIMP Music Library funkciójával, egy hierarchikus "Grouping Tree" és okos lejátszási listák (Smart Playlists) segítségével tesszük kereshetővé.

### 3.1. AIMP Grouping Tree (Hierarchikus Szűrés a Tanároknak)
1. **Metrum & Ritmusalap (Rhythm Basis):** 4/4 (Standard), 3/4 - 6/8 (Triple/Compound), Amorf (No Beat/Ambient).
2. **Fő stílus (Main Style):** Salsa, Bachata, Jazz & Contemporary, Fitness, stb.
3. **Óra fázisa (Class Part):** Pre-class, Warm-up, Footwork, Choreo, Practice, Drill, Relax.
4. **Zenei alstílus / Hangulat (Sub-style / Mood):** Pl. Dura, Romantica, Sensual, Mambo, Guaguancó, Dominican.

### 3.2. Akusztikai és Energetikai Skálák (Mérnöki adatok)
- **Keménység (#1-6):** #1 (Soft) -> #6 (Hard/Harsh)
- **Sűrűség (÷1-6):** ÷1 (Thin) -> ÷6 (Congested/Thick)
- **Energia (=1-6):** =1 (Apathetic) -> =6 (Excited/Fire)
- **Ritmus Komplexitás (%1-6):** %1 (Marching/Monoton) -> %6 (Pulsing syncopated)

### 3.3. Pedagógiai és Strukturális Jelzők
- **Értékelés {BA+}:** {B} Kezdők számára befogadható, {A} Csak haladóbb fülnek.
- **Komplexitás (o1-6) / Diverzitás (mu1-6):** Bonyolultság és téma-változatosság.
- **MAP (Ütemtérkép):** A zene szerkezeti leírása (pl. `[i4! v88 r4!4 4x4]`).

---

## 4. Véglegesített Metaadat Leképezés (Tag Szótár)

Az új rendszerben a fenti kódrendszer az alábbi ID3v2 mezőkbe kerül automatizálásra:

| Funkció | ID3v2 Mező | Példa / Formátum | Leírás |
| :--- | :--- | :--- | :--- |
| **Sebesség** | `BPM` | `145` | A régi mp/8-ütem érték átszámolva. **Fontos:** Az AIMP alapértelmezetten ez alapján rendez növekvő sorrendbe. |
| **Stílus** | `GENRE` | `4/4 Salsa On2` | Metrum és Fő stílus kombinációja. |
| **Óra fázis** | `GROUPING` | `Footwork / Warm-up` | A táncóra adott szakaszának jelölése. |
| **Hangulat** | `MOOD` | `Vivid / Fire / Romantic` | Nemzetközi leírók és hangulati tag-ek. |
| **{BA+} Érték** | `CUSTOM1` | `B+` vagy `A` | A zene "fülelhetőségi" szintje (Kezdő/Haladó). |
| **Mérnöki Kód** | `COMMENT` | `=5 #4 ÷4 %6 \| mu4 o5 \| MAP: [i4 v8]`| Dedikált, kódolt string a tanár számára a zene anatómiájáról és a MAP-ról. |

---

## 5. Munkafolyamat és Akcióterv

### Tanári Lejátszási Logika (Workflow)
1. A tanár a bal oldali sávban (Grouping Tree) kiválasztja az útvonalat (pl. `4/4` -> `Salsa` -> `Footwork`).
2. Az AIMP kilistázza a megfelelő zenéket, **automatikusan BPM szerint növekvő** sorrendben.
3. A `COMMENT` mező vizuális kijelzésével a tanár lejátszás közben azonnal látja a zene mérnöki energiaszintjeit és az ütemtérképet (MAP).

### Következő Lépések a LPDPC Gépen
1. **RegEx Script:** Egy Reguláris Kifejezés (RegEx) motor vagy mp3tag akciócsoport megírása, amely a meglévő (salsa.txt, bachata.txt stb.) régi fájlneveket feltöri, és az információkat a fenti táblázatnak megfelelően a TAG-ekbe írja.
2. **AIMP Sablon (Skin/View):** Olyan egyedi felület és nézet kialakítása az LPDPC-n, amely a `GROUPING`, `BPM`, `MOOD` és legfőképp a `COMMENT` mezőt tökéletesen láthatóvá teszi.
3. **GWS Szinkronizáció:** Az AIMP Music Library adatbázisfájljának és a zenéknek a felhős szinkronizációs tesztje.
4. **Fejlesztői Környezet / AI Kiegészítés:** Az eredeti céloknak megfelelően (ha a zenei rendszer stabil) az AI keretrendszerek és az IDE-k konfigurálása az LPDPC-n.

---
*LPDPC Master Context - Utoljára frissítve: 2026.05.10. v6.0*

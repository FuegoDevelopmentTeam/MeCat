# Fuego KMTSE - Zenei Rendszer és Lejátszási Architektúra
**Dátum:** 2026. május 10.
**Kapcsolódó fő dokumentum:** LPDPC Setup Thread - v1.0
**Projekt:** Zenei könyvtár modernizálása, metaadat-alapú rendszerezés és stúdió minőségű lejátszás szinkronizálása a tánctanárok eszközeire.

---

## 1. Hardver és Szoftver Alapok (LPDPC Master Gép)
* **Környezet:** Intel Core Ultra 9 285K, 64GB RAM, 8TB SSD NVMe, Asus ROG STRIX B860-I.
* **Audio Interfész:** Audient iD14 (stúdió minőségű D/A konverzió).
* **Elsődleges Lejátszó (Cél):** AIMP (Bizonyítottan jobb hangminőség a master gépen, mint a Virtual DJ DSP nélküli állapota).
* **Szinkronizáció:** Google Workspace (GWS) Drive a master gép és a tánctanári laptopok között.

## 2. A Zenei Rendszerezés Evolúciója

### Jelenlegi "Old School" (Fájlnév alapú) Rendszer:
Minden információ a fájlnévbe van kódolva az alábbi szintaktika szerint:
`[Sebesség/8-ütem mp-ben]-[Válogatás index] {[Kezdő értékelés][Haladó értékelés]} [STÍLUS] . ([Karakterisztika/Tag-ek]) [Előadó] - [Cím].mp3`
*Példák a mellékelt txt-kből:* `1,90-14 {o6} MEJOR . [mu 4 melody on2 descarga show] Joe Cuba - Quinto Sabroso.mp3`
`3,63-22 {o3} BACHATA . (LQ flat marching) Luciano Pereyra - Si No Es Muy Tarde.mp3`

### Új Koncepció (Meta TAG alapú):
A cél egy letisztult fájlnév, miközben a komplex adatokat (a megálmodott bővített leképezés alapján) ID3/Meta TAG-ekbe költöztetjük.
* **Fájlnév:** Csak a legszükségesebb azonosítási információk (pl. Előadó - Cím).
* **Meta TAG-ek:** Sebesség (BPM / 8-ütem időtartam), Értékelések (Kezdő/Haladó pontszámok), Stílus alkategóriák (pl. Bachaton, Dura, Mambo), Hangulati elemek (pl. sensual, marching), **Ütemtérkép** (pl. szerkezet: intro, X x 8 ütem, kiállások, 4 ütemes törések).

## 3. Megoldandó Feladatok és Szoftveres Kihívások

1. **AIMP Okos Szűrés (Smart Playlists / Filter Folders):**
   A VDJ "Filter Folder" funkcióját kell reprodukálnunk AIMP-ban. Az AIMP Audio Library (Zenei Könyvtár) és a "Smart Playlist" funkciója képes Meta TAG-ek alapján dinamikusan szűrni. Ezt kell a tánctanárok számára bolondbiztosra és pörgős órai használatra konfigurálni.
2. **Tömeges TAG-elés és Fájlnév-konverzió (Tag Editor):**
   Szükségünk lesz egy robusztus szoftverre (pl. Mp3tag), amely képes a meglévő "kódolt" fájlnevekből Reguláris Kifejezések (RegEx) segítségével kinyerni az információkat, és automatikusan beírni azokat a megfelelő ID3v2 tagekbe (pl. `BPM`, `RATING`, `COMMENT`, `GROUPING`).
3. **Ütemtérkép Szabványosítása:**
   A táblázatban megálmodott kibővített paramétereknek (pl. szerkezeti térkép, táncos specifikus jelek) fix helyet kell találni a Meta TAG szabványban (javasolt a `COMMENT` vagy a `UNSYNCEDLYRICS` mező), hogy a lejátszó felületén a tanár azonnal lássa lejátszás közben.
4. **GWS Szinkronizációs Modell:**
   Kialakítani egy olyan környezetet, amely a GWS-en keresztül transzparensen szinkronizálja az AIMP adatbázis fájljait és az át-tittelt zenéket a laptopokra, megőrizve a lejátszási listákat.

---
## 4. Következő Lépések (Akcióterv)
1. **Adatmodell véglegesítése:** Melyik régi fájlnév-adatpont melyik hivatalos ID3v2 mezőbe kerüljön?
2. **RegEx Script készítése:** A fájlnevek automatikus szétbontásához és TAG-be írásához.
3. **AIMP tesztkörnyezet:** Kialakítani egy AIMP Portable verziót okos lejátszási listákkal, amivel a tanárok kipróbálhatják a VDJ-szerű szűrési élményt.

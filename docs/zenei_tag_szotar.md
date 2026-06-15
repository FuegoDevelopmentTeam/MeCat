# Táncspecifikus Zenei TAG Rendszer és Mester-Szótár

Ez a dokumentum a salsa és bachata táncórák zenei válogatásához használt korábbi (nyers, fájlnevekbe rejtett) jelöléseket és a tervezett, modern, AI-kompatibilis hierarchikus TAG-rendszert foglalja össze betűrendi struktúrában. 

---

## 1. Zenei Értékelő Rendszer (Rating Tags)

A fájlokban korábban használt `{xx}` (például `{o5}`, `{45}`) kódok az alábbiak szerint épülnek fel és alakulnak át az új `[R: X-Y]` formátummá:

- **Első szám (X):** Mennyire jó/élvezhető a zene a laikus, populáris közönség számára (0-6 skálán).
- **Második szám (Y):** Mennyire jó a zene a szakemberek, sok kilométeres haladó táncosok körében (0-6 skálán).

### Értékelési szótár:

- `{0X}` **→** `[R: 0-X]`: Csak haladóknak/szakmának ajánlott zene, a laikus közönségnek nem tetszik (pl. túl bonyolult latin jazz vagy elvont pop-fusion).
- `{X0}` **→** `[R: X-0]`: Nagyon népszerű, kommersz zene (pl. rádiós pop sláger), de táncszakmailag lapos, egysíkú.
- `**{XY}` → `[R: X-Y]`**: Mindkét célcsoport számára értékes zene (pl. `{45}` → `[R: 4-5]`: a közönség is szereti, és zeneileg is gazdag).
- `**{+}` → `[R: X-Y-STAR]**`: Kiugróan jó módosító jelölés. Kiemelt kedvenc, "banger", az óra vagy a buli tervezett fénypontja.
- `0x` **/** `x` **→** `[R: UNRATED]`: Meghatározhatatlan, rendhagyó vagy még be nem sorolt rétegzene.

---

## 2. Átfogó TAG Szótár (Betűrendben)

### TAG Kategória Előtagok Magyarázata
A TAG-ek előtagjai a mögöttes kategóriát jelölik, ezzel segítve a gyors azonosítást és szűrést:

-   **[R: ...] (Rating)**: Zenei értékelési metódus.
-   **[G: ...] (Genre)**: Zenei műfaj, stílus, alstílus.
-   **[M: ...] (Mood)**: Zenei hangulat, karakter, érzelmi töltés.
-   **[S: ...] (System/Quality)**: Rendszertechnikai vagy minőségi paraméter.
-   **[U: ...] (Utility/Usage)**: Használati mód, funkció, pedagógiai cél.
-   **[T: ...] (Tempo)**: Tempóra vonatkozó jelölés (gyorsítás/lassítás).
-   **[I: ...] (Instrumentation/Vocal)**: Hangszerelésre vagy énekhangra vonatkozó információ.

---


### A, Á

- **Acoustic / Unplugged** → `**[I: Instrumental]**
  - *Leírás:* Elektronikus hangszerektől mentes, tiszta, gyakran csak akusztikus gitárra és énekre épülő letisztult hangzásvilág.
- **Afro / Afro-Jazz / Guaguancó** → `**[G: Cuban-Timba]** vagy `**[G: Salsa-Dura]**`
  - *Leírás:* Erős afrokubai ritmikai elemekkel, rumba alapokkal vagy hagyományos afrikai kórussal rendelkező törzsi lüktetés.
- **Auth / Autentikus / Traditional** → `**[G: Bachata-Autentica]** vagy `**[G: Salsa-Dura]**`
  - *Leírás:* Hagyományos, a stílus kulturális gyökereihez hű hangzás (pl. dominikai bachata vagy klasszikus mambo).

### B

- **Balada / Romcsi / Nyálas / Szupernyál / Hypernyál / Passionate / Warm** → `**[M: Romantic]**
  - *Leírás:* Lassabb tempójú, rendkívül érzelmes, szerelmes, olykor giccses hangvételű zene. Ideális a lágyabb, páros mozdulatok gyakorlásához.
- **Boogaloo / Latin Funk / Soul** → `**[G: Boogaloo]**
  - *Leírás:* A 60-as évekbeli amerikai R&B/Soul és a latin zene fúziója. Gyakran angol nyelvű, kifejezetten "tapsolós", húzós beat jellemzi.
- **Breezy / Light / Soft** → `**[M: Chill]**
  - *Leírás:* Könnyed, lágy, szellős lüktetés. Nem tolakodó, tökéletes háttérzene vagy technikai magyarázatok alá játszható alap.

### C, Cs

- **Chacha / Chacha-Salsa / Chacha-Mambo** → `**[G: Chacha]**
  - *Leírás:* Kifejezetten Cha-cha-cha alapritmusra és lüktetésre épülő zene, többnyire latin-jazz vagy tradicionális mambo alapokon.
- **Chill / Calm / Serene / Snow** → `**[M: Chill]**
  - *Leírás:* Nyugodt, lebegős hangulatú, relaxáló, alacsonyabb intenzitású zene.
- **Classic / Old rec / Muffled / Dobozhang** → `**[S: Bad-Quality]**
  - *Leírás:* Régi, archív felvétel, amelyen érződik a korabeli analóg technika, vagy gyengébb minőségű a digitalizálása ("dobozhangzás"). Órán csak indokolt esetben használandó.
- **Cool / Swag / Street / Modern / Urban** → `**[G: Bachata-Urbana]** vagy `**[G: Salsa-Romantica]**`
  - *Leírás:* Hip-hopos, modern, utcai, lazább lüktetés. Gyakran RnB és Reggaeton elemekkel keverve.
- **Cuban / Timba** → `**[G: Cuban-Timba]**
  - *Leírás:* Kubai stílusú salsa vagy timba. Törtebb, agresszívebb ritmusképlet jellemzi domináns dobokkal és rezesekkel. Ruedához kiváló.

### D

- **Dark / Suicide / Mystic / Strange / Crazy** → `**[M: Dark]**
  - *Leírás:* Különleges, sötétebb, elvontabb hangulat. A laikus füleknek zeneileg olykor nehezen lekövethető vagy szokatlan szerkezetű (pl. Billie Eilish vagy dubstep-bachata fusion).
- **Descarga / Drum / Percussion** → `**[U: Descarga]**
  - *Leírás:* Hangszeres örömzenélés, többnyire kiemelt konga, timbal vagy bongo szólókkal. **Shine (szóló lábmunka) gyakorláshoz kötelező.**
- **DJ / Levimix (Lm) / Rmx / Blend / Intro** → `**[U: DJ-Tools]**
  - *Leírás:* Lemezlovasok által vágott, módosított, gyorsított/lassított verziók, speciális órai intrók vagy pop-latin mashupok.
- **Dura / Mejor / On2 / Mambo / Salsa Brava** → `**[G: Salsa-Dura]** vagy `**[G: Mambo-On2]**`
  - *Leírás:* Klasszikus, kőkemény salsa brava vagy mambo struktúra, éles fúvósszekcióval és tisztán hallható clave ritmussal.

### E, É

- **Emu / Mu (pl. mu_0, mu_4, mu_5, stops, cuts)** → `**[U: Breaks-Heavy]**
  - *Leírás:* Komoly zenei kiállásokra és ritmikai törésekre (breaks) utaló technikai jelölés. A szám a törés váratlanságát és erejét jelölte. Kiváló zeneiség (musicality) fejlesztéséhez.
- **Energetic / Fire / Vivid / Marching / Menetelős** → `**[M: Energetic]**
  - *Leírás:* Magas energiájú, pörgős, határozott és magával ragadó lüktetés. Kiválóan alkalmas pörgős tréningekhez és haladóbb kombinációkhoz.

### F

- **Flat / Monoton / Uniform / Practicing / Lapos** → `**[U: Flat-Practice]**
  - *Leírás:* Teljesen egyenletes ritmusvezetés, nincsenek benne váratlan kiállások vagy törések. **Kezdőknek ritmustartáshoz és alaplépések rögzítéséhez ideális.**
- **Flute / Violin / Fuvola / Hegedű** → `**[G: Pachanga]**
  - *Leírás:* Olyan dalok, amelyekben a fuvola vagy a hegedű kap főszerepet (jellemzően a Charanga és Pachanga stílusok sajátossága).

### G, Gy

- **Gy / Gyors / Quick / Fast** → `**[T: Fast]**
  - *Leírás:* Az átlagosnál lényegesen pörgősebb tempójú felvétel, vagy mesterségesen felgyorsított zenei verzió.

### I, Í

- **Instrumental / Instr** → `**[I: Instrumental]**
  - *Leírás:* Ének nélküli, tisztán hangszeres kompozíció. Segít a táncosnak a hangszerekre (pl. zongoramontuno, basszus) koncentrálni.

### J

- **Jazz / Latin-Jazz / Jazz-Funk** → `**[G: Latin-Jazz]**
  - *Leírás:* Dzsesszes harmóniamenetekre és komplex hangszeres improvizációkra épülő zene. Magas szakmai értékű, de a laikus fülnek nehezen emészthető tánczene.

### L

- **LQ (Low Quality) / Off-key** → `**[S: Bad-Quality]**
  - *Leírás:* Rossz minőségben digitalizált, torz vagy zeneileg hamis, fület bántó hangszerelésű/énekhangú felvétel.

### M

- **Musical / Revue / Show / Drama / Broadway / Teátrális** → `**[U: Show-Revue]**
  - *Leírás:* Színpadias, teátrális, erősen dramatizált felvételek grandiózus kiállásokkal. Kifejezetten színpadi fellépésekhez és koreográfiákhoz ajánlott.

### N

- **Non-Latin / Rettenet** → `**[S: Non-Latin]**
  - *Leírás:* Alapvetően nem latin műfajú zenék (pl. pop, RnB, funk), amelyeknek a lüktetése vagy üteme ritmikailag mégis alkalmas a salsa/bachata lépéseire.

### P

- **Pachanga** → `**[G: Pachanga]**
  - *Leírás:* Pattogós, rendkívül játékos és pörgős ritmus, jellemzően fuvola és hegedű kísérettel, jellegzetes pachanga-lépésekhez.
- **Pop / Commercial / Melody** → `**[G: Bachata-Pop]** vagy `**[G: Salsa-Romantica]**`
  - *Leírás:* Rádióbarát, fülbemászó dallamvilágú, széles közönség számára azonnal megszerethető kommersz hangzás.

### R

- **Reggaeton / Salsaton / Bachaton / Dembow** → `**[G: Bachata-Urbana]**
  - *Leírás:* Urbánus, lüktető Reggaeton (dembow) alapokat vagy rap-betéteket tartalmazó hibrid tánczene.

### S, Sz

- **Slow / Vontatott** → `**[T: Slow]**
  - *Leírás:* Az átlagosnál jóval lassabb tempó, ami kiváló a bonyolultabb páros figurák lassított lebontásához vagy teljesen kezdő órákra.

### V

- **Vocal+ / Nice vocal / Female vocal / Male eng vocal** → `**[I: Vocal-Focus]**
  - *Leírás:* Domináns, kiemelkedően tiszta és dallamos énekhang. Olyan zeneiségi feladatokhoz ideális, ahol a táncosoknak az énekszólamot kell lekövetniük.

### W

- **Warm / Warmup / Levezetés** → `**[U: Warmup]**
  - *Leírás:* Kellemes, lágyabb energiájú zene, amely tökéletes az óra eleji bemelegítéshez (warmup) vagy az óra végi nyújtáshoz és levezetéshez.


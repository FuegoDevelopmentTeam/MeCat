# MeCat_Music_Tags.md — Hierarchikus Zenei Tag-Taxonomia (AI-Ready)

Version: 1.0.0  
Date: 2026-06-15  
Status: **BRAINSTORMING** — a `docs/kiindulasi_docs/music_tags.md` optimalizált, kiterjesztett utódja.  
DANA SSoT: D083, D087, D093; ontológia-forrás: KineLex `term_id` (D084).  
Kapcsolódó: `MASTER_CONCEPT_MeCat.md`, `zenei_tag_szotar.md` (legacy szótár).

> **Filozófia:** A régi `music_tags.md` oszlopai **szűk, egymáshoz közeli** fogalomtér-pontok voltak (pl. „relax" és „calm" szomszédok). Ez a dokumentum **távolságtartó, egyenlő jelentéstartalom-távolságú** csomópontokat használ — mint a színskála sárga → narancs → zöld → zöldeskék → királykék helyett az okker–alma-zöld szűk sáv helyett.  
> **Célcsoportok:** (1) **Tánctanár** — óraterv, pedagógiai cél, musicality-oktatás; (2) **Hangulat-menedzser / DJ support** — buli-ív, átmenetek, energia-governance, CUE-k.

---

## 0. Használati szabályok

| Szabály | Leírás |
| :--- | :--- |
| **Kulcs formátum** | `domain.subdomain.value` (pl. `affect.valence.positive.calm`) |
| **Skálák** | Ahol `scale:0-6` — 0 = dimenzió minimuma, 6 = maximuma |
| **Objektív vs. overlay** | **Minősítő Alkotmány** = objektív tagek; **Ízlés Alkotmány** = `taste.*` overlay (P49) |
| **KineLex kötés** | `genre`, `subgenre`, `dance_style`, `usage.pedagogy`, `cue.linked_term` → kötelező `term_id` (D084) |
| **Legacy map** | `[legacy: …]` = régi `music_tags.md` / fájlnév-jelölés |

**Szeparátorok (legacy kompatibilitás, write-back):** `,` `-` `{ }` `.` `!` `@mm:ss` `°` `§` `%` `#` `÷` — a MeCat belső SSoT a strukturált tag; a fájlnév/meta **projekció**.

---

## I. AZONOSÍTÁS & KATALÓGUS (`identity.*`)

*A „ki-mi-mikor" dimenzió — független a hangzástól.*

| Tag kulcs | Típus | Leírás | Legacy |
| :--- | :--- | :--- | :--- |
| `identity.artist` | string | Előadó | ARTIST |
| `identity.title` | string | Cím | TITLE |
| `identity.remixer` | string | Feldolgozó DJ / mix | `(DJ. Xy)` |
| `identity.filename_canonical` | string | **`Előadó - Cím (DJ. Xy).ext`** — ez a letisztult fájlnév | — |
| `identity.selection_index` | int / date | Válogatás-index: mennyire új a gyűjteményben | `-XX` |
| `identity.collection_era` | enum | Mikori válogatás korszaka (index vagy dátum meta) | — |
| `identity.fingerprint.acoustic` | string | Tartalom-alapú ujjlenyomat (dedup) | — |
| `identity.version.parent_id` | uuid | Verzió-fa: eredeti ↔ remix ↔ speed-edit | — |

---

## II. TEMPÓ & MOZGÁS (`tempo.*`)

*Sebesség és tempó-módosítók — külön domain az identitástól és a ritmustól.*

| Tag kulcs | Típus | Leírás | Legacy |
| :--- | :--- | :--- | :--- |
| `tempo.bpm` | number | Beats per minute | `X,xx` első rész |
| `tempo.bar_duration_sec` | number | Egy 8-ütem hossza mp-ben (tánc-számolás) | `1,90` = 1,90 mp/8 |
| `tempo.modifier.altering` | flag | Változó tempó a szám során | `al` |
| `tempo.modifier.speedup` | flag | Gyorsított verzió | `su` / Gy |
| `tempo.modifier.speeddown` | flag | Lassított verzió | `sd` / Slow |
| `tempo.dance_class` | enum | `very_slow` → `very_fast` (7 fokozat, egyenlő távolság) | — |
| `tempo.stability` | enum | `rock_steady` / `slight_drift` / `variable` / `rubato` | — |

---

## III. MŰFAJ & TÁNC-TAXONÓMIA (`taxonomy.*`) — KineLex `term_id`

*A táncpedagógiai besorolás — minden érték KineLex-ből.*

| Tag kulcs | Típus | Leírás | Legacy |
| :--- | :--- | :--- | :--- |
| `taxonomy.genre` | term_id | Fő tánc/zenei műfaj (SALSA, BACHATA, CHACHA…) | genre oszlop |
| `taxonomy.subgenre` | term_id | Alműfaj, stílus, kulturális eredet | `.LA` `.PR` `.NY` on1/on2 |
| `taxonomy.dance_style_timing` | term_id | Időzítés (on1, on2, on1on2…) | on1/on2 |
| `taxonomy.genre_group` | term_id | latin_dance / ballroom / street / non_latin | NONLATIN |
| `taxonomy.origin` | term_id | cuban / new_york / puerto_rican / dominican… | origin |
| `taxonomy.authenticity` | enum | `authentic` / `commercial` / `fusion` / `crossover` | auth/comm |

**Példa műfaj-értékek (referencia, nem duplikált ontológia):** RUMBA, CHACHA, BOOGALOO, SALSA, SAMBA, MERENGUE, CUMBIA, BACHATA, REGGAETON, NONLATIN, JAZZ, TANGO, IDO…

---

## IV. KÖZÖNSÉG & ÍZLÉS (`taste.*`) — Ízlés Alkotmány overlay (P49)

*Szubjektív réteg — tanáronként / diákközösségenként eltérő.*

| Tag kulcs | Típus | Skála | Leírás | Legacy |
| :--- | :--- | :--- | :--- | :--- |
| `taste.rating.beginner` | int | 0–6 | Kezdő/laikus tetszés | `{X0}` / `{XY}` első |
| `taste.rating.advanced` | int | 0–6 | Haladó/szakmai érték | `{0Y}` / `{XY}` második |
| `taste.rating.student` | int | 0–6 | Tanítványi közösségi index | — |
| `taste.banger` | flag | — | Kiemelt fénypont (+) | `{+}` / STAR |
| `taste.listening_difficulty` | enum | — | `beginner_ears` (B) / `advanced_ears` (A) | B/A |
| `taste.preference` | enum | — | `favorite` / `avoid` / `neutral` | — |

---

## V. VOKÁL & NYELV (`vocal.*`)

*Ének-performans — külön domain a hangulattól.*

| Tag kulcs | Típus | Leírás | Legacy |
| :--- | :--- | :--- | :--- |
| `vocal.presence` | enum | `none` / `sparse` / `balanced` / `dominant` / `a_capella` | 0\|instr / instr |
| `vocal.gender` | enum | `female` / `male` / `duet` / `chorus` / `mixed` | fmc / 1fmc–6fmc |
| `vocal.melody_quality` | scale | 0–6: `bad`→`excellent` | melody skála |
| `vocal.language` | enum | `en` / `sp` / `hu` / `pt` / `fr` / `instrumental` / `mixed` | 'en 'sp 'hu |
| `vocal.focus` | flag | Énekszólam követésére ideális (musicality óra) | Vocal+ |

---

## VI. AFFektív & ENERGETIKAI TÁJ (`affect.*`)

*Kiterjesztett érzelmi tér — **pozitív** és **negatív** tengely külön, plusz **aktiváció** és **textúra**.*

### VI.A. Aktiváció / intenzitás (`affect.arousal.*`) — skála 0–6

| Érték | Jelentés | Legacy energy |
| :--- | :--- | :--- |
| `dormant` | Szinte statikus, meditatív | very low |
| `whisper` | Csendes, alig hallható energia | silent |
| `gentle` | Pihentető, laza | relax |
| `moderate` | Átlagos órai energia | average / 3 |
| `driving` | Határozott, előremozdító | energetic |
| `surge` | Erőteljes, pörgős | vivid / excited |
| `blaze` | Maximális tűz | fire / high |

### VI.B. Pozitív affekt (`affect.valence.positive.*`) — skála 0–6

| Érték | Jelentés | Legacy |
| :--- | :--- | :--- |
| `serene` | Derűs, békés | serene / peace |
| `warm` | Meleg, barátságos | warm |
| `playful` | Játékos, könnyed | playful / ease |
| `uplifting` | Felemelő | uplift / cheerful |
| `celebratory` | Ünnepi | celebration |
| `passionate` | Lelkes, érzelmes | passion |
| `euphoric` | Örömáradás | euphory / joyful |

### VI.C. Negatív / kihívás affekt (`affect.valence.negative.*`) — skála 0–6 (opcionális jelölés)

| Érték | Jelentés | Legacy |
| :--- | :--- | :--- |
| `melancholic` | Melankolikus, bánatos | sorrow / melancholic |
| `tense` | Feszült | tense / thrill |
| `restless` | Nyugtalan | restless |
| `discomfort` | Kényelmetlen a táncosnak | discomfort |
| `flat` | Lapos, unalmas | boring / flat |
| `chaotic` | Zűrzavaros | chaos |

### VI.D. Textúra (`affect.texture.*`)

| Tag kulcs | Skála 0–6 | Legacy |
| :--- | :--- | :--- |
| `affect.texture.hardness` | soft → extreme | #1–#6 |
| `affect.texture.density` | sparse/a_capella → congested | ÷1–÷6 |
| `affect.texture.brightness` | dark → bright | dark/bright |
| `affect.texture.smoothness` | rough → silky | smooth |

> **Miért jobb?** A régi rendszerben „relax", „calm", „peaceful" **egymás mellett** volt; itt **aktiváció** (VI.A) és **affektív tónus** (VI.B/C) **ortogonális** — egy szám lehet `arousal:gentle` + `valence.positive:passionate` (lassú romantikus bachata).

---

## VII. RITMUS & METRIKUS ALAP (`rhythm.*`)

*Az alap lüktetés — külön a musicality szerkezettől.*

| Tag kulcs | Típus | Leírás | Legacy |
| :--- | :--- | :--- | :--- |
| `rhythm.time_signature` | enum | `4_4` / `3_4` / `6_8` / `7_8` / `12_8` / `mixed` / `irregular` | ¾ 7/8 strange |
| `rhythm.time_signature_class` | enum | `simple` / `compound` / `mixed` / `free` | — |
| `rhythm.groove.overall` | enum | `marching` / `uniform` / `syncopated` / `floating` / `pulsing` / `monotone` / `bass_driven` | rhythm bars |
| `rhythm.groove.syncopation` | scale | 0–6 szinkópa-intenzitás | syncopated skála |
| `rhythm.beat_clarity` | enum | `clear` / `soft` / `ambiguous` / `hidden` | clearb |
| `rhythm.flow` | enum | `stream` / `floating` / `pulsing` / `marching` | stream/float/pulse |
| `rhythm.clave_audibility` | enum | `dominant` / `present` / `subtle` / `absent` | clave |

---

## VIII. MUSICALITY & IDŐBELI SZERKEZET (`structure.*`)

*A táncpedagógia „korona-dimenziója" — MAP, szegmensek, irregularitás.*

### VIII.A. Ütemtérkép (MAP)

| Tag kulcs | Leírás | Legacy |
| :--- | :--- | :--- |
| `structure.map.code` | Kódolt MAP string | `[i4! v88 r4!4 4x4]` |
| `structure.map.base_meter` | `4_4_based` / `3_4_based` / `mixed` / `non_standard` | 4x4 / 3/3 / strange |
| `structure.map.eighth_complexity` | `simple` / `moderate` / `complex` | tört nyolcas arány |
| `structure.map.accent_pattern` | `!` kiemelt záró ütemek jelölése | ! |

**MAP szegmens jelölők:**

| Kód | Jelentés |
| :--- | :--- |
| `i` / `i4` | intro (4 nyolcas) |
| `v` / `v88` | verse |
| `ref` / `r4!4` | refrén |
| `B` | bridge (lágyabb váltás) |
| `E` | edge (élesebb váltás) |
| `S@mm:ss` | stop @ időpont |
| `instr` | instrumental szakasz |
| `solo` | szóló |
| `out` / `coda` | outro / coda |
| `part` | jolly joker / részlet |

### VIII.B. Ritmus-változatosság (ütemen belüli)

| Tag kulcs | Skála / enum | Legacy |
| :--- | :--- | :--- |
| `structure.irregularity.level` | 0–6 | homogen → hard |
| `structure.irregularity.pattern_count` | int | hányféle 8-as ritmus |
| `structure.in_beat_qa` | enum | `none` / `subtle` / `moderate` / `rich` | in beat Q&A's |
| `structure.parts_diversity` | scale 0–6 (§) | §1–§6 |
| `structure.style_change_frequency` | enum | `stable` / `occasional` / `frequent` | difficult |

### VIII.C. Automatikus szegmens (MIR kimenet)

| Tag kulcs | Leírás |
| :--- | :--- |
| `structure.segment.{n}.type` | intro/verse/chorus/bridge/break/solo/outro |
| `structure.segment.{n}.start_bar` | 8-as alapú sorszám |
| `structure.segment.{n}.energy_delta` | energia ugrás a szegmens elején |
| `structure.downbeat.grid` | „1" / on1/on2 pozíció JSON |
| `structure.energy_curve` | ütem-szinkron energia-görbe |

---

## IX. CUE-PONTOK & ÁTMENET-TÉR (`cue.*`) — D093

*Musicality-oktatás és DJ/tánctanár **zenei pillanatok** könyvtára. Rendszer- és tanári szint.*

### IX.A. CUE entitás mezők

| Mező | Típus | Leírás |
| :--- | :--- | :--- |
| `cue.id` | uuid | Egyedi azonosító |
| `cue.scope` | enum | `system` (Minősítő) / `teacher` (Ízlés) / `organization` (megosztott) |
| `cue.media_id` | uuid | Melyik számhoz tartozik |
| `cue.time` | `@mm:ss.ms` vagy `bar:N beat:B` | Időpont |
| `cue.type` | enum | lásd IX.B |
| `cue.theme` | term_id / string | Tematikus csoport (pl. „shine prompt", „partner change") |
| `cue.label` | string | Emberi olvasható név |
| `cue.linked_term_ids[]` | term_id[] | KineLex mozdulat/musicality fogalom |
| `cue.strength` | 0–6 | Váltás erőssége / meglepődség |
| `cue.teaching_note` | text | Pedagógiai megjegyzés |
| `cue.transition_family` | uuid | Hasonló CUE-k családja (dedup/ajánló) |
| `cue.source` | enum | `human` / `mir_auto` / `mir_suggested` / `import_legacy` |
| `cue.confidence` | enum | `high` / `low` (review-jelölés) |

### IX.B. CUE típusok (egyenlő távolságú kategóriák)

| `cue.type` | Pedagógiai / DJ jelentés |
| :--- | :--- |
| `downbeat` | „Az egyes" / számolás kezdete |
| `phrase_start` | Új 8-as / musical phrase kezdete |
| `break` | Ütem-szünet, kiállás (shine/hit) |
| `hit` | Percusszív akcentus |
| `stop` | Teljes megállás (@S@) |
| `texture_shift` | Hangzásvilág váltás (edge/bridge) |
| `energy_peak` | Csúcspont / banger pillanat |
| `vocal_entry` | Ének belépés (musicality follow) |
| `instrument_solo` | Hangszer-szóló kezdete |
| `transition_out` | Kilépés / levezetés kezdete |
| `dj_mix_point` | Optimális keverési pont (Camelot/BPM) |
| `class_exercise` | Konkrét gyakorlat indító (tanári CUE) |

### IX.C. CUE-adatbázis működés (D093)

1. **Manuális rögzítés:** tanár/lektor a MeCat lejátszóban (§X) megjelöl → `scope=teacher` vagy jóváhagyva `system`.
2. **Tematizálás:** `cue.theme` + `cue.type` + `linked_term_ids` — kereshető könyvtár („minden salsa on2 break@bar33").
3. **Automatikus feldolgozás (MIR):** új zene scan → break/hit/peak detektálás → **javasolt CUE** (`mir_suggested`).
4. **Hasonlóság-illesztés:** detektált CUE vektor összevetése a CUE-adatbázissal → **egyező** (`match_score ≥ θ`) vagy **hasonló** (`transition_family`) CUE felvétele automatikusan, emberi review-val (P43).
5. **Golden Data:** jóváhagyott CUE-párosítások → `cue_learned_rules` (mint tag_learned_rules).

---

## X. HANGZÁS & PRODUKCIÓ (`production.*`)

| Tag kulcs | Leírás | Legacy |
| :--- | :--- | :--- |
| `production.quality.tier` | `studio` / `good` / `acceptable` / `lo_fi` / `damaged` | — |
| `production.quality.flags` | `old_record` / `noise` / `LQ` / `PB` / `off_key` | oldRec/noise/LQ/PB |
| `production.era` | `archival` / `classic` / `retro` / `evergreen` / `contemporary` / `future` | old/classic/retro/modern |
| `production.source` | `original` / `remix` / `medley` / `chart_edit` / `live` | rmx/chart |
| `production.loudness.lufs` | number | Integrált loudness (D058) |
| `production.loudness.dynamic_range` | enum | `compressed` / `moderate` / `wide` | — |

---

## XI. HANGSZEREK & STÍLUSJEGYEK (`contains.*`)

| Tag kulcs | Példák | Legacy |
| :--- | :--- | :--- |
| `contains.instrument.*` | piano, guitar, bass, percussion, brass, strings, flute, synth, vocal_lead | instruments |
| `contains.style_marker.*` | pachanga, timba, latin_jazz, bigband, symphonic, drum_focus | styles |
| `contains.theme.*` | xmas, new_year, romantic_ballad, story_song, puerto_rico, tropical | theme |
| `contains.vibe.*` | urban, street, oriental, afro, cuban, jazz, funk, soul, rap | vibe |

---

## XII. PEDAGÓGIA & HASZNÁLAT (`usage.*`) — Tánctanár + DJ

*Mihez való a szám — külön domain a műfajtól.*

### XII.A. Órai cél (KineLex `term_id` preferált)

| Tag kulcs | Leírás | Legacy |
| :--- | :--- | :--- |
| `usage.pedagogy.warmup` | Bemelegítés / levezetés | Warmup |
| `usage.pedagogy.basic_step` | Alaplépés, ritmustartás | Flat-Practice |
| `usage.pedagogy.partnerwork` | Páros figura | — |
| `usage.pedagogy.footwork_shine` | Lábmunka / shine | Descarga |
| `usage.pedagogy.musicality_lab` | Musicality feladat | breaks-heavy / mu |
| `usage.pedagogy.styling` | Stílus, interpretáció | — |
| `usage.pedagogy.performance` | Show / fellépés | Show-Revue |
| `usage.pedagogy.practice_flat` | Egyenletes, lapos gyakorló | flat/uniform |

### XII.B. DJ / hangulat-menedzsment

| Tag kulcs | Leírás | Legacy |
| :--- | :--- | :--- |
| `usage.dj.opening` | Est nyitó | — |
| `usage.dj.peak_hour` | Csúcsidő | banger |
| `usage.dj.wind_down` | Levezetés | cooldown |
| `usage.dj.mix_friendly` | Keverésre alkalmas | Camelot input |
| `usage.dj.tool_intro` | DJ intro / edit | DJ-Tools |
| `usage.dj.tool_outro` | DJ outro / clean exit | — |
| `usage.dj.request_friendly` | Közönség-kérésre jó | pop/comm |
| `usage.dj.floor_clearing` | „Ürítő" (ritka, szándékos) | — |

### XII.C. Rendezvény / színpad

| Tag kulcs | Leírás |
| :--- | :--- |
| `usage.stage.demo` | Bemutató óra |
| `usage.stage.competition` | Verseny kategória illeszkedés |
| `usage.stage.social_dance` | Social buli tánc |
| `usage.stage.rueda` | Rueda de casino |
| `usage.stage.performance` | Színpadi koreográfia |

---

## XIII. KERESÉSI PROFillÉPEK (Smart Playlist sablonok)

| Profil | Szűrő példa |
| :--- | :--- |
| **Kezdő salsa on2 óra** | `taxonomy.dance_style_timing=on2` + `usage.pedagogy.basic_step` + `taste.rating.beginner≥4` + `tempo.bpm:160-190` |
| **Shine blokk** | `usage.pedagogy.footwork_shine` + `structure.segment.type=break` + `rhythm.clave_audibility≥present` |
| **Musicality haladó** | `usage.pedagogy.musicality_lab` + `structure.irregularity.level≥3` + `cue.type=break` |
| **Buli peak** | `usage.dj.peak_hour` + `affect.arousal≥surge` + `taste.banger=true` |
| **Bachata romantikus** | `taxonomy.genre=bachata` + `affect.valence.positive.passionate` + `affect.arousal≤moderate` |

---

## XIV. Legacy → MeCat migrációs térkép (rövid)

| Régi jelölés | Új tag |
| :--- | :--- |
| `{45}` | `taste.rating.beginner=4` + `taste.rating.advanced=5` |
| `{+}` | `taste.banger=true` |
| `[mu 4]` | `structure.parts_diversity=4` + `usage.pedagogy.musicality_lab` |
| `on2 descarga` | `taxonomy.dance_style_timing=on2` + `usage.pedagogy.footwork_shine` |
| `°2.23` | `cue.time=@2:23` + `cue.type` (kontextus szerint) |

---

## XV. Nyitott kérdések (ICEBOX)

- **♥ TAG-01:** A `affect.valence.negative` tagek megjelenjenek-e a diák gyakorló-nézetben, vagy csak tanári?
- **♥ TAG-02:** Hány `transition_family` küszöb (θ) elég az auto-CUE illesztésnél?
- **♥ TAG-03:** DJ `usage.dj.floor_clearing` etikai/jogi jelölés szükséges-e?

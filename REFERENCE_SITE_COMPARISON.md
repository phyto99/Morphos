# Reference Site Comparison — Morphos vs. Chromonym & zSnout
*Sources: actual source code fetched from GitHub repos, not screenshots.*
*chromonym/ithkapp → grammardata.json | zsakowitz/ithkuil → generate/ TypeScript files*
*Date: 2026-04-27*

---

## Sites Analyzed

| Site | Repo | Type |
|---|---|---|
| https://chromonym.github.io/ithkapp/ | chromonym/ithkapp | Vue SPA — simplified word builder |
| https://v8.zsnout.com/ithkuil/kit | zsakowitz/ithkuil | TypeScript toolkit — full generator/parser |

---

## Parameter-by-Parameter Comparison

### Configuration

| App | Chromonym | zSnout | Verdict |
|---|---|---|---|
| 9 values (UNI/DPX/DCT…) — **Ithkuil 2011 IDs** | 4 simplified groups (UPX/DPX/D/M) | **20 canonical IDs** | **Fixed** — our app now uses the 20 canonical IV IDs |

zSnout canonical 20 (confirmed from `generate/ca/configuration.ts`):
`UPX, DPX, DSS, DSC, DSF, DDS, DDC, DDF, DFS, DFC, DFF, MSS, MSC, MSF, MDS, MDC, MDF, MFS, MFC, MFF`

Chromonym simplifies to 4 groupings for UX. zSnout is authoritative.

---

### Specification

| App | Chromonym | zSnout |
|---|---|---|
| BSC/CTE/CSV/OBJ ✓ | BSC/CTE/CSV/OBJ ✓ | BSC/CTE/CSV/OBJ ✓ |

**Match across all three. No action needed.**

---

### Context

| App | Chromonym | zSnout |
|---|---|---|
| EXS/FNC/RPS/AMG ✓ | EXS/FNC/RPS/AMG ✓ | EXS/FNC/RPS/AMG ✓ |

**Match. No action needed.**

---

### Version

| App | Chromonym | zSnout |
|---|---|---|
| **Missing** | PRC/CPT ✓ | PRC/CPT ✓ |

**Gap confirmed** — both reference sites have Version. We need to add PRC (Processual) and CPT (Completive) to PARAMS and puzzles.

---

### Affiliation

| App | Chromonym | zSnout |
|---|---|---|
| CSL/ASO/VAR/COA ✓ | CSL/ASO/COA/VAR ✓ | CSL/ASO/VAR/COA ✓ |

**Match. No action needed.**

---

### Perspective

| App | Chromonym | zSnout |
|---|---|---|
| M/G/N/A ✓ | M/G/N/A ✓ | M/G/N/A ✓ |

**Match. No action needed.**

---

### Extension

| App | Chromonym | zSnout |
|---|---|---|
| DEL/PRX/ICP/ATV/GRA/DPL ✓ | DEL/PRX/ICP/ATV/GRA/DPL ✓ | DEL/PRX/ICP/ATV/GRA/DPL ✓ |

**Match. No action needed.**

---

### Essence

| App | Chromonym | zSnout |
|---|---|---|
| NRM/RPV ✓ | NRM/RPV ✓ | NRM/RPV ✓ |

**Match. No action needed.**

---

### Valence

| App | Chromonym | zSnout |
|---|---|---|
| 9 values ✓ | 9 values ✓ | 9 values ✓ |

All three: `MNO PRL CRO RCP CPL DUP DEM CNG PTI`. **Match.**

---

### Phase

| App | Chromonym | zSnout |
|---|---|---|
| PCT/ITR/REP/ITM/RCT/FRE/FRG/VAC/FLC ✓ | PCT/ITR/REP/ITM/RCT/FRE/FRG/VAC/FLC ✓ | PCT/ITR/REP/ITM/RCT/FRE/FRG/VAC/FLC ✓ |

**Match. No action needed.**

---

### Aspect

| App | Chromonym | zSnout |
|---|---|---|
| **9 of 36** | 36 ✓ | 36 ✓ (from `generate/formative/slot-8/aspect.ts`) |

Full 36 confirmed from zSnout source:

| Col 1 — Timeline | Col 2 — Durational | Col 3 — Consequence | Col 4 — Non-temporal |
|---|---|---|---|
| RTR | RSM | PMP | DCL |
| PRS | CSS | CLM | CCL |
| HAB | PAU | DLT | CUL |
| PRG | RGR | TMP | IMD |
| IMM | PCL | XPD | TRD |
| PCS | CNT | LIM | TNS |
| REG | ICS | EPD | ITC |
| SMM | EXP* | PTC | MTV |
| ATP | IRP | PPR | SQN |

*EXP here = Experiential aspect (not Expressive illocution — different category entirely)*

**App missing 27 values (Cols 2–4). High-priority gap.**

---

### Mood

| App | Chromonym | zSnout |
|---|---|---|
| FAC/SUB/ASM/SPC/COU/HYP ✓ | FAC/SUB/ASM/SPC/COU/HYP ✓ | FAC/SUB/ASM/SPC/COU/HYP ✓ |

**Match across all three. No action needed.**

---

### Validation

| App | Chromonym | zSnout |
|---|---|---|
| OBS/REC/PUP/RPR/USP/IMA/CVN/ITU/INF ✓ | OBS/REC/PUP/RPR/USP/IMA/CVN/ITU/INF ✓ | OBS/REC/PUP/RPR/USP/IMA/CVN/ITU/INF ✓ |

**Match. No action needed.**

---

### Illocution

| App (before fix) | Chromonym | zSnout | App (after fix) |
|---|---|---|---|
| ASR/DIR/IRG/ADM/HOR/CNJ/**EXP/OPT**/DEC | ASR/DIR/DEC/IRG/**VER**/ADM/**POT**/HOR/CNJ | ASR/DIR/DEC/IRG/**VRF**/ADM/**POT**/HOR/CNJ | ASR/DIR/IRG/ADM/HOR/CNJ/**VER/POT**/DEC ✓ |

- EXP (Expressive) was **wrong** — replaced with **VER** (Verificative)
- OPT (Optative) was **wrong** — replaced with **POT** (Potentiative)
- CNJ description updated: "Conjectural" (not "Conjunctive/subordinate")
- zSnout uses `VRF`; chromonym uses `VER`. We use `VER` (yuorb canonical).
- **Fixed in commit `64c0ff7`.**

---

### Register

| App (before fix) | Chromonym | zSnout | App (after fix) |
|---|---|---|---|
| NRR/DSV/**PNT(Punctuative)**/SPF/EXM/**COG(Cogitative)**/END(Endophoric) | Not in grammardata.json | NRR/DSV/PNT(Parenthetical)/SPF/EXM/**CGT(Cogitant)**/END(Carrier-End) | NRR/DSV/PNT(Parenthetical)/SPF/EXM/**CGT(Cogitant)**/END(Carrier-End) ✓ |

Three label/ID errors fixed:
- `COG` → `CGT` (Cogitant)
- `PNT` label "Punctuative" → "Parenthetical"
- `END` label "Endophoric" → "Carrier-End"
- **Fixed in commit `64c0ff7`.**

---

### Bias

| App | Chromonym | zSnout |
|---|---|---|
| 59 → **61** (after fix) | Not detailed | **63 values** from `generate/adjunct/bias.ts` |

Added `CRP` (Corruptive) and `PPV` (Propositive). May still be missing a few — zSnout lists 63, our app now has 61.

---

### Ca Morphology Table (config × affiliation → affix)

| App (before) | zSnout | App (after) |
|---|---|---|
| 9×4 = 36 entries, **wrong consonants** | 20×4 = 80 entries, canonical | 20×4 = 80 entries, canonical ✓ |

Old table used invented/Ithkuil-2011 affixes. New table from `generate/ca/affiliation.ts`:
- Config form: per-config consonant (e.g. MSS="t", MDF="ç")
- Affiliation suffix: CSL="" (silent), ASO="l", VAR="ř", COA="r"
- Combined: Ca = cfg_form + afl_suffix (e.g. MSS+ASO = "tl")

**Fixed in commit `7611002`.**

---

## Feature Coverage Summary

| Feature | Chromonym | zSnout | Our App | Status |
|---|---|---|---|---|
| Configuration (20) | Simplified (4) | ✓ | ✓ | Fixed |
| Specification (4) | ✓ | ✓ | ✓ | Done |
| Context (4) | ✓ | ✓ | ✓ | Done |
| Version (2) | ✓ | ✓ | **Missing** | TODO |
| Affiliation (4) | ✓ | ✓ | ✓ | Done |
| Perspective (4) | ✓ | ✓ | ✓ | Done |
| Extension (6) | ✓ | ✓ | ✓ | Done |
| Essence (2) | ✓ | ✓ | ✓ | Done |
| Valence (9) | ✓ | ✓ | ✓ | Done |
| Phase (9) | ✓ | ✓ | ✓ | Done |
| Aspect (36) | ✓ | ✓ | 9/36 | **Gap** |
| Mood (6) | ✓ | ✓ | ✓ | Done |
| Validation (9) | ✓ | ✓ | ✓ | Done |
| Illocution (9) | ✓ | ✓ | ✓ | Fixed |
| Register (7) | ? | ✓ | ✓ | Fixed |
| Bias (~63) | Partial | ✓ | 61 | Near-done |
| Case-Scope (6) | ? | ✓ | **Missing** | TODO |
| Concatenation/Format | ? | ✓ | **Missing** | TODO |
| Suppletive Adjuncts (4) | ? | ✓ | **Missing** | TODO |
| Modular Adjuncts | ? | ✓ | **Missing** | TODO |

---

## Notable Differences in Approach

| Dimension | Chromonym | zSnout | Our App |
|---|---|---|---|
| Purpose | Quick builder for casual use | Full TypeScript library + toolkit | Spaced-repetition learning |
| Configuration display | 4 simplified groups | All 20 with full names | All 20 with 3D visual system |
| Aspect coverage | All 36 | All 36 | 9/36 (learning focus) |
| Bias | Likely partial | All 63 | 61 |
| Learning/quiz | None | Partial (learn page) | Full FSRS + 10 engine types |
| Synesthesia | None | None | Full audio+color+spatial |
| Morpheme audio | None | None | Tone mapping per value |

---

## What We Do Better

- Spaced repetition (FSRS-4.5) — neither reference site has this
- Synesthetic color/audio/spatial encoding per parameter
- 10 puzzle engine types (MCQ/MATCH/SORT/SPEED/BUILD/MULTI/VISUAL/INPUT/SENTENCE)
- HookTip story anecdotes per value
- Complete learning narrative across 13 chapters
- Formative word builder with live IK library generation

## What They Do That We Don't

- zSnout: full formative parser (Ithkuil → glossed breakdown)
- zSnout: root/affix database search (800+ roots)
- zSnout: complete script/writing system renderer
- zSnout: Case-Scope, Format, Suppletive Adjuncts, Modular Adjuncts
- Both: full 36-aspect coverage

---

## Action Items From This Comparison

| Priority | Item | Source |
|---|---|---|
| High | Add Version (PRC/CPT) to PARAMS + puzzles | Both sites |
| High | Add Aspect Cols 2–4 (27 values) | Both sites |
| Medium | Add Case-Scope (CCN/CCA/CCS/CCQ/CCP/CCV) | zSnout |
| Medium | Add 2 remaining bias values to reach 63 | zSnout |
| Lower | Suppletive adjuncts (CAR/QUO/NAM/PHR) | zSnout |
| Lower | Modular adjuncts | zSnout |

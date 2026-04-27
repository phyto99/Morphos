# Yuorb Gap Analysis — Morphos Integration
*Source: yuorb.github.io, chapters 01–15. Date audited: 2026-04-27.*
*Purpose: roadmap of missing/incorrect content for future fixes.*

---

## FULLY COVERED ✓

| Category | Values | Notes |
|---|---|---|
| Affiliation | 4: CSL/ASO/COA/VAR | ✓ |
| Perspective | 4: M/G/N/A | ✓ (was P, now G) |
| Extension | 6: DEL/PRX/ICP/ATV/GRA/DPL | ✓ |
| Essence | 2: NRM/RPV | ✓ |
| Mood | 6: FAC/SUB/ASM/SPC/COU/HYP | ✓ |
| Validation | 9: OBS/REC/PUP/RPR/USP/IMA/CVN/ITU/INF | ✓ |
| Phase | 9: PCT/ITR/REP/ITM/RCT/FRE/FRG/VAC/FLC | ✓ |
| Bias | ~59 of ~64 | See partial section |
| Register | 7: NRR/DSV/PNT/SPF/EXM/COG/END | Verify IDs below |
| Illocution | 9 values | Verify IDs below |

---

## PARTIALLY COVERED ⚠

### Configuration (9 of 20 covered)
Canonical set has **20 configurations** — app only teaches 9.
Missing 11:

| Missing ID | Name |
|---|---|
| MSS | Multiplex-Similar-Separate |
| MSC | Multiplex-Similar-Connected |
| MSF | Multiplex-Similar-Fused |
| MDS | Multiplex-Dissimilar-Separate |
| MDC | Multiplex-Dissimilar-Connected |
| MDF | Multiplex-Dissimilar-Fused |
| MFS | Multiplex-Fuzzy-Separate |
| MFC | Multiplex-Fuzzy-Connected |
| MFF | Multiplex-Fuzzy-Fused |
| DSS | Duplex-Similar-Separate |
| DSC | Duplex-Similar-Connected |

(also DSF/DDS/DDC/DDF/DDS/DFS/DFC/DFF still need audit vs app)

### Aspect (9 of 36 covered)
App covers only Column 1 (RTR/PRS/HAB/PRG/IMM/PCS/REG/SMM/ATP).
**27 values missing:**

| Column | Missing IDs |
|---|---|
| Col 2 — Durational | RSM, CSS, PAU, RGR, PCL, CNT, ICS, EXP, IRP |
| Col 3 — Consequence | PMP, CLM, DLT, TMP, XPD, LIM, EPD, PTC, PPR |
| Col 4 — Non-temporal | DCL, CCL, CUL, IMD, TRD, TNS, ITC, MTV, SQN |

### Cases (≈58 of 68 covered)
All 68 canonical cases in 8 families. Verify app has every one:

| Family | Count | Canonical IDs |
|---|---|---|
| Transrelative | 9 | THM, INS, ABS, AFF, STM, EFF, ERG, DAT, IND |
| Appositive | 9 | POS, PRP, GEN, ATT, PDC, ITP, OGN, IDP, PAR |
| Associative | 9 | APL, PUR, TRA, DFR, CRS, TSP, CMM, CMP, CSD |
| Adverbial | 9 | FUN, TFM, CLA, RSL, CSM, CON, AVR, CVS, SIT |
| Relational | 8 | PRN, DSP, COR, CPS, COM, UTL, PRD, RLT |
| Affinitive | 8 | ACT, ASI, ESS, TRM, SEL, CFM, DEP, VOC |
| Spatio-Temporal I | 8 | LOC, ATD, ALL, ABL, ORI, IRL, INV, NAV |
| Spatio-Temporal II | 8 | CNR, ASS, PER, PRO, PCV, PCR, ELP, PLM |

**Known suspect IDs in app**: ORT (should be ORI?), INT (canonical?), ABE/CSM overlap check, ERG/DAT/IND may be absent.

### Bias (≈59 of 64 covered)
Potentially missing 5–9 IDs. Candidates from audit: `DLC DOL FOR IPT PSC PSM RAC RVL SOL`
*Need direct yuorb table to confirm exact 64.*

### Referentials (partial coverage)
App has "basic" referentials. Missing:
- Full 11 referent categories: `1m, 2m, 2p, ma, pa, mi, pi, Mx, Rdp, Obv, PVS`
- Effect variants (NEUTRAL / BENEFICIAL / DETRIMENTAL) per referent
- Special affix categories: AGGLOMERATIVE (`-ļ-/-tļ-`), NOMIC (`-ç-/-x-`), ABSTRACT (`-w/-y-`)
- Slot structure for referentials (Single-Referent 2-slot / Dual-Referent 5-slot / Combination 6-slot)

### Affixes (partial)
- 7 affix gradient types (Type 0, A1, A2, B, C, D1, D2) not taught
- 30-form vowel-form matrix not covered
- Case-accessor affixes (all 68 cases usable as affixes in Slots V/VII) — likely incomplete

---

## COMPLETELY MISSING ✗

### 1. Version (2 values) — EASY ADD
- **PRC** — Processual (default/unmarked; goal-neutral)
- **CPT** — Completive (goal/result-oriented)
- Lives in Slot II alongside Stem.

### 2. Specification (4 values) — CRITICAL, Slot IV
- **BSC** — Basic
- **CTE** — Contential
- **CSV** — Constitutive
- **OBJ** — Objective
- Mandatory Slot IV component alongside Function and Context.

### 3. Context (4 values) — CRITICAL, Slot IV
- **EXS** — Existential
- **FNC** — Functional
- **RPS** — Representational
- **AMG** — Amalgamative
- Mandatory Slot IV component.

### 4. Valence (9 values) — MAJOR VERBAL CATEGORY
- MNO, PRL, CRO, RCP, CPL, DUP, DEM, CNG, PTI
- Governs agent/patient relationship between two participants.

### 5. Effect (9 values) — VERBAL MODULATION
Values encode who benefits/is harmed:
`1:BEN, 2:BEN, 3:BEN, SLF:BEN, UNK, SLF:DET, 3:DET, 2:DET, 1:DET`

### 6. Level (9 values) — VERBAL MODULATION
Scalar gradation values:
`MIN, SBE, IFR, DFC, EQU, SUR, SPL, SPQ, EXC`

### 7. Case-Scope (6 values) — CRITICAL FOR MULTI-FORMATIVE SYNTAX
- **CCN** — Natural (default)
- **CCA** — Antecedent
- **CCS** — Subaltern
- **CCQ** — Qualifier
- **CCP** — Precedent
- **CCV** — Successive
- Determines which formative a case-marked formative modifies.

### 8. Concatenation / Format — CONSTRUCTION TYPE
- Type-1 concatenation: Circumstantial (adjunct-like relationship)
- Type-2 concatenation: Derivational (creates compound)
- Format: 68 values parallel to the case system (Vf slot)
- Governs how multi-word constructions join.

### 9. Carrier Root System (-S-)
- Special root for introducing foreign words and proper names
- 3 stems × 4 specifications (BSC/CTE/CSV/OBJ) = 12 combinations

### 10. Modular Adjuncts
- Adjunct type that carries Valence, Phase, Level, Effect, Aspect, Mood information
- Scope markers: `ʼ` (default), `w` (parent), `y` (concatenated)

### 11. Suppletive Adjuncts (4 types)
- **CAR** — Carrier adjunct
- **QUO** — Quotative adjunct
- **NAM** — Naming adjunct
- **PHR** — Phrasal adjunct

### 12. Parsing Adjuncts
- Mark syntactic boundaries in complex utterances.

### 13. Case-Frame / Relation (binary)
- UNFRAMED (default) vs. FRAMED
- Determines whether a formative takes Vc (Case) or Vf (Format) in Slot IX
- Marked by stress pattern (penultimate vs. antepenultimate)

### 14. WH-Question Construction
Three mechanisms:
1. DIRECTIVE illocution (yes/no questions)
2. PVS referential + IVL₁/₄ affix `-inļ`
3. PTN affix degrees 1, 2, 3, 8, 9

### 15. Case-Stacking on Verbs
- Applying case to unframed verbal formatives for adverbial meaning.

### 16. Specialized Cs-Roots
- Elevating an affix to root status using special Slot II indicators (ëi, eë, ëu, oë)

### 17. Specialized Referential Roots
- PRC and CPT versions × 4 specifications

### 18. Transrelative Specialized Affixes (9)
- CNS, RSN, DLB, XPT, ENB, IMP, AGN, SIA, CHC

### 19. Locative Particle Strings (3 systems)
- Translative Motion Roots: -TR-, -PR-, -KR-, etc.
- Spatial Position Roots: -Ţ-, -Ḑ-, -P-, -K-, etc.
- Positionally-Defined Component Roots: -ŢF-

### 20. Syntax Rules
- Semantic roles (Agent, Patient, Experiencer, etc.) — not taught
- Pragmatic roles (Topic, Comment, Focus) — not taught
- Word order rules: verb-initial main clause, topic sentence-initial, focus pre-verbal
- Subordinate clause verb-initial mandatory + TPF affix for focus/topic

### 21. Numbers System
- Centesimal (base-100) system
- Number roots 0–9
- TNX affix for multiples of ten
- Number affixes: XX2–XX9, X10, XOH, XTT, XTM, XTQ, UHN
- Partitive case for base units, Comitative for combinations

### 22. Geographic Nomenclature Affixes
- **CLG** (`-ḑc-`): Cultural/Geo-Demographic Association (9 degrees)
- **OGC** (`-dn-`): Orientation Relative to Geographic Central Point (9 directional values)

---

## ID ERRORS — FIXED ✓

Confirmed via chromonym/ithkapp source (grammardata.json) and zsakowitz/ithkuil source:

| Was | Now | Category | Source |
|---|---|---|---|
| EXP (Expressive) | **VER** (Verificative) | Illocution | zsnout slot-9/illocution.ts |
| OPT (Optative) | **POT** (Potentiative) | Illocution | chromonym grammardata.json |
| CNJ (Conjunctive) | **CNJ** (Conjectural) | Illocution — desc updated | chromonym |
| COG (Cogitative) | **CGT** (Cogitant) | Register | zsnout adjunct/register.ts |
| PNT "Punctuative" | **PNT** "Parenthetical" | Register label | zsnout adjunct/register.ts |
| END "Endophoric" | **END** "Carrier-End" | Register label | zsnout adjunct/register.ts |
| (missing) | **CRP** (Corruptive) | Bias | zsnout adjunct/bias.ts |
| (missing) | **PPV** (Propositive) | Bias | zsnout adjunct/bias.ts |

*Note: zsnout uses VRF for Verificative; chromonym uses VER. We use VER (yuorb canonical).*

## IDs STILL TO VERIFY

| Suspect ID | Issue | Check against |
|---|---|---|
| ORT | Should be ORI (Orientative)? | Yuorb Ch.04 ST-I table |
| INT | Canonical case? | Yuorb Ch.04 full case list |
| ABE | Transrelative case — canonical? | Yuorb Ch.04 |
| CSM | Adverbial or Transrelative? | Yuorb Ch.04 |
| FNC | Case or Context value? (name collision) | Yuorb Ch.03 + Ch.04 |

## CONFIGURATION — MAJOR PENDING REFACTOR

**Our app uses Ithkuil 2011 IDs** (UNI/DPX/DCT/AGG/SEG/CPN/COH/CST/MUL — 9 values).
**Canonical Ithkuil IV IDs** (confirmed zsnout generate/ca/configuration.ts):

| ID | Full Name |
|---|---|
| UPX | Uniplex |
| DPX | Duplex |
| DSS | Duplex Similar Separate |
| DSC | Duplex Similar Connected |
| DSF | Duplex Similar Fused |
| DDS | Duplex Dissimilar Separate |
| DDC | Duplex Dissimilar Connected |
| DDF | Duplex Dissimilar Fused |
| DFS | Duplex Fuzzy Separate |
| DFC | Duplex Fuzzy Connected |
| DFF | Duplex Fuzzy Fused |
| MSS | Multiplex Similar Separate |
| MSC | Multiplex Similar Connected |
| MSF | Multiplex Similar Fused |
| MDS | Multiplex Dissimilar Separate |
| MDC | Multiplex Dissimilar Connected |
| MDF | Multiplex Dissimilar Fused |
| MFS | Multiplex Fuzzy Separate |
| MFC | Multiplex Fuzzy Connected |
| MFF | Multiplex Fuzzy Fused |

This affects: TONE_MAP, PARAMS.configuration.values (all 9 entries), Ca morphology table (IK_CFG),
all puzzle data referencing DCT/AGG/SEG/CPN/COH/CST/MUL, and all SVG shape visualizations.
IK_CFG currently patches old→new for formative generation but the learning labels are still wrong.
**This is the highest-priority remaining fix.**

---

## PRIORITY ORDER FOR FUTURE WORK

### Critical / Do First
1. Verify suspect IDs (register/illocution) — check yuorb directly before committing
2. Add **Version** (PRC/CPT) — quick win, small addition
3. Add **Specification** (BSC/CTE/CSV/OBJ) — Slot IV, mandatory
4. Add **Context** (EXS/FNC/RPS/AMG) — Slot IV, mandatory
5. Fix **Case** gaps — audit full 68 vs app list, add missing, fix ID errors

### High Value
6. Add **Valence** (9 values: MNO/PRL/CRO/RCP/CPL/DUP/DEM/CNG/PTI)
7. Add **Case-Scope** (6 values: CCN/CCA/CCS/CCQ/CCP/CCV)
8. Add **Aspect** columns 2–4 (27 missing values)
9. Add **Effect** (9 values) — already partially in app, verify IDs
10. Add **Level** (9 values) — already partially in app, verify IDs

### Medium
11. Expand **Configuration** (11 missing of 20)
12. Add **Concatenation/Format** concept and puzzles
13. Add **Modular Adjuncts** explanation
14. Add **Suppletive Adjuncts** (CAR/QUO/NAM/PHR)
15. Expand **Referentials** (full 11 referent types + effects)
16. Add **Syntax** chapter (word order, topic/focus/comment)
17. Add **WH-Question** construction module
18. Add **Case-Frame/Relation** binary

### Lower
19. Bias: find and add missing 5–9 IDs
20. Numbers system (centesimal base-100)
21. Affix gradient types (7 formal types)
22. Parsing adjuncts
23. Locative particle strings
24. Case-stacking on verbs
25. Geographic nomenclature affixes (CLG/OGC)

---

## NOT TO ADD (confirmed absent from Ithkuil IV)
- Sanction — 2011 only, removed from IV
- Designation — 2011 only, removed from IV
- Modality — not a separate parameter in IV
- "Merged/Distributed Perspective" — not canonical
- Version with 4 values — only 2 (PRC/CPT)

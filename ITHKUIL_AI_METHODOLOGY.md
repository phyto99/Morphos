# Ithkuil AI Chatbot — Methodology & Architecture
*Design document for a semantically rigorous Ithkuil ↔ English AI system.*
*Status: pre-implementation prototype. Companion visual prototype: `chatbot-visual-prototype.html`*

---

## Core Philosophy

Ithkuil is not a natural language — it is a precision instrument. A translator/chatbot cannot use
statistical paraphrase the way GPT-style models do. Every morpheme is load-bearing. The system
must:

1. **Decompose** the input into explicit semantic/pragmatic atoms
2. **Audit** each atom for ambiguity, implicit assumptions, and unstated presuppositions
3. **Reconstruct** into Ithkuil morpheme-by-morpheme with every choice justified
4. **Visualize** the entire reasoning chain as color-coded blocks

---

## The Pipeline

```
INPUT (English or Ithkuil)
    │
    ▼
[STAGE 1] SEMANTIC DECOMPOSITION
    │  Break into: propositions, participants, predicates,
    │  modality, evidentiality, pragmatic intent
    ▼
[STAGE 2] AMBIGUITY AUDIT
    │  Flag every underspecified element.
    │  List alternative interpretations with probability weights.
    ▼
[STAGE 3] PARAMETER MAPPING
    │  Map each semantic atom to an Ithkuil parameter + value.
    │  Show confidence. Show alternatives considered.
    ▼
[STAGE 4] MORPHEME ASSEMBLY
    │  Build slot-by-slot (Slots I–X).
    │  Each slot shows: chosen value, reason, alternatives.
    ▼
[STAGE 5] OUTPUT
       Ithkuil formative string + gloss + back-translation
```

---

## Block System — Color Coding

Each block in the visual output corresponds to a semantic or grammatical category.
Color scheme mirrors the app's existing PARAMS hex palette.

| Block Type | Color | Covers |
|---|---|---|
| **ROOT** | `#6366f1` indigo | Semantic root + stem selection |
| **FUNCTION** | `#8b5cf6` violet | STA / DYN — stative vs. dynamic |
| **SPECIFICATION** | `#a78bfa` lavender | BSC / CTE / CSV / OBJ |
| **CONTEXT** | `#7c3aed` deep violet | EXS / FNC / RPS / AMG |
| **CONFIG** | `#3b82f6` blue | Configuration (1 of 20) |
| **AFFILIATION** | `#06b6d4` cyan | CSL / ASO / COA / VAR |
| **PERSPECTIVE** | `#10b981` emerald | M / G / N / A |
| **EXTENSION** | `#84cc16` lime | DEL / PRX / ICP / ATV / GRA / DPL |
| **ESSENCE** | `#eab308` amber | NRM / RPV |
| **VERSION** | `#f97316` orange | PRC / CPT |
| **ASPECT** | `#ef4444` red | 1 of 36 aspect values |
| **PHASE** | `#f43f5e` rose | 1 of 9 phase values |
| **VALENCE** | `#ec4899` pink | 1 of 9 valence values |
| **MOOD** | `#14b8a6` teal | FAC / SUB / ASM / SPC / COU / HYP |
| **VALIDATION** | `#0ea5e9` sky | 1 of 9 validation values |
| **ILLOCUTION** | `#64748b` slate | ASR / DIR / IRG / ADM / HOR / CNJ / EXP / OPT / DEC |
| **REGISTER** | `#475569` steel | NRR / DSV / PNT / SPF / EXM / COG / END |
| **CASE** | `#854d0e` brown | 1 of 68 cases + family |
| **CASE-SCOPE** | `#92400e` dark brown | CCN / CCA / CCS / CCQ / CCP / CCV |
| **BIAS** | `#be185d` crimson | 1 of 64 bias values |
| **AFFIX** | `#0f766e` dark teal | Slot V / VII affixes |
| **AMBIGUITY** | `#dc2626` bright red | Unresolved forks — requires clarification |
| **IMPLICIT** | `#9ca3af` grey | Assumed defaults (shown but not stated) |
| **DISCARDED** | `#374151` dark grey | Considered but rejected interpretations |

---

## Block Structure (per block)

Each rendered block contains:

```
┌─────────────────────────────────────────────┐
│ [CATEGORY]          [CHOSEN VALUE]    [CONF] │
│ ─────────────────────────────────────────── │
│ Reason: "the verb 'study' implies ongoing   │
│ purposeful action without goal-boundary"    │
│ ─────────────────────────────────────────── │
│ Alternatives considered:                    │
│   • CPT (0.3) — if completion is implied    │
│   • [ASK USER] — sentence is ambiguous      │
└─────────────────────────────────────────────┘
```

Fields:
- **CATEGORY**: parameter name (e.g., VERSION)
- **CHOSEN VALUE**: canonical ID + label (e.g., PRC — Processual)
- **CONF**: confidence 0.0–1.0 (shown as color fill)
- **Reason**: one-sentence justification referencing the input
- **Alternatives**: other values considered + why rejected

---

## Decoding Mode (Ithkuil → English)

When input is Ithkuil:

1. **Tokenize** by slot boundaries
2. **Parse** each slot morpheme → parameter + value
3. **Generate block** per parameter — no ambiguity at this stage (Ithkuil is unambiguous)
4. **Reconstruct** in English — but flag that English *cannot* fully express the precision
5. Show **semantic residue**: what is lost in translation

```
Ithkuil input: [formative string]
    │
    ▼
Slot I:  Concatenation type  →  [CONCAT block]
Slot II: Version + Stem       →  [VERSION block] + [STEM block]
Slot III: Root                →  [ROOT block]
Slot IV: Vr (Fn+Spec+Ctx)    →  [FUNCTION block] + [SPEC block] + [CONTEXT block]
Slot V:  CsVx affixes        →  [AFFIX blocks...]
Slot VI: Ca complex           →  [CONFIG] + [AFFIL] + [PERSPECT] + [EXT] + [ESSENCE]
Slot VII: VxCs affixes       →  [AFFIX blocks...]
Slot VIII: VnCn              →  [ASPECT/PHASE/VALENCE/LEVEL/EFFECT] + [MOOD/CASE-SCOPE]
Slot IX: Vc/Vf/Vk            →  [CASE or FORMAT] + [ILLOCUTION/VALIDATION]
Slot X:  Stress pattern      →  [FRAME block]
    │
    ▼
English gloss (with residue warnings)
```

---

## Encoding Mode (English → Ithkuil)

The hardest direction. English is systematically underspecified relative to Ithkuil.

### Step 1 — Proposition extraction
Break the sentence into atomic propositions.
> "She was probably studying when I arrived"
→ Prop A: [she] [study] [ongoing]
→ Prop B: [I] [arrive] [completed, past]
→ Relation: Prop A temporally contains Prop B

### Step 2 — Underspecification audit
For each proposition, identify what Ithkuil *requires* that English left unstated:

| Required param | English said | Assumed | Confidence |
|---|---|---|---|
| Configuration | (none) | UNI (one entity) | 0.85 |
| Extension | (none) | DEL (bounded) | 0.7 |
| Perspective | (none) | M (monadic) | 0.9 |
| Version | "was studying" | PRC (process) | 0.8 |
| Validation | "probably" | USP (presumptive) | 0.95 |
| Aspect | "was ... when" | PRG (progressive) + ICP (inceptive) | 0.6 |

Items with confidence < 0.6 → **AMBIGUITY block** (ask user or list alternatives)

### Step 3 — Reconstruction
Assemble morphemes for the highest-confidence interpretation.
Show full block chain. Low-confidence choices highlighted in yellow.

### Step 4 — Clarification request (if needed)
> "I need to confirm one thing: by 'she was studying,' do you mean:
> (A) she was in the middle of a study session (PRG aspect), or
> (B) she was regularly in the habit of studying at that time (HAB aspect)?"

---

## Handling Genuine Ithkuil Speech

When the AI is **speaking in Ithkuil** (not translating — actually composing):

1. The AI selects a proposition to express
2. Runs full parameter selection (all slots) — no English intermediary
3. Generates Ithkuil formative string
4. Shows the block chain as "reasoning visible"
5. Provides gloss on request; does NOT provide gloss by default (defeats the purpose)

This mode trains the user to read Ithkuil directly rather than relying on English scaffolding.

---

## Ambiguity Resolution Strategies

| Ambiguity type | Strategy |
|---|---|
| Lexical (which root?) | Show top 3 roots by semantic distance; ask user |
| Aspectual (ongoing vs. habitual?) | Show both; ask user to pick |
| Perspectival (one entity or archetype?) | Default to M; flag if context suggests N/A |
| Evidential (how does speaker know?) | Default to OBS; flag as assumption |
| Illocutionary (statement or question?) | Detectable from syntax; low ambiguity |
| Config (how many, what arrangement?) | English often omits — always flag |
| Valence (who does what to whom?) | Parse syntactic roles; show result |

---

## Visual Block UI Layout

```
┌─────────────────────────────────────────────────────────────────┐
│  INPUT                                                          │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  "She was probably studying when I arrived."              │  │
│  └───────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────┤
│  DECOMPOSITION                                                  │
│                                                                 │
│  [ROOT: study/learn]  [FUNCTION: DYN]  [VERSION: PRC ●●●●○]   │
│  [SPEC: BSC]          [CONTEXT: EXS]   [ASPECT: PRG ●●●○○]    │
│  [CONFIG: UNI ●●●●○]  [PERSPECT: M]   [VALID: USP ●●●●●]     │
│  [MOOD: FAC]          [CASE: ABS]      [ILLOCUT: ASR]          │
│                                                                 │
│  ⚠ AMBIGUITY: Aspect PRG vs HAB — sentence is compatible       │
│    with both. [PRG] [HAB] [ASK USER]                           │
│                                                                 │
│  ░░ IMPLICIT: Config=UNI assumed (no plural marker in English)  │
├─────────────────────────────────────────────────────────────────┤
│  ITHKUIL OUTPUT                                                 │
│  [formative string]                                             │
│  Slot-by-slot gloss ▼                                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## Implementation Notes

- Block system is React components, mirrors Morphos color palette
- Each block is a `<SemanticBlock>` with category/value/confidence/reason/alts props
- Confidence rendered as colored fill bar (0–1) inside block
- AMBIGUITY blocks have pulsing red border; IMPLICIT blocks have dashed grey border
- User can click any block to expand alternatives and override
- Overrides propagate downstream (changing Aspect may invalidate assembled formative)
- Full block chain is serializable — can be saved as a "translation session"

---

## What Makes This Different from Existing Tools

| Feature | Chromonym Ithkapp | zSnout Kit | This System |
|---|---|---|---|
| Shows reasoning | ✗ | ✗ | ✓ (block chain) |
| Handles ambiguity | ✗ | ✗ | ✓ (flags + resolves) |
| Speaks genuine Ithkuil | ✗ | ✗ | ✓ (no English crutch) |
| Teaches via deconstruction | ✗ | Partial | ✓ (every choice explained) |
| Decodes with residue warnings | ✗ | Partial | ✓ |
| Full 36-aspect coverage | Partial | ✓ | ✓ |
| Semantic confidence scoring | ✗ | ✗ | ✓ |

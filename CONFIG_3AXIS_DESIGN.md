# Configuration — 3-Axis System Redesign
*Design doc for replacing the flat 9-value (Ithkuil 2011) Configuration list*
*with a 3-axis combinatorial system matching Ithkuil IV's canonical 20 values.*

---

## The Core Idea

Configuration in Ithkuil IV is not 20 arbitrary vocabulary items — it is 3 independent
binary/ternary axes combined. The learner should build up the system axis by axis, then
combine. Cognitive load drops from "memorize 20 labels" → "understand 3 concepts."

### The 3 Axes

| Axis | Dimension | Values |
|---|---|---|
| **Plexity** | How many? | U (Uniplex — one) / D (Duplex — two) / M (Multiplex — many) |
| **Similarity** | How alike? | S (Similar) / D (Dissimilar) / F (Fuzzy/unknown) |
| **Separability** | How connected? | S (Separate) / C (Connected) / F (Fused) |

**Special cases:**
- `UPX` = Uniplex — Similarity and Separability are irrelevant (one thing can't compare to itself)
- `DPX` = Duplex with Similarity/Separability *unspecified* — speaker declines to mark them

### The 20 IDs Derived

```
UPX                          (Uniplex — axes 2+3 N/A)
DPX                          (Duplex — axes 2+3 unspecified)
D×S×S = DSS   D×S×C = DSC   D×S×F = DSF
D×D×S = DDS   D×D×C = DDC   D×D×F = DDF
D×F×S = DFS   D×F×C = DFC   D×F×F = DFF
M×S×S = MSS   M×S×C = MSC   M×S×F = MSF
M×D×S = MDS   M×D×C = MDC   M×D×F = MDF
M×F×S = MFS   M×F×C = MFC   M×F×F = MFF
```

---

## PARAMS Structure Changes

### Replace `PARAMS.configuration` with 3 separate axis params

```javascript
PARAMS.plexity = {
  label:"Plexity", slot:"VI (Ca)", cssVar:"--cfg", hex:"#16a34a",
  synNote:"Pitch height — low=one, mid=two, high=many",
  spatialAxis:"y",
  values:[
    {id:"U", label:"Uniplex",   icon:"●",  freq:261.6, hue:"#16a34a",
      desc:"A single bounded instance — one, complete, undivided",
      hook:"One apple. Not apples — this specific, complete, single thing."},
    {id:"D", label:"Duplex",    icon:"●●", freq:392.0, hue:"#0369a1",
      desc:"Exactly two instances of the referent",
      hook:"Two of something — the speaker is saying: precisely two."},
    {id:"M", label:"Multiplex", icon:"∷",  freq:523.3, hue:"#7c2d12",
      desc:"More than two instances — an open-ended plurality",
      hook:"Many somethings — more than two, the exact number left open."},
  ]
};

PARAMS.similarity = {
  label:"Similarity", slot:"VI (Ca)", cssVar:"--cfg-sim", hex:"#0f9548",
  synNote:"Timbre — pure tone=similar, rough=dissimilar, noisy=fuzzy",
  spatialAxis:"x",
  values:[
    {id:"S", label:"Similar",    icon:"◎", freq:440.0, hue:"#15803d",
      desc:"The instances are of the same kind — alike in nature or type",
      hook:"Two matching socks. Multiple coins of the same denomination."},
    {id:"D", label:"Dissimilar", icon:"○", freq:554.4, hue:"#075985",
      desc:"The instances differ in kind — distinguishably different from each other",
      hook:"A spoon and a fork. A crowd of strangers — all different."},
    {id:"F", label:"Fuzzy",      icon:"⊙", freq:659.3, hue:"#44403c",
      desc:"Similarity is unclear, unknown, or irrelevant to the speaker",
      hook:"Two wrapped gifts — you can't tell if they're the same thing inside."},
  ]
};

PARAMS.separability = {
  label:"Separability", slot:"VI (Ca)", cssVar:"--cfg-sep", hex:"#0f9548",
  synNote:"Spatial pan — left=separate, center=connected, right=fused",
  spatialAxis:"z",
  values:[
    {id:"S", label:"Separate",   icon:"· ·", freq:440.0, hue:"#166534",
      desc:"The instances are spatially or conceptually apart from each other",
      hook:"Coins scattered across a table — none touching."},
    {id:"C", label:"Connected",  icon:"··",  freq:523.3, hue:"#0f766e",
      desc:"The instances are in contact or structurally linked",
      hook:"Grapes in a bunch — individual but touching, forming a natural cluster."},
    {id:"F", label:"Fused",      icon:"◼",   freq:659.3, hue:"#1c1917",
      desc:"The instances are merged into a single undifferentiated whole",
      hook:"A snowball — many flakes compressed; a spork — two utensils fused into one."},
  ]
};
```

### Keep a derived `PARAMS.configuration` for formative generation

The buildForm() function needs a single Ca ID. Keep a thin derived param:

```javascript
// Utility: derive Ca ID from 3 axis values
function getCaId(plexity, similarity, separability) {
  if (plexity === 'U') return 'UPX';
  if (!similarity || !separability) return plexity === 'D' ? 'DPX' : 'MSS'; // fallback
  return plexity + similarity + separability; // e.g. "DSC", "MDF"
}
```

---

## Puzzle System Changes

### New puzzle modules (axis-by-axis)

**Module A — Plexity only** (easy entry)
- MCQ: "A single apple" → U / D / M
- SPEED: flash a scenario, pick Plexity fast
- Visuals: ● vs ●● vs ∷ icons

**Module B — Similarity only**
- MCQ: "Two matching socks" → Similar / Dissimilar / Fuzzy
- Scenarios focus on contrast: identical twins vs odd couple vs mystery

**Module C — Separability only**
- MCQ: "Grapes in a bunch" → Separate / Connected / Fused
- Physical metaphors: scattered coins / touching grapes / merged snowball

**Module D — Full 3-axis combination** (hard, later chapter)
- BUILD: pick each axis with 3 dropdowns → system shows Ca ID
- MATCH: scenario → Ca ID (e.g. "a pile of identical tiles" → MSF)
- INPUT: type the 3-letter Ca ID from a description
- SORT: order 5 configs from least to most structurally complex

### Retire these puzzle types (or retheme)
- The old `CFG_SCENARIOS` map (9 keys) → replace with 3 per-axis scenario banks
- The SORT puzzle ordering by complexity → still valid, just uses 20 IDs
- The MATCH puzzle (config label ↔ scenario) → keep, adapt to new IDs

---

## Visual (ConfigScenario) Changes

The SVG renderer should encode the 3 axes visually and systematically:

| Axis | Visual encoding |
|---|---|
| **Plexity** | Number of circles rendered: 1 / 2 / 6 |
| **Similarity** | Circle appearance: all same color (S) / different shapes (D) / blurred/grey (F) |
| **Separability** | Spacing: far apart (S) / touching (C) / overlapping/merged (F) |

```javascript
function ConfigScenario({configId, size=100}) {
  const id = configId || 'UPX';
  const P = id[0];         // U D M
  const Sim = id[1] || 'S'; // S D F
  const Sep = id[2] || 'S'; // S C F

  const count = P === 'U' ? 1 : P === 'D' ? 2 : 6;

  // Spacing: Separate=far, Connected=touching, Fused=overlapping
  const gap = Sep === 'S' ? 28 : Sep === 'C' ? 18 : 6;

  // Color: Similar=monochrome green, Dissimilar=mixed colors, Fuzzy=grey
  const colors = Sim === 'S'
    ? Array(count).fill('#16a34a')
    : Sim === 'D'
      ? ['#16a34a','#0369a1','#7c2d12','#0f766e','#78350f','#134e4a']
      : Array(count).fill('#71717a');

  // Opacity: Fused = high overlap + partial transparency
  const opacity = Sep === 'F' ? 0.55 : 0.85;

  // Layout: arrange in a row or circle
  const positions = computePositions(count, gap); // helper below

  return (
    <svg width={size} height={size} viewBox="0 0 100 100">
      <rect width={100} height={100} fill="var(--surface)" rx={6}/>
      {positions.map((pos, i) => (
        <circle key={i} cx={pos.x} cy={pos.y} r={12}
          fill={colors[i % colors.length] + (Math.round(opacity * 255).toString(16))}
          stroke={colors[i % colors.length]}
          strokeWidth={1.5}
        />
      ))}
    </svg>
  );
}

function computePositions(count, gap) {
  if (count === 1) return [{x:50, y:50}];
  if (count === 2) return [{x:50-gap/2, y:50}, {x:50+gap/2, y:50}];
  // 6 items: arrange in 2 rows of 3
  const cols = 3, rows = 2;
  const startX = 50 - (cols-1)*gap/2;
  const startY = 50 - (rows-1)*gap/2;
  return Array.from({length:count}, (_,i) => ({
    x: startX + (i % cols) * gap,
    y: startY + Math.floor(i / cols) * gap
  }));
}
```

---

## Ca Morphology Table (canonical 20 × 4)

Keep this exactly as derived from zsnout `generate/ca/affiliation.ts`.
The `IK_CFG` identity map should map all 20 IDs to themselves:

```javascript
const IK_CFG = {
  UPX:"UPX", DPX:"DPX",
  DSS:"DSS", DSC:"DSC", DSF:"DSF",
  DDS:"DDS", DDC:"DDC", DDF:"DDF",
  DFS:"DFS", DFC:"DFC", DFF:"DFF",
  MSS:"MSS", MSC:"MSC", MSF:"MSF",
  MDS:"MDS", MDC:"MDC", MDF:"MDF",
  MFS:"MFS", MFC:"MFC", MFF:"MFF",
};

const CA_TABLE = {
  UPX:{CSL:"",    ASO:"l",   VAR:"ř",   COA:"r"  },
  DPX:{CSL:"s",   ASO:"sl",  VAR:"sř",  COA:"sr" },
  DSS:{CSL:"c",   ASO:"cl",  VAR:"cř",  COA:"cr" },
  DSC:{CSL:"ks",  ASO:"ksl", VAR:"ksř", COA:"ksr"},
  DSF:{CSL:"ps",  ASO:"psl", VAR:"psř", COA:"psr"},
  DDS:{CSL:"ţs",  ASO:"ţsl", VAR:"ţsř", COA:"ţsr"},
  DDC:{CSL:"fs",  ASO:"fsl", VAR:"fsř", COA:"fsr"},
  DDF:{CSL:"š",   ASO:"šl",  VAR:"šř",  COA:"šr" },
  DFS:{CSL:"č",   ASO:"čl",  VAR:"čř",  COA:"čr" },
  DFC:{CSL:"kš",  ASO:"kšl", VAR:"kšř", COA:"kšr"},
  DFF:{CSL:"pš",  ASO:"pšl", VAR:"pšř", COA:"pšr"},
  MSS:{CSL:"t",   ASO:"tl",  VAR:"tř",  COA:"tr" },
  MSC:{CSL:"k",   ASO:"kl",  VAR:"kř",  COA:"kr" },
  MSF:{CSL:"p",   ASO:"pl",  VAR:"př",  COA:"pr" },
  MDS:{CSL:"ţ",   ASO:"ţl",  VAR:"ţř",  COA:"ţr" },
  MDC:{CSL:"f",   ASO:"fl",  VAR:"fř",  COA:"fr" },
  MDF:{CSL:"ç",   ASO:"çl",  VAR:"çř",  COA:"çr" },
  MFS:{CSL:"z",   ASO:"zl",  VAR:"zř",  COA:"zr" },
  MFC:{CSL:"ž",   ASO:"žl",  VAR:"žř",  COA:"žr" },
  MFF:{CSL:"ż",   ASO:"żl",  VAR:"żř",  COA:"żr" },
};
```

---

## Synesthetic Palette

Since the axes are now independent, each gets its own color identity:

| Axis | Color family | Rationale |
|---|---|---|
| Plexity | Green spectrum (light→mid→dark) | Growing density |
| Similarity | Hue shift (green→blue→grey) | Conceptual distance between items |
| Separability | Value shift (bright→dark) | Physical closeness collapsing |

TONE_MAP for the combined Ca IDs: map by axis combination rather than arbitrary assignment.
Plexity controls octave (U=C4, D=C5, M=C6), Similarity controls interval offset,
Separability controls fine-tuning within the note.

---

## Chapter Integration

| Chapter | What to teach |
|---|---|
| Ch.03 intro | Plexity only — U/D/M, count dimension |
| Ch.03 mid | Similarity — S/D/F, sameness dimension |
| Ch.03 end | Separability — S/C/F, contact dimension |
| Ch.03 synthesis | Full 3-axis combos — build Ca IDs, use in formatives |
| Ch.08+ | Advanced: Ca morphology table, all 20 forms in context |

---

## Files to Change When Implementing

| Location | What changes |
|---|---|
| `PARAMS.configuration.values` | Replace 9-value flat list with 3-axis system (or split into 3 params) |
| `TONE_MAP` | Remap config IDs from 9 → 20, axis-based frequency logic |
| `CFG_ORDER` | Update to 20 canonical IDs in logical order |
| `CFG_SCENARIOS` | Replace with 3 per-axis scenario banks |
| `IK_CFG` | Identity map for all 20 IDs |
| `CA_TABLE` (or `CA_MORPHOLOGY`) | 20×4 canonical table |
| `ConfigScenario` SVG | Systematic 3-axis renderer (see above) |
| Puzzle generators | New axis-by-axis modules + updated combo puzzles |
| `buildForm()` | Ensure it reads from `IK_CFG` / `CA_TABLE` using 20-ID system |
| `defaultP()` | Default config value = `UPX` |
| Any hardcoded old IDs | Sweep for DCT/AGG/SEG/CPN/COH/CST/MUL in puzzle targets/accepts |

---

## What NOT to Do

- Don't add 20 separate PARAMS entries — the axes are the unit of teaching
- Don't expose `IK_CFG` mapping in the UI — it's an internal generation detail
- Don't make Similarity/Separability visible when Plexity = U (irrelevant axes)
- Don't lose the old `CFG_SCENARIOS` text — repurpose it into the 3 axis scenario banks

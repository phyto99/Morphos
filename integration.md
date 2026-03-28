MORPHOS v3 — IDE Integration Framework
What This Document Is
MORPHOS v3 is a complete single-file Ithkuil IV learning system (morphos3.html, ~5,000 lines). Everything works in-browser with placeholder morphology. This document specifies exactly what the IDE session needs to do to upgrade it to production quality with real data.

Architecture Overview
morphos3.html
├── <script type="text/babel">          ← All React/JS (Layers 1–19)
│   ├── Layer 1:  Audio engine (Web Audio API, synesthetic tones)
│   ├── Layer 2:  Linguistic data (PARAMS, ROOTS, CASES, MODULES)
│   ├── Layer 3:  Learner model + SM-2 SRS + zsnout mapping + sentence exercises
│   ├── Layer 4:  UI primitives (SynCell, ParamStrip, SlotBlock, Meter…)
│   ├── Layer 5:  Animated SVG scenarios (ConfigScenario, ExtensionScenario…)
│   ├── Layer 6:  Morpheme Architecture Visualizer (MAV)
│   ├── Layer 7:  Concept Decomposition Workspace (CDW)
│   ├── Layer 8:  All puzzle engines + 113-puzzle registry + PuzzleHall
│   ├── Layer 9:  Word Deconstruction Engine (WDE)
│   ├── Layer 10: Abstraction Ladder
│   ├── Layer 11: Root Family Tree (SemanticGraph)
│   ├── Layer 12: Perspective Sorter
│   ├── Layer 13: Validation Chain
│   ├── Layer 14: Form Dissection (DISSECT_FORMS)
│   ├── Layer 15: Morphological Sandbox (buildForm + MAV + gloss)
│   ├── Layer 16: Dashboard + FutureFeatures
│   ├── Layer 17: Phonology reference, Live root hydration
│   ├── Layer 18: Settings (theme sliders, SRS, audio)
│   └── Layer 19: App shell (routing, nav, topbar, SRS, LM)
└── <script type="module">              ← ESM zsnout loader (Layer 20)
    └── Loads @zsnout/ithkuil from esm.sh
        Exposes window.IK = {gen, parse, data, gloss, rootIndex, loaded}
        Dispatches 'ithkuil-loaded' CustomEvent

Integration Priority 1: Root Library (CRITICAL)
Current State
jsconst ROOTS = [/* 20 hand-written entries */];
What zsnout Provides (AUTO)
When the ESM module loads, window.IK.rootIndex contains the full root database indexed by Cr string. The hydration hook already runs automatically:
js// Already in morphos3.html — Layer 17C / App useEffect
ROOTS.forEach(r => {
  const live = window.IK.rootIndex[cr.toLowerCase()];
  if (live && !r._hydrated) {
    r.stem1 = live.stems?.[0]?.BSC || r.stem1;
    r.stem2 = live.stems?.[1]?.BSC || r.stem2;
    r.stem3 = live.stems?.[2]?.BSC || r.stem3;
    r.desc  = live.refers || r.desc;
    r._hydrated = true;
  }
});
What IDE Needs to Do
Replace the 20-entry ROOTS array with ~200–500 carefully chosen seed entries covering all 9 domains, formatted as:
js{
  id: "KŢ",           // MORPHOS internal ID (Cr string with diacritics)
  domain: "II",        // Domain I–IX
  name: "Cognition",   // Human-readable name
  color: "#c2410c",    // Domain color (see DOMAIN_META)
  stem1: "...",        // Will be overwritten by zsnout hydration
  stem2: "...",
  stem3: "...",
  desc: "...",
  crosslinks: ["MŘ", "ŇV"]  // Semantically adjacent roots
}
Strategy: Use zsnout's data.roots to pull the full list, pick 200–500 diverse entries across domains, write them as static seed data. The live hydration will then overwrite stems with real content on load.

Integration Priority 2: Affix Database (HIGH)
Current State
jsconst AFFIXES = [/* 8 invented placeholder entries */];
What's Needed
Ithkuil IV has hundreds of affixes in 3 types with 9 degrees each. The system needs a real database.
Data Shape
jsconst AFFIXES = [
  {
    id: "INT",
    label: "Intensity Modifier",
    type: 1,          // Type 1 (Vx+Cs format), 2, or 3
    cs: "st",         // Consonantal form
    desc: "Modifies degree or intensity of the root's property",
    degrees: [
      "minimally, barely perceptible",
      "slightly, noticeable",
      "somewhat, clearly present",
      "moderately",
      "substantially",
      "strongly",
      "very strongly",
      "near-maximally",
      "maximally, fully realized"
    ]
  },
  // ...
];
How It Plugs Into Existing Puzzles
All affix puzzles in the registry (affix_degree, affix_fn, affix_stack) sample from AFFIXES. Replace the array and they automatically use real data. No puzzle code changes needed.
Suggested IDE Approach
Pull from zsnout's affix data if available, or transcribe from the Ithkuil IV spec PDF (Appendix II). Start with 50–100 of the most common affixes.

Integration Priority 3: Real Morphological Generation (ALREADY HANDLED)
Current State
buildFormLive() tries window.IK.gen.formativeToIthkuil() and falls back to buildForm() (the full spec-accurate placeholder).
Mapping Already Written
IK_CFG, IK_EXT, IK_MOOD, IK_VLD, IK_ASP, IK_ROOT — all in Layer 3B. These map MORPHOS parameter IDs to zsnout API values.
What IDE May Need to Adjust
The zsnout formativeToIthkuil() API shape should be verified against the actual package. Current assumption:
jswindow.IK.gen.formativeToIthkuil({
  root: "kţ",
  stem: 1,
  specification: "BSC",
  function: "STA",
  ca: {
    configuration: "UPX",  // zsnout uses UPX not UNI
    affiliation: "CSL",
    perspective: "M",
    extension: "DEL",
    essence: "NRM",
  }
})
If the actual API differs, update buildFormLive() in Layer 3B.

Integration Priority 4: Parse + Gloss (ALREADY HANDLED)
glossForm(params) already calls:
jswindow.IK.parse.parseWord(form)
window.IK.gloss.glossWord(parsed)
Result shown in Sandbox below the assembled form. No changes needed if zsnout exports these.

Integration Priority 5: Audio Phoneme Files (MEDIUM)
Current State
Pronunciation uses window.speechSynthesis (Web Speech API). Works but quality is poor for unusual phonemes like ř, ļ, ħ.
What's Needed
A set of audio files (/audio/phonemes/) for the 34 consonants and 16 vowels. Each should be a short WAV or OGG of the isolated phoneme.
Integration Point
In Phonology component, speak() function:
jsconst speak = (text) => {
  // Replace with:
  const audio = new Audio(`/audio/phonemes/${encodeURIComponent(text)}.ogg`);
  audio.play().catch(() => {
    // Fallback to speechSynthesis
    const u = new SpeechSynthesisUtterance(text);
    window.speechSynthesis.speak(u);
  });
};

Integration Priority 6: Data Persistence (MEDIUM)
Current State
All learner model (lm) state is in-memory React state. Lost on page reload.
Simple Solution
js// In App component, after useState(initLM()):
const [lm, setLm] = useState(() => {
  try {
    const saved = localStorage.getItem('morphos_lm');
    return saved ? { ...initLM(), ...JSON.parse(saved) } : initLM();
  } catch { return initLM(); }
});

// Persist on every lm change:
useEffect(() => {
  localStorage.setItem('morphos_lm', JSON.stringify(lm));
}, [lm]);
Full Solution (Backend)
POST /api/lm with lm state on change. GET /api/lm on load. Allows cross-device sync.

What Is Complete and Needs No IDE Work
FeatureStatusFull Ca allomorph table (CA_BASE, CA_PER, CA_EXT, CA_ESS)✅ CompleteVr pattern table (all 9 stem×pattern combos)✅ CompleteFull Vc case vowel table (48+ cases)✅ Complete113 puzzle definitions across 15 categories✅ Complete8 puzzle engines (MCQ, MATCH, SORT, SPEED, BUILD, MULTI, VISUAL, INPUT)✅ CompleteSENTENCE engine with 7 multi-clause exercises✅ CompleteSM-2 spaced repetition (srsUpdate, srsDue, srsWeakest)✅ CompleteAdaptive drill routing (adaptiveDrills, PARAM_DRILLS)✅ CompleteDifficulty tiering in MCQ (mastery-dependent option count)✅ CompleteModule progression tracking + prereq gating✅ Complete▶ Next recommended puzzle in topbar✅ CompletePhonology module (34 consonants, 16 vowels, 10 rules, IPA grid)✅ CompleteRegister-in-context puzzles (10 samples, DSV/PTH/MAT/REF/ALG)✅ CompleteError correction without hints (error_find, error_identify_value)✅ CompleteBlank-canvas encoding (blank_canvas BUILD puzzle)✅ Completezsnout live engine bridge (window.IK, 'ithkuil-loaded' event)✅ CompleteRoot hydration from window.IK.rootIndex✅ CompleteLive gloss display in Sandbox✅ CompleteTheme system (Lichess/Discord/Pure slider + brightness slider)✅ CompleteSynesthetic audio engine (Web Audio API, TONE_MAP)✅ CompleteMorpheme Architecture Visualizer (MAV)✅ Complete

Build System Upgrade (Optional)
If converting to a proper React app:
bashnpx create-react-app morphos --template typescript
# or
npm create vite@latest morphos -- --template react-ts
Move each Layer into its own module:

src/data/ — PARAMS, ROOTS, CASES, MODULES, AFFIXES
src/audio/ — synesthetic engine
src/engines/ — MCQEngine, MatchEngine, etc.
src/puzzles/ — PUZZLE_REGISTRY
src/hooks/ — useIthkuil, useSRS, useLearnerModel
src/components/ — MAV, ParamStrip, Phonology, Sandbox…
src/views/ — PuzzleHall, Dashboard, Settings…

The window.IK bridge becomes a proper React context wrapping @zsnout/ithkuil imports.

Quick Integration Checklist
□ 1. Replace ROOTS array with 200–500 seed entries from zsnout data
□ 2. Replace AFFIXES with real Type 1/2/3 entries from spec
□ 3. Verify buildFormLive() API shape matches installed zsnout version
□ 4. Add localStorage persistence for lm state
□ 5. (Optional) Add /audio/phonemes/ files for 34 consonants + 16 vowels
□ 6. (Optional) Add backend for cross-device LM sync
□ 7. (Optional) Bundle as Vite/CRA app for production

Testing the Integration
After plugging in real data, run these checks:

Root hydration: Open Puzzle Hall → Adaptive → should show real root names in weak-spot list
Live gloss: Open Sandbox → assemble any form → gloss line should appear below the word
Live indicator: Topbar should show ● live within ~2–3 seconds of page load
Root puzzles: root_match, stem_id, root_infer should now show real stem descriptions
Affix puzzles: affix_degree, affix_fn should show real affix names and descriptions
Form accuracy: buildFormLive() output should match zsnout's canonical romanization


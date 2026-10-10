# YourPower — Simulation Engine Research

*Research completed Oct 8, 2026. This is the biggest technical decision in the project.*

## Options compared

| Engine | Accuracy | Performance | License | Mobile feasibility |
|---|---|---|---|---|
| **ngspice** (C, SPICE kernel) | Gold standard — transient, AC, nonlinear, real device models | Fast on-device (C, decades of optimization) | **BSD (permissive)** — safe commercially | No official mobile build; compile C for arm64 + FFI bindings. Nontrivial but doable. |
| **ngspice → WebAssembly** (eecircuit-engine) | Full ngspice | Near-native; long runs in a Web Worker | BSD terms apply | Runs in any WebView — usable via bridge. See WebSpice. |
| **spicey** (pure TypeScript SPICE) | Teaching-grade — R/L/C, diodes, controlled sources; **no transistors yet** | ~15 KB, instant, zero deps | **MIT** | Trivial to embed anywhere JS runs |
| **CircuitJS1 (Falstad)** | Good for linear teaching circuits; simplified transistor models | Real-time, tuned for education | **GPL-2.0 (copyleft)** — avoid embedding in a commercial app | Embedding triggers share-alike; iframe of hosted build is the safe route |
| **Custom MNA solver** | As good as your models — fine for DC + linear + basic transients | Instant for <100 nodes; dense LU suffices | Yours | Easiest integration |

Skipped: Lcapy (Python symbolic, server-side only, no mobile story).

## Recommendation: hybrid engine strategy

1. **Start with a custom MNA solver** for the game loop. DC, linear components, and simple capacitor/inductor transients (backward Euler) cover ~80% of a Circuit Analysis I course. Deterministic sub-millisecond solves, and level validation is trivial — you grade against *your own* solver, no ambiguity.
2. **Graduate to ngspice (via WASM)** for advanced chapters (transistors, op-amps, AC sweeps) where real device models matter. BSD license is the safest full-power option.
3. **Prototype on web first** with spicey (MIT) to validate the game loop before writing native code.

Key fact making custom feasible: every element is just a matrix "stamp" added to a conductance matrix by inspection — resistors, sources, dependent sources all reduce to stamp patterns taught in undergrad courses.

## Practical architecture

Flutter app → sim core (Dart first, later C/Rust via FFI shared across iOS/Android) → sim loop on an isolate/worker → Flutter canvas rendering.

## Sources
- ngspice manual/license: http://www.mirrorservice.org/sites/distfiles.macports.org/ngspice/ngspice-33-manual.pdf
- WebSpice (ngspice WASM): https://github.com/jwt625/WebSpice
- spicey (MIT): https://github.com/tscircuit/spicey
- CircuitJS GPL embedding analysis: https://github.com/srivtx/ee-curriculum/blob/HEAD/research/EE_INTERACTIVE_FEATURES_RECOMMENDATION.md
- Custom MNA engine reference: https://github.com/sergeyfarin/electric-circuit-modeling
- MNA theory (UBC notes): https://people.ece.ubc.ca/shahriar/eece251_notes/eece251_set2_1up.pdf

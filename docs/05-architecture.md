# YourPower — Architecture Document

*The contract Cursor builds from. Keep it current: when a decision changes, update this doc first.*

## 1. Tech stack

| Layer | Choice | Why |
|---|---|---|
| App framework | **Flutter (Dart)** | One codebase iOS+Android; `CustomPainter`/Impeller gives full control of the schematic canvas at 60–120fps; Dart feels familiar from C++ |
| Sim core (v1) | **Custom MNA solver in Dart** | DC + linear + basic transients cover Chapters 1–3; deterministic, testable; level validation grades against our own solver |
| Sim core (later) | C/Rust core via Dart FFI, then ngspice WASM for advanced chapters | Shared native core across platforms; BSD-licensed ngspice is the safe full-power option |
| State management | **Riverpod** | Standard, testable, scales from MVP to leagues/multiplayer |
| Local persistence | **drift (SQLite)** | Player progress, levels, attempts, sandbox saves — relational fits |
| Backend (v1) | **None** — levels bundled, all local | No login needed to play |
| Backend (later) | Supabase | Leaderboards, leagues, community puzzle sharing, cloud save |
| Charts/plots | Custom painters (Bode, waveforms) | Full stylistic control; fl_chart as fallback for simple plots |

## 2. Module map

```
lib/
├── sim/                  # The engine. Pure Dart, zero Flutter imports (unit-testable)
│   ├── netlist.dart      # Component, Node, Netlist models
│   ├── stamper.dart      # MNA stamps: R, L, C, V/I sources, dependent sources
│   ├── solver.dart       # Dense LU solve; DC operating point
│   ├── transient.dart    # Backward-Euler time stepping
│   └── analysis.dart     # AnalysisResult: node voltages, branch currents, waveforms
├── canvas/               # Schematic editor UI
│   ├── schematic_painter.dart   # CustomPainter: components, wires, current-flow animation
│   ├── interaction.dart        # Drag, drop, wire, select, pan/zoom gestures
│   └── palette.dart            # Component palette (gated by unlocks)
├── game/                 # The game layer
│   ├── level.dart        # Level definitions: objective, constraints, par
│   ├── validator.dart    # Runs sim, checks objective within tolerance
│   ├── scoring.dart      # Zachtronics scoring: parts count, power, settling time
│   └── progression.dart  # XP, streaks, leagues, unlocks
├── curriculum/           # Chapters, lessons, unlock graph (data-driven, JSON)
├── ui/                   # Screens: home, campaign map, challenge, sandbox, results, profile
└── services/             # Persistence, notifications, audio/haptics, analytics
```

**Rule:** `sim/` never imports Flutter. The game grades against the sim — the sim is the referee.

## 3. Data models

```dart
Component { id, type (R/L/C/Vsrc/Isrc/Switch/LED/...), value, unit, nodeA, nodeB, params }
Node      { id, x, y }                      // schematic position; solver assigns indices
Netlist   { components, nodes, groundNode }
AnalysisResult { nodeVoltages: Map<nodeId, double>, branchCurrents: Map<compId, double>,
                 waveform: List<TimePoint>?, converged: bool }
Level     { id, chapterId, title, brief, objective: Objective, constraints, par, hints[] }
Objective { kind (targetVoltage | maximizePower | timing | ...), targetNode, targetValue,
            tolerance, metricWeights }
Attempt   { levelId, netlist, result, scores {parts, power, settling}, stars, durationMs }
PlayerProgress { xp, level, streak, longestStreak, freezes, unlockedComponents[], starsPerLevel{} }
```

## 4. Key flows

**Build → Simulate → Verify (the core loop)**
1. Player drags components from palette onto canvas, wires nodes.
2. Taps RUN → `validator` builds `Netlist` → `solver`/`transient` on a background isolate → `AnalysisResult`.
3. Canvas animates current flow + probe values from the result (the magic moment).
4. For challenges: `validator` checks `Objective` within tolerance → pass/fail → `scoring` computes parts/power/settling percentiles → results screen.

**Level validation (authoring-time):** every shipped level must be solvable — CI runs a reference solution through `validator`. If the reference fails, the level doesn't ship.

**Progression:** completing a level awards XP + stars (1 star pass, up to 3 for optimization percentiles); stars gate chapters; components unlock into the sandbox palette.

## 5. API contracts

- v1: no backend, no API. Curriculum ships as versioned JSON assets.
- Later (Supabase): `POST /attempts` (levelId, netlist hash, scores), `GET /leaderboards/{levelId|daily}`, `GET /puzzles/community`. Netlists are the interchange format — hashable, diffable, shareable.

## 6. Coding conventions

- `sim/` is pure Dart with 100% unit-test coverage on stamps and solver (compare against known textbook circuits).
- All sim math in doubles; tolerances explicit (`1e-9`), never magic numbers.
- Feature-first UI organization; Riverpod providers per feature, no global mutable state.
- Every user-visible string in localization files from day one.

## 7. Non-goals (v1)

- No transistors, op-amps, or digital logic (Chapters 1–3: DC → KCL/KVL → Thévenin).
- No multiplayer, no community sharing, no backend.
- No AC phasor/Bode chapters yet (engine supports the path; content comes in v2).

## 8. Build order

1. MNA core (DC) + Flutter canvas + Chapters 1–3 content.
2. Transient sim + Chapters 4–6 (RC/RL, timing, intro AC).
3. Zachtronics scoring + streaks/leagues + sandbox unlocks.
4. ngspice-backed chapters (op-amps, filters, digital).
5. Community puzzle sharing + daily challenges.

## Open technical questions

- Exact tolerance defaults per objective kind (tune in playtesting).
- Isolate protocol for sim loop (keep UI at 60fps during long transients).
- Reference-solution authoring tooling (internal level editor vs hand-written JSON).

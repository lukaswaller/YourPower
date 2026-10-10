# AGENTS.md — YourPower

## What this is
YourPower is a mobile game that teaches electrical engineering through interactive circuit building and real simulation: drag-and-drop components, a custom MNA (modified nodal analysis) solver, campaign chapters mapped to Circuit Analysis I, and Zachtronics-style optimization scoring (parts count / power / settling time).

## Stack (exact — do not substitute)
- Flutter (Dart), single codebase for iOS + Android
- Sim core v1: custom MNA solver in pure Dart (DC + linear + backward-Euler transients)
- Sim core later: C/Rust via FFI, then ngspice WASM (BSD) for advanced chapters
- State: Riverpod · Persistence: drift (SQLite)
- Backend v1: none (levels bundled); later Supabase for leaderboards/community

## Docs (read before structural work)
- docs/01-vision-and-features.md — product spec / PRD
- docs/05-architecture.md — THE contract: modules, data models, flows, build order
- docs/06-ui-ux.md — screens, core loop (predict, build, simulate, verify)
- docs/02-04 — research behind the decisions

## Commands
- flutter pub get
- flutter run
- flutter test
- flutter analyze

## Do not
- Never embed CircuitJS (GPL-2.0). ngspice is BSD — that is the safe full-power option.
- lib/sim/ must never import Flutter — the sim is the referee; the game grades against it.
- Every shipped level must be solvable: CI runs a reference solution through the validator.
- No new dependencies without asking.

## How to work
- Plan before coding: explain the approach, wait for approval on anything structural.
- One micro-task at a time; sim stamps/solver need unit tests against known textbook circuits.
- Update docs/05-architecture.md when a decision changes.

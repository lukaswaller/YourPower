# YourPower — UI and UX Plan

## Design language

- **"Clean lab" aesthetic:** dark canvas (deep slate/navy) with glowing traces — circuits look alive. UI chrome light and minimal so the schematic is the hero.
- **Vector-clean components**, IEEE-style symbols. No cartoon clip-art — this is a *real* tool that happens to be a game.
- **Colorblind-safe palette** for current-flow animation (use motion + brightness, not just red/green).
- Typography: a technical mono for values (`5.00 V`), friendly rounded sans for game UI.

## Screens

1. **Home** — streak flame, XP bar, daily quest card, Continue button (deep-links to current level), tabs: Campaign / Sandbox / Profile.
2. **Campaign map** — chapter worlds (DC Basics → … → Op-Amps) as a winding path; nodes show stars earned; locked nodes greyed with requirement ("Earn 12★ in Chapter 2").
3. **Lesson intro (60 seconds)** — interactive, *inside* the sandbox: "Drag a resistor here. See the voltage divide?" Teach by doing, then the challenges test it. No walls of text.
4. **Challenge screen** — the core screen:
   - Top: objective card ("Make V(out) = 5.00V ±1%") with live readout of the probed value.
   - Middle: the canvas — palette on the left/bottom, RUN button prominent.
   - Tapping RUN animates current flow (the magic moment), then pass/fail.
5. **Results screen** — pass animation, XP gained, then the Zachtronics panel: your solution scored on parts count / power / settling time vs. histogram of all players, with percentile ranks. "Optimize" button re-enters the level.
6. **Sandbox** — free build with all unlocked components, save/load slots, probe anywhere, oscilloscope strip at the bottom.
7. **Profile** — stats, achievements, league standing, settings.

## Key interactions

- **Predict → Build → Simulate → Verify:** before first RUN on a level, ask the player to *predict* (tap the expected value). Makes the simulation result land harder.
- **Probing:** tap any wire/node to pin a live probe pill showing V/I. Probes persist while editing — watch values update as you drag component values (EveryCircuit-style live tuning).
- **Component values:** tap a component → bottom sheet with slider + precise entry. Sliders make "tune to 5V" tactile.
- **Debug levels:** the circuit ships broken; player taps the suspect component to "inspect", fixes it. Keep a "rewire" affordance obvious.
- **Haptics:** satisfying click on component place, success chord on pass, subtle tick as current animates.

## Onboarding (first 60 seconds)

No tutorial screens. Level 1: a battery, one resistor slot, an LED — "Drag the resistor in. Press RUN." LED lights, confetti-ish current pulse, +50 XP. The player has *built a circuit* in under a minute.

## Accessibility & polish

- Dynamic text sizing respected in UI chrome (canvas stays fixed-scale with pinch zoom).
- Reduce-motion setting disables current-flow animation (static arrows instead).
- All objectives also stated as text, not color-only.

## Later UX (v2+)

- Multiplayer race mode (same puzzle, live ghost of opponent's progress).
- Community puzzle browser with ratings.
- "Exam mode" — timed sets styled like real course midterms.

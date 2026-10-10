# YourPower — Vision and Feature List

**Working title:** YourPower
**One-liner:** LTspice meets a puzzle game — learn all of electrical engineering by playing.

## The idea

Reading about circuits is boring. *Building* circuits is fun. This app is a circuit sandbox + game:

- **Build** circuits drag-and-drop style (like LTspice / EveryCircuit) and **simulate** them live — see voltages, currents, waveforms.
- **Play** structured challenges: "make this node exactly 5V", "light the LED with the fewest components", "find and fix the broken circuit".
- **Progress** through a full EE curriculum as game levels — DC basics all the way through AC, op-amps, filters, and digital.
- **Get rewarded** — XP, streaks, unlocks, ranks. Learning you can feel.

The thesis: most of EE is genuinely fun to *do*; textbooks just hide the fun. This app removes the hiding.

## Who it's for

- EE/CE students (like Lukas) who want practice that doesn't feel like homework.
- Hobbyists and Arduino/tinkerer types who want intuition, not lectures.
- Anyone studying for circuits exams who'd rather play than re-read notes.

## Core features (MVP)

1. **Circuit sandbox** — drag-and-drop components (R, L, C, sources, switches, LEDs), wire them up, run DC simulation, probe any node/branch. Live, fast, forgiving.
2. **Real simulation engine** — actual nodal analysis under the hood (not fake animations), so skills transfer to real tools.
3. **Challenge mode** — the game loop:
   - *Target challenges:* "Get V(out) = 5V ±1%" — pick/change component values to hit it.
   - *Efficiency challenges:* solve with fewest components / lowest power.
   - *Debug challenges:* "this circuit is broken — find the fault."
   - *Speed challenges:* timed builds.
4. **Campaign progression** — levels mapped to a real Circuit Analysis I syllabus: Ohm's law → KCL/KVL → nodal/mesh → Thevenin → capacitors/inductors → AC/phasors → op-amps → filters. Each concept taught *by doing*.
5. **Rewards** — XP, levels, streaks, component unlocks, solution ratings (★ for elegance), leaderboards for daily challenges.
6. **Learn-by-doing lessons** — 60-second interactive explainers *inside* the sandbox, right before you use the concept. No walls of text.

## Later phases

- AC/transient simulation with oscilloscope views and Bode plots.
- Op-amp, diode (nonlinear), and digital logic levels.
- Multiplayer: race a friend on the same challenge; share circuit puzzles.
- "Exam mode" — timed problem sets mirroring real course midterms.
- Community puzzle sharing.

## What makes it stand out

- It's a **real simulator**, not a cartoon — skills transfer to LTspice and coursework.
- Game structure (campaign + challenges + rewards) where competitors are either dry simulators (EveryCircuit) or non-interactive content (textbooks, Brilliant).
- Built by an EE student, for EE students — curriculum-mapped from day one.

## Monetization (draft)

- Free: full sandbox + first campaign chapters.
- **Pro/season pass:** later chapters, unlimited hints, advanced sim (AC/transient), multiplayer, custom puzzle builder. Education apps monetize well on subscriptions (Duolingo model).

## Open questions

- Exact name + art style (clean vector? playful?).
- Which simulation engine? (Biggest technical decision — see Research.)
- Mobile-first; desktop/web later?
- How is difficulty tuned so it stays fun, not frustrating?

## Status

📝 Planning — vision drafted, research in progress.

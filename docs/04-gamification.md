# YourPower — Gamification Research

*Research completed Oct 8, 2026.*

## The two pillars

### 1. Duolingo stack — retention mechanics
- **XP, streaks, leagues, quests.** Streaks are the strongest retention lever (~10M users with 1-year+ streaks). Leagues add ~17% learning time and 3× highly-engaged users.
- **Streak freezes + customizable daily goals** (not guilt) drive the compounding effect — with freeze access, users maintain streaks 4.5× longer by day 21.
- **Adopt:** streaks + freezes + weekly leagues (~30-player cohorts).
- **Reject hearts/lives** — punishing mistakes kills the experimentation a circuit game needs. Wrong answers must be *interesting*, not costly.

### 2. Zachtronics pattern — the scoring model (Opus Magnum, Shenzhen I/O)
The single best model for this game: open-ended engineering puzzles where *any working solution* passes, then the player is scored on competing metrics against **histograms of all players' solutions**. The tradeoff triangle (faster ⇔ more parts ⇔ bigger) makes perfection unattainable and optimization self-motivating — "arms races with friends" drive hours of replay.

**Mapped to circuits:** score every solution on **component count vs. power efficiency vs. settling time / parts cost**. Show the player's percentile on each axis after every solve.

## Structure: campaign + sandbox

- Circuit Jam's validated structure: **puzzles unlock sandbox components** — gate new parts behind mastery, then let players build freely.
- Add a **puzzle editor + community sharing** (Steam Workshop did this for Opus Magnum) for long-tail content.
- **Daily challenges** with leaderboards for the competitive layer.

## Reward loop

- Micro-wins per level: XP + celebratory feedback (confetti done *well* — the EveryCircuit current-flow animation is the "magic moment" equivalent).
- Component unlocks as the "collection" hook.
- Daily quests ("solve a filter challenge").
- Streak-saver notifications only when a streak is genuinely at risk.

## Challenge archetypes (per curriculum chapter)

1. **Target-value** — "make node X read exactly 5V" (DC basics, dividers).
2. **Diagnosis** — "this circuit is broken — find the miswired component" (KCL/KVL, nodal/mesh).
3. **Reverse-engineering** — match a mystery black box's behavior with your own equivalent circuit (Thévenin/Norton).
4. **Timing** — "light the LED exactly 2s after the switch flips" (RC transients).
5. **Maximization** — "maximize power delivered to the load" (maximum power transfer, AC).
6. **Filter shaping** — "attenuate 60Hz by 40dB while passing 1kHz" with live Bode plot (frequency response).
7. **Optimization** — fewest components for a target op-amp gain (Zachtronics scoring).

## Core loop for every level

**Predict → Build → Simulate → Verify.** Show the target, let the player *predict* before running, then animate the result. Tie boss levels to real course milestones (e.g. a midterm-prep gauntlet).

## Sources
- Duolingo gamification evidence: https://github.com/flagrare/llm-tutor/blob/HEAD/docs/research/04-gamification-evidence.md
- https://trophy.so/blog/duolingo-gamification-case-study
- Zachtronics design: https://en.wikipedia.org/wiki/Zachtronics
- https://www.pcgamer.com/perfectly-solving-opus-magnums-puzzles-is-impossible-but-thats-ok/

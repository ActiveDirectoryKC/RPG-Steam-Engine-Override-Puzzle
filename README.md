# Steam Engine Override Interface

A single-file, self-contained puzzle/password guessing minigame built for any
tabletop RPG. The goal is to provide a mechanism for the players to solve a
puzzle collectively to prevent an exploding steam engine. No build step, no
dependencies: open `steam-engine-override-puzzle.html` in a browser and it
runs.

Originally built as a train-decoupling / boiler-override encounter, but the
mechanics are generic enough to reskin for any "enter the code while reading
environmental feedback" scene.

> **[AI DISCLOSURE]** This tool was entirely AI developed via guided prompts.
> The prompt information is provided below. The inspiration is the typical
> "lever pull" minigames from video games and playing "hotter-colder" with my
> kids. I'm not a terribly great JavaScript programmer, hence my use of
> Claude to generate the code.

## General mechanics

Players work with a bank of dials, each having 16 possible values (`0-9`,
`A-F`), to find a hidden code that is set by the DM. Each time the players
make a change and submit, one attempt is consumed and they are shown whether
the system improved or got worse. The idea is the players play a game of
hotter-colder to guess the password, which in effect decides whether they're
able to save the steam engine.

Along with each "lever pull" / submission, the players were asked to roll a
DC20 INT check to assess their ability to work the system without issue.

- **Failed INT check:** Use the chart below to determine consequence. Alter
  according to your campaign.
- **Success INT check:** No harm, and the players operate the system
  successfully.
- **Natural 1 (critical failure):** The players take a negative consequence
  from the chart, and the DM also removes an attempt from the system using
  the dashboard at the top.
- **Natural 20 (critical success):** The players successfully manipulate the
  system, and the DM can give them a boon — either a clue or bonus tries — at
  their discretion.

### Failed check consequence chart

> Note: the original notes for this chart used per-gauge stat bumps (e.g.
> "Pressure +1") as the web app effect. The app has no control for nudging a
> specific gauge directly — gauges are only ever derived from the 7 dial
> positions vs. the hidden solution, and the only DM-adjustable number is the
> attempts budget (Reward 4's −1/+1 stepper, or the Decouple field). So the
> effects below were redesigned around what the app can actually do: minor
> and major failures are pure table narrative with no app interaction at all,
> and critical failures are the only tier that costs an attempt, using the
> same Reward 4 stepper described earlier. This keeps the chart consistent
> with the existing Natural 1 rule (which already docks an attempt) rather
> than introducing a second, conflicting way to touch the app.

When a player fails **any** repair check, they roll based on failure margin.
All consequences are inconvenient but survivable.

#### Minor failure (failed by 1–2)

Roll a d6. Pure narrative/mechanical flavor — no app interaction.

| Roll | Consequence | Effect |
|---|---|---|
| 1 | Slipped Tool | Character drops their tool; uses their next action to retrieve it |
| 2 | Steam Burn | Character takes 1d4 damage; minor wound (no mechanical penalty) |
| 3 | Loud Noise | Alarming metallic screech; enemies nearby hear it (if applicable) |
| 4 | Grease Splatter | Character's hands slip; disadvantage on their next check this round |
| 5 | Steam Pocket | Character coughs and sputters but recovers; wastes 6 seconds |
| 6 | Vibration Jolt | Character staggers; no mechanical effect, just awkward |

#### Major failure (failed by 3–5)

Roll a d8. Still no app interaction — costs time, position, or a temporary
penalty at the table, not attempts.

| Roll | Consequence | Effect |
|---|---|---|
| 1 | Wrench Slips | Character loses their grip; −2 to next check if retrying same task |
| 2 | Minor Fire | Small flames erupt; party takes 1d6 fire damage (no save) |
| 3 | Passenger Shout | A frightened passenger grabs the character; must use action to shake them off |
| 4 | Hissing Leak | Character's work location becomes harder to reach; next character gets disadvantage on that task |
| 5 | Character Stumbles | Character is knocked prone; uses their next action to stand up |
| 6 | Loud Bang | A component backfires; all party members within 20 ft. must make DC 12 DEX save or be startled (disadvantage on their next action) |
| 7 | Tool Damage | Character's tool breaks; they need 1 round to grab a backup or improvise |
| 8 | Steam Vent | Hot steam jets from an unexpected crack; character takes 2d4 damage and must move 5 ft. away |

#### Critical failure (failed by 6+)

Roll a d8. This is the only tier that touches the app — most results cost the
party **1 attempt** (use Reward 4's −1 button), on top of the narrative
effect. It stacks with a Natural 1's attempt loss if both happen to trigger
on the same roll.

| Roll | Consequence | Effect | Attempts Cost |
|---|---|---|---|
| 1 | Explosion (Minor) | Party takes 2d6 fire damage; character who failed takes double | −1 |
| 2 | Cascading Malfunction | The broken component creates a chain reaction; next character to work on ANY task has disadvantage | −1 |
| 3 | Passenger Injury | An unfortunate passenger is hurt in the chaos; character feels guilty (narrative tension only) | none |
| 4 | Equipment Loss | Character's toolkit is knocked overboard or becomes unreachable; they're unarmed for repairs next round | −1 |
| 5 | Fuel Spillage | Fuel leaks and pools; next character working on that section has disadvantage unless they use an action to mop it up | −1 |
| 6 | Violent Shudder | Train lurches hard; DC 12 DEX save for all party or prone | −1 |
| 7 | Broken Coupling | One train car rocks loose; passengers scream; character must use action to stabilize or lose 1 segment distance | −1 |
| 8 | Pressure Spike | System overloads momentarily; character takes 3d4 damage and the whole cabin shudders | −2 |

#### How it works at the table

1. Player fails a check → tell them the failure margin.
2. They roll the appropriate die (d6 for minor, d8 for major/critical).
3. You narrate the consequence — make it feel real but not catastrophic.
4. **Only on a critical failure:** dock the listed attempts using Reward 4's
   −1 button in the DM Control Panel. Minor and major failures never touch
   the app.
5. Play continues — they can still attempt repairs or try a different
   approach.

**Example:** Barbarian attempts to "Manually Release Pressure Valve" (DC 11),
rolls a 7 (fails by 4 = Major Failure). Barbarian rolls d8 → gets a 3
("Passenger Shout"). *Narrative: "As you wrench the valve, a terrified
passenger lurches forward and grabs your arm, shrieking 'Are we going to
die?!' You have to spend a moment reassuring them before you can work
again."* No app interaction — the Barbarian's next action is to calm the
passenger, wasting time but not costing an attempt.

> Adjust to fit your campaign — this is meant as a starting point, not a
> fixed rule.

## Quick start

1. Open `steam-engine-override-puzzle.html` in any modern browser (desktop or
   shared screen works best — there's a DM-only panel at the top that
   shouldn't be visible to players).
2. In the **DM Control Panel**, set your own override code (see below) or
   leave the default in place.
3. Share the screen with players, or drive it yourself while narrating.
4. Players call out dial moves; you turn the dials. They call out "SUBMIT"
   when ready to commit a guess.
5. Interface resolves automatically to a win or a `SYSTEM OVERLOAD` fail
   screen based on attempts remaining.

No installation, no server, no accounts. It's one HTML file — email it, drop
it in a shared folder, or host it as a GitHub Pages page and it'll work the
same way.

### Screenshot

![DM Control Panel, dials, and top of the status dashboard](screenshots/interface-top.png)

![Full status dashboard with all seven gauges and the submit log](screenshots/interface-bottom.png)

## For the DM

Everything a DM needs to steer the encounter lives in the blue **DM Control
Panel** at the top of the page. None of this is visible to players unless you
choose to share it.

### Setting the override code

- The code is 7 hex characters (`0-9`, `A-F`).
- Type it into the masked **Override Code** field and hit **Set Code &
  Restart**. Use **Show/Hide** to check what you typed without it being
  visible on a shared screen the rest of the time.
- Changing the code resets the whole interface (dials, attempts, gauges) to a
  fresh state based on the new code.

### Rewards (1–3): revealing digits

Three checkboxes let you hand players a confirmed correct digit as an
in-fiction reward (passing a skill check, finding a maintenance log, etc.).

They're fixed left-to-right: **Reward 1 reveals Position 0, Reward 2 reveals
Position 2, Reward 3 reveals Position 5.** Checking one snaps that dial to
the correct value and locks it (its ▲/▼ buttons disable) so it can't be
accidentally bumped off afterward.

For these checks I gave them an Investigation/Intelligence check to find
clues. This was intended to be hard, to encourage guessing. You can adjust
the DC accordingly.

- Clue 1: DC20 check / natural 20 on a lever pull
- Clue 2: DC22 check / natural 20 on a lever pull
- Clue 3: DC24 check / natural 20 on a lever pull

### Reward 4: attempts adjustment

A simple **−1 / +1** stepper that permanently nudges the attempts budget up
or down, applied immediately (no restart needed). Use it for "the crew buys
you time" or "that mistake cost you" moments.

### Override Decouple Warning

A checkbox + editable number field. If your table decides to decouple the
car rather than push through the override, check this box to add the field's
value (default `+4`) to the attempts remaining. It's a plain number, so you
can make it negative instead if you want the opposite tradeoff for your
scene. This applies live and can pull the interface back from a failure
state if you grant enough attempts to bring the count above zero.

### DM Reference panel

Click **Show DM Reference** (in the main dial panel) to see, per position:
the correct digit, the current dial value, which system it drives, and its
live HOT/COLD/OPTIMUM read. This is meant to stay on your screen only —
players are never shown which dial affects which gauge (see **Design intent**
below for why).

### Max Attempts

Set the starting attempts budget (default 18) in the config row and hit
**Apply & Restart**.

## For players

- **ENGINE OVERRIDE INTERFACE**: 7 dials, each a hex digit `0–F`. Turn any
  dial with its ▲/▼ buttons — this is free, unlimited, and doesn't cost
  anything.
- **Engine Status Dashboard**: 7 gauges (Pressure, Structural, Fuel,
  Vibration, Coolant, Traction, Exhaust), each 0–10. Lower is safer (green =
  0–3, yellow = 4–6, red = 7–10) — except Pressure, which only ever climbs.
- **SUBMIT**: locks in your current 7-digit guess as an official attempt.
  This is the only thing that costs you anything, and the only thing that
  updates the dashboard. If you're wrong, the game tells you how many digits
  were correct out of 7 and the gauges update to reflect your new dial
  state.
- **Win**: all 7 digits correct on a submit.
- **Lose**: attempts hit 0 before you solve it — `SYSTEM OVERLOAD`.
- **3 attempts or fewer**: a warning banner appears suggesting the crew
  consider decoupling instead of continuing to push the override.

There's no in-app hint that tells you which dial affects which gauge. That's
the puzzle.

## Design intent

**Hot/cold, not right/wrong.** Every position is scored by hex distance from
the correct digit, wrapping past `F` back to `0` (so it's a 16-point circular
scale, not a straight line). A guess that's numerically higher than correct
reads as pushing the system one direction; numerically lower pushes it the
other. Since a single gauge bar can't show two directions at once, both
"too high" and "too low" just mean "off," and the gauge rises either way —
only an exact match brings it down. This means small misses barely register
and big misses spike hard, giving players a genuine hot/cold signal without
ever telling them the actual digit.

**Gauges only move on SUBMIT.** Early versions of this tool had gauges update
live as dials turned, and separately had them update on every attempt. Both
felt wrong for different reasons: live updates made it too easy to brute-force
one dial at a time with zero cost, while submit-gated updates with no direct
correspondence made success feel arbitrary. The current version splits the
difference: dial turns are always free, but the dashboard is a snapshot taken
only at the moment of commitment — mirroring the fiction that you don't know
if a manual override took hold until you actually throw the switch.

**No stated position-to-gauge mapping.** Which of the 7 dials feeds which of
the 6 stabilization gauges is deliberately never shown to players (it's
visible only in the DM Reference panel). Since one gauge (Vibration) is fed
by two positions and the rest by one each, untangling that is itself part of
the puzzle — players have to isolate variables by testing dials and watching
which gauges respond, rather than reading a table.

**Pressure is a separate axis on purpose.** Every other gauge reflects how
close the *current dial combination* is to correct. Pressure reflects how
much of the *attempts budget* has been spent, scaled so it hits 10/10 exactly
when attempts reach zero — regardless of how good or bad the guesses were.
The idea is a ticking clock that's independent of skill: even a table that's
reading the other gauges perfectly is still racing the boiler.

**Deterministic starting state.** The dials don't start at zero or fully
random — they start at a fixed hash of whatever code the DM sets (each
starting digit is the solution digit shifted by a fixed per-position offset,
mod 16). Same code always produces the same starting dials and the same
opening gauge reading, so a DM can playtest an encounter and know it'll play
out identically at the table, without the starting values obviously mirroring
the solution itself.

**Averaged, not summed, contributions.** Where a gauge is fed by more than
one position (currently just Vibration), its value is the *average* of those
positions' individual contributions, not the sum. Summing allowed one badly
wrong position to permanently max out a gauge and mask any progress on the
other feeding it — averaging guarantees every dial change is visible on its
gauge, no matter what the other contributing dial is doing.

## Customizing further

Everything above the `<script>` tag is presentation; everything below it is
mechanics. A few constants worth knowing if you want to reskin or rebalance
this for a different encounter, all near the top of the `<script>` block:

| Constant | What it controls |
|---|---|
| `DEFAULT_SOLUTION` | The 7-char hex code used before a DM sets their own |
| `SYSTEMS` / `SYS_KEY` | The 6 stabilization gauge names (Pressure is handled separately) |
| `POSITION_SYSTEM` | Which of the 7 dial positions feeds which gauge |
| `START_SALT` | The per-position offsets used to derive starting dial values from the code |
| `OPTIMUM_DROP` / `MAX_SPIKE` | How much a correct digit lowers a gauge vs. how much a maximally-wrong digit raises it |
| `REWARD_POSITIONS` | Which 3 positions the DM's reward checkboxes reveal |

No build tooling required — edit the constants directly in the HTML file and
reload.

## How this was built

This tool was built entirely through AI-guided prompts — see
[PROMPT-INSTRUCTIONS.md](PROMPT-INSTRUCTIONS.md) for the original prompt and
the workflow used to get from idea to working puzzle.

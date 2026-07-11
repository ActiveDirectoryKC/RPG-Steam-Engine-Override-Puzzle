# Prompt Instructions & Development Process

How this tool actually got built — the prompting workflow, the initial
Claude prompt, and how it evolved from there.

> Numbers in the prompt below (15 attempts, a 16-character code, 5 systems)
> reflect where the idea started, not what shipped. After about a dozen
> back-and-forth sessions in Claude the tool settled on 7 characters, 18
> default attempts, and 7 gauges — see the main [README](README.md) for the
> current spec. This page is a record of the process, not documentation of
> the final product.

## DuckDuckGo

> I tend to use DuckDuckGo or ChatGPT to generate an initial prompt to feed
> into Claude. I don't pay for either of those and do pay for Claude Pro.
> This reduces my token usage on Claude and helps me focus the goals before
> burning through tokens generating code.
>
> DuckDuckGo is the "free tier" set to "Fast."

I'm looking for a means of simulating a faulty steam engine for a D&D game. I
was hoping for a web application but I'll settle for anything that can help
me generate the challenge. The issue is they have managed to save the train,
but now it is out of control. They need to fix it while under time
constraints. I want it to be more dynamic than "roll a die" — I would like a
visual to give them some indication without me saying yay or nay.

I'm thinking of a hotter-colder mechanic: an interactive local website that
displays the engine status and provides an interactive simulation of them
pulling levers by working dials. Each character in the password has an
effect that causes the heat, or pressure, or something to go up.

5 players. Level 7. They are expected to be pretty banged up.

**Stakes:** There are still passengers beyond themselves. The train is
headed towards critical infrastructure and they receive a message that if
they don't figure out how to stop it, people or not, the city will be
forced to stop the train... forcefully.

- If they can stop the crash and the response: win-win. The line survives
  and the train survives to run later. Full reward.
- If they can't, they can attempt to decouple and let what happens happen
  to the train. This would affect city infrastructure, but would keep most
  people alive. Reduced reward.
- If they super fail, everyone dies. No reward.
  - Likely fewer final consequences for the party, to keep the game going.
    No money.

Let's build a prompt for Claude, which I have more tokens to use on.

1. I like the live feedback display that works towards a status.
2. Instead of "time," I want attempts. If the players exceed so many
   attempts, the system fails. I back this with them doing something
   in-game/in-character that grants them an attempt. Depending on their
   success in the game, I give them a clue (up or down).
3. The "password" would be a set of characters that can be one of 16 values
   (hex). I'd like some balance to it so it shows problems but is difficult
   to revert.

## Claude

> I pay for Claude Pro with my own money. Code can eat through tokens very
> quickly, so I try to use it sparingly.
>
> The following prompt was where I got started in Claude, and it took about
> a dozen sessions of back-and-forth with tweaks and testing to get a
> working puzzle.
>
> Claude settings: Sonnet 5, Effort Low. My D&D campaign content is confined
> to a single project with lots of my lore files, but little of that played
> into the code development here.

```
You are building a D&D encounter tool: a Steam Engine Override Interface web app.

REQUIREMENTS:

### Core Mechanic
- Players input a code consisting of hex characters (0-9, A-F) that is between 6-7 characters long.
- Each correct character they input stabilizes a corresponding engine system
- Wrong characters cause specific system failures
- They win when all 7 characters are correct
- They lose if they exceed a maximum number of attempts (set to 15 by default, configurable)

### Attempt System
- Display "Attempts Remaining: X/12" prominently
- Each incorrect submission costs 1 attempt
- Partial correct inputs (e.g., getting 5/16 characters right) do NOT cost an attempt—only full submissions cost attempts
- When attempts reach 0, display a FAILURE screen with a "SYSTEM OVERLOAD" message

### Live Feedback Display
Create a real-time gauge dashboard showing engine status. Include these 5 systems:
1. **Pressure** (0-10)
2. **Structural** (0-10)
3. **Fuel** (0-10)
4. **Vibration** (0-10)
5. **Coolant** (0-10)
6. **Traction** (0-10)
7. **Exhaust** (0-10)

Each system should display:
- A visual bar/gauge (filled based on current value)
- A numeric value (e.g., "6/10")
- A color indicator (🟢 Green if 0-3, 🟡 Yellow if 4-6, 🔴 Red if 7-10)
- All systems should ideally stay in the 0-3 range for stability

### Scoring System
For each character in the input:
- **Correct character:** Contributes to system balance (lowers one or more systems toward 0-3 range)
- **Wrong character:** Causes 1-3 systems to spike upward (creating problems)

Design the scoring so that:
- Wrong characters create noticeable, varied problems (not all systems spike equally)
- The system is balanced—some characters fix multiple problems, others create new ones
- There's no obvious pattern (not alphabetical, not sequential)
- Players need to use clues strategically to narrow down possibilities

SUGGESTED PASSWORD: Generate a random 16-character hex code. Example: 3A7F2E8B1C5D9F4A

For each character position (0-15), define how correct vs. incorrect characters affect the 5 systems:
- Correct character: Reduces 1-2 systems by 1-3 points each
- Wrong character: Increases 2-3 random systems by 2-4 points each
- Some characters should have asymmetric effects (e.g., position 5 might spike Pressure by 3 when wrong, but barely affect Fuel)

### User Interface

**Input Section:**
- Text input field labeled "ENTER OVERRIDE CODE"
- Display current input in real-time (show characters as they type)
- Show a visual representation: how many characters are correct (e.g., "5/16 CORRECT")
- SUBMIT button
- Display "Attempts Remaining: X/12" in large, bold text (RED if X ≤ 3)

**Status Dashboard:**
For each of the 5 systems, display in a clean, readable format:
```

With some back-and-forth, this prompt was altered to get things to where
they were needed.

Generate a single React artifact (.jsx) — a 6-beat interactive terminal narrative for "Cogs vs Clips" by Softmax. Do NOT describe or summarize — just generate the artifact immediately. Say nothing before or after.

## Design

Dark terminal: bg `#0b1a0b`, text `#22c55e`, font `IBM Plex Mono, Menlo, Courier New, monospace`, accent `#fbbf24` (yellow). Max-width 720px centered. Top bar: red/yellow/green dots + "secure channel -- softmax research lab". Scanline: thin green bar animating top-to-bottom continuously. Blinking green block cursor.

## Typewriter Engine

Lines type at ~30ms/char. Each line object: `{pre:">> ", text:"...", color:"#22c55e", pause:300}`. Yellow text wrapped in markup. Lines appear sequentially with pauses. Use useEffect + setTimeout, clean up on beat change.

## Character Icons

Generate simple inline SVGs (~40x50px) for 4 characters. Each is a distinct geometric robot silhouette:
- **MINER**: pickaxe shape, color `#f0997b`
- **ALIGNER**: heart/balance shape, color `#e24b4a`  
- **SCOUT**: binoculars/eye shape, color `#5dcaa5`
- **SCRAMBLER**: lightning/disruption shape, color `#85b7eb`

## State

`beat` (1-6), `commanderName` (string), `typingDone` (bool). All navigation via internal buttons. Zero chat interaction after generation.

## Beat 1: "Comm Check"

Lines:
- `ESTABLISHING SECURE CONNECTION...` (pause 2500)
- `CONNECTION ESTABLISHED.` (pause 1000)
- blank
- `SOFTMAX // MISSION COMMAND`
- `[PRIORITY: CRITICAL]` (yellow, pause 400)
- blank
- `To: Unknown Agent`
- `From: Territorial Defense Initiative`
- `Re: Sector breach in progress` (pause 1000)
- blank
- `Encrypted transmission intercepted.`
- `Identity verification required to decrypt.` (pause 1000)
- blank
- `Agent, identify yourself` (yellow)

After typing: slide up input (placeholder "enter your callsign") + green DECRYPT button. On click: save name, go to beat 2.

## Beat 2: "The Transmission"

Lines:
- `LOADING TRANSMISSION...` (pause 500)
- blank
- `Transmission decrypted.`
- `{NAME}, listen carefully.` (pause 600)
- blank
- `The Clips are expanding.` (Clips=yellow)
- `Junction by junction. Sector by sector.`
- `Our perimeter is failing.` (pause 600)
- blank
- `We've lost 40% of contested territory in 72 hours.`
- `We're running out of options.`
- `We need a new plan.` (pause 600)
- blank
- `That's where you come in.` (yellow)

Show CONTINUE button when done. BACK in top bar.

## Beat 3: "The Arena"

Part 1 lines:
- `LOADING TACTICAL DISPLAY...` (pause 500)
- blank
- `This is the asteroid Machina_1.`
- `The Cogs and the Clips are competing for territory.` (Clips=yellow)

Show territory map: 28x12 grid. Left ~35% green cells, right ~65% yellow cells, jagged boundary. Header: "MACHINA_1 SURFACE // LIVE FEED". Footer: "COGS: 32%" green vs "CLIPS: 68%" yellow.

Part 2 lines:
- blank
- `Green territory is yours. Yellow is theirs.` (Yellow is theirs=yellow)
- `Capture junctions to expand. Lose them and you shrink.`
- `Your team must work together to win.` (work together=yellow)

CONTINUE + BACK.

## Beat 4: "The Roster"

Lines before cards:
- `LOADING BRIEFING...` (pause 500)
- blank
- `There are 4 types of Cogs.`

Show 4 roster cards sliding in sequentially (300ms stagger). Each: SVG icon (left), name in role color (bold), description in green.
- MINER (`#f0997b`): "Mines resources from the asteroid surface"
- ALIGNER (`#e24b4a`): "Uses resources to make hearts, and spends them to capture junctions"
- SCOUT (`#5dcaa5`): "Finds junctions to capture"
- SCRAMBLER (`#85b7eb`): "Attacks enemy junctions"

Lines after cards:
- blank
- `Mine resources to make hearts.` (make hearts=yellow)
- `Spend hearts to capture junctions.` (capture junctions=yellow)
- `Capture junctions to hold territory.` (hold territory=yellow)
- blank
- `The team that holds the most territory for the longest time wins.` (most territory + longest time=yellow)

CONTINUE + BACK.

## Beat 5: "Call to Action"

Lines:
- `COMMANDER {NAME}, HERE IS YOUR MISSION...` (pause 600)
- blank

Show 4 SVG icons in a row with float animation. Then "MISSION ORDERS" panel (glowing green border):
- 01: Choose a Cog to train.
- 02: Design a strategy (a "Policy") that teaches your Cog to be a good teammate.
- 03: Upload your Policy to compete in Alignment League.

More lines:
- blank
- `Use any AI (Codex, Claude, etc.) to help design your Policy.`
- `Use Softmax tools to train and improve.`
- `The fate of our species depends on you!` (yellow)

CONTINUE + BACK.

## Beat 6: "The Handoff"

Lines part 1:
- `Now it's time to build...` (pause 500)
- blank
- `Copy this into Codex, Claude Code, Terminal, or any coding` (yellow)
- `environment:` (yellow)

Show code block (green glowing border) with COPY button:
```
git clone https://github.com/Metta-AI/cogames.git
cd cogames
uv pip install cogames
```

Lines part 2:
- blank
- `Good luck Commander {NAME}!` (yellow)
- `Your agent will take it from here...`
- blank
- `End transmission` (dim)

BACK button only. No continue (last beat).

## Rules
1. ONE React component, default export, no required props.
2. ALL navigation internal — buttons inside artifact, no sendPrompt.
3. Use only Tailwind core utilities for layout. Custom CSS via style tags for animations.
4. Clean up all timeouts on beat change / unmount.
5. Generate NOW. Say NOTHING else.

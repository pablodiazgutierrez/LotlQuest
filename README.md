# Lotl Quest

A swim-and-zap game for lotlquest.com, built from the drawings: four axolotls named
**Mochi**, **Coal**, **Matcha**, and **Storm**, purple orbs, tridents, and a red lava sea.

Everything is in `index.html`. No build, no dependencies, no server needed.

## Play it

Double-click `index.html`, or serve the folder:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

On a tablet: landscape orientation, add to home screen for fullscreen.

## Controls

| | Player 1 | Player 2 |
|---|---|---|
| Tablet | drag a finger anywhere | drag a second finger |
| Keyboard | arrow keys | W A S D |
| Power | big round button, or SPACE | its own round button, or Q |

Menus work by mouse/touch **and** keyboard: `← →` pick a lotl, `ENTER` starts,
`2` toggles two players, `ESC` goes back, `?` or `H` opens How To Play.
There's a **?** button on the title, roster, and pause screens.

## Training mode

`🎓 TRAINING` on the lotl screen is the way in for a first-timer. It is the real game with
three differences: a hint card at the bottom walks through one lesson at a time (swim,
orbs, the trident, taking a hit, the big power), **nothing can beat you** — damage floors
at 25 HP and the bar heals itself back up — and there is no stage clock, so the boss never
arrives. The director holds each thing back until its lesson comes up, so lesson one is an
empty ocean and nothing else.

Nothing in a training run touches the save file: no score, no orbs, no unlocks. When the
last card says `YOU'RE READY!` a big gold button starts the real game, and `▶ SKIP TRAINING`
is there the whole time for the second child who already knows all this.

## How it plays

- **Drag anywhere** to swim. The lotl follows your finger — that's the whole control scheme.
- **The trident fires by itself, always forward (to the right).** Line enemies up on your right, or grab a temporary auto-aim target power-up.
- **Purple orbs** are the points, and they charge the power meter. Orbs magnet toward you when close, so near-misses still feel good.
- **Big round button, bottom-left** = the special power, once it hits 100%.
- A 100 HP health bar, long invincibility flash after a hit, and floating health refills keep damage easy to understand.
- Each level runs ~42 seconds, then that world's **boss** shows up. Beat it, restore 30 HP, and enter the next world. 8 worlds, 8 different bosses, then round 2 (same bosses, more HP). Endless.

## Levels and bosses

Every world has its own boss with its own trick. Big attacks always flash a **red
warning stripe with a yellow `!`** first, then hit — the lesson is "get out of the stripe."

| Level | World | Boss | What it does |
|---|---|---|---|
| 1 | Volcano | 🔥 Magma Lotl | Fans of 3 fireballs, sends blobs (the original) |
| 2 | Jungle | 🐸 Big Croak | Hops around spitting seeds; every 3rd hop lands on your row and the tongue lashes out |
| 3 | Desert | 🦀 King Pinch | Throws its claws like boomerangs (they come back!); sand walls with one gap |
| 4 | Ice Cave | 🐡 Frosty Puff | Puffs up then pops a ring of ice shards; icicles drop from the roof |
| 5 | Deep Sea | 🦑 Kraken | Tentacles slam up from the floor / down from the roof; ink shots |
| 6 | Candy Reef | 👑 Jelly Queen | Wobbly shots, baby jellies, and she vanishes and reappears (sparkles show where) |
| 7 | Shipwreck | 🦈 Ghost Shark | A pirate ghost shark: throws bones, then dashes across your whole row |
| 8 | Storm Sea | ⚡ Shadow Lotl | The final boss: fireball fans, lightning columns, and gap walls |

Beating the Shadow Lotl gives a `YOU BEAT ALL 8!` banner, a 👑 CHAMPION line on the
title screen, and round 2. Every boss turns "angry" (faster) below 40% HP, like before.

Tuning: bullets stay near the old Magma Lotl pace (~2/sec calm, ~3.4 angry). A bot that
swims in circles without dodging gets hit about as often by every boss as by the Magma
Lotl (10–15 hits/min); the Shadow Lotl is a little higher, the Ghost Shark lower. Boss HP
is now `60 + 13 per level` (it was `+28`), since the later bosses hit in more ways. Hearts
also float by during boss fights now. Each boss is one object in the `BOSSES` array.

## Continue and the level picker

- **Game over** on level 2+ offers `⏩ TRY LEVEL N AGAIN` as the big button (ENTER),
  with `🔁 LEVEL 1` (R) and `🏠 MENU` under it.
- **`🗺 LEVELS`** on the lotl screen (or `L`) opens a picker with every level you've
  reached, showing its boss. Locked ones are a `?`. A gold `⏩ CONTINUE` marks the level
  you last lost on, and the big button at the bottom continues from there — including
  round-2 levels past 8.
- The continue point is the last level you lost on (or quit from via the menu). Losing
  on level 1 doesn't overwrite it. A continued run starts at the beginning of that level,
  so there's time to charge the power before the boss.

## The collection

The card drawings were a roster screen, so that's what the character select is: **6 lotls,
2 unlocked at the start and 4 earned by playing.** Starting with almost the whole roster
left nothing to chase, so only Mochi and Coal are free. The last two come straight out of
the battle drawing — the yellow/orange fire one and the blue one.
Locked cards show as a `?` silhouette with a progress bar, which is the whole hook.

| Lotl | Power | What it does | How you get it |
|---|---|---|---|
| Mochi | 💗 Mochi Bounce | Restores 30 HP for every teammate, clears nearby enemy shots, shockwave | start |
| Coal | 🫧 Bubble Storm | 26 short-range bubbles in every direction | start |
| Matcha | 🌿 Vine Beam | Green beam in the direction you face | collect 75 orbs |
| Storm | ⚡ Thunder | Lightning hits every nearby enemy | reach level 2 |
| Blaze | 🔥 Fire Blast | Short-range spray in the direction you face | collect 300 orbs |
| Splash | 🌀 Whirlpool | Sucks every nearby orb into you | reach level 4 |

Unlocks are lifetime totals, so they carry across runs and he makes progress even on
bad games. Finishing a run that crosses the line gets a "NEW LOTL!" card with a sunburst.
`Blaze` and `Splash` are my placeholder names — he should rename them (they're in the
`LOTLS` array at the top of the script).

They differ only in the power, so an 8-year-old can pick by favorite color and not
be punished for it.

## The fun bits

- **Chains.** Grab orbs without a gap and the counter climbs — and so does the *pitch of
  the collect sound*, note by note. Every 8 orbs the multiplier steps up (x2, x3, x4…)
  with a flash and a ring. Taking a hit breaks the chain, which makes dodging matter
  beyond protecting the health bar.
- **ORB RUSH.** Every ~19 seconds a big sine-wave river of 42 orbs pours across the
  screen. It's the "wheee" moment, and it's where the huge chains come from.
- **Whirlpool + chains** is the best combo in the game: save Splash's power for the
  densest part of an Orb Rush and all nearby orbs fly into you as one enormous chain.
- **Hit-stop.** The frame freezes for a few beats when you take a hit or kill the boss,
  so impacts land instead of sliding by.
- **Target power-ups.** Grab the floating target for 7 seconds of auto-aim. Normal
  trident shots only ever go forward, so the power-up feels special.

## Two players (already works)

Character select → **2 PLAYERS** → tap a card for P2 → tap 2 PLAYERS again.
Two people, two fingers, one tablet. Each finger claims the nearest free lotl.
A downed player floats back after 8 seconds, so nobody sits out.

## Going online later

The seam is already in place. Every player is an entry in `G.players`, and a player's
entire input is a target point plus a power tap:

```js
window.LotlNet.applyRemoteInput(slot, tx, ty, firedPower);
window.LotlNet.localInputSnapshot();  // -> [{slot, tx, ty}]
```

That's the whole network payload — no physics to sync, since positions ease toward
the target. Local co-op already runs through this exact path. To go online:

1. Pick a host (WebSocket room server, or peer-to-peer via WebRTC for 2 players).
2. Send `localInputSnapshot()` for your slot ~15×/sec; apply what arrives.
3. Make the spawn director deterministic — replace `Math.random()` with a seeded RNG
   and send the seed at match start. That's the one real change needed.

## Deploying to lotlquest.com

It's a single static file, so anything works — Cloudflare Pages, Netlify, GitHub Pages,
or an S3 bucket. Drop the folder in and point the domain at it. No backend until multiplayer.

## Fixed after the first playtest

- **Enemies wouldn't die.** `hurtFoe` played the explosion and paid out the score but
  never removed the foe, so it kept flying with 0 hp and re-paid every time it was hit
  again. Foes now get a `dead` flag and are swept from the world, and score is paid once.
- **Buttons ignored mouse clicks and taps.** The button list is rebuilt every frame, and
  release compared the *object* pressed against the newly-built one, so it never matched
  unless press and release happened inside a single frame. Buttons now carry stable ids.
- **Keyboard was uncontrollable.** Holding a key threw the target point 190px ahead, which
  pinned the lotl at max speed instantly. Keys now walk the target at a steady ~336 px/s
  (about 3 seconds to cross the screen) with a short coast when you let go.
- **Two players shared one keyboard binding.** Arrows and WASD both drove P1 and P2 was
  stranded. Split: P1 arrows + SPACE, P2 WASD + Q. Solo still accepts both sets.

## Known rough edges

- Difficulty past stage 5 is untuned guesswork; watch him play and adjust `STAGE_LEN`
  and the spawn rate in `director()`.
- If two lotls unlock in the same run, only the first gets a celebration card (the
  second is still unlocked and waiting on the collection screen).
- Unlock thresholds (250 orbs / stage 3) are a guess at his pace — first session will
  tell you whether that's too grindy. They're in the `LOTLS` array.
- One enemy type per behavior (blob / spiker / puffer / jelly). Bosses are where his
  next drawings should go — a new boss is one entry in `BOSSES` (an `update` and a `draw`).
- Boss tuning past level 5 is measured with a bot, not with him; watch which boss he
  gets stuck on and adjust its timers (each boss's numbers are at the top of its `update`).
- Art is drawn in code to match the crayon style. The best next step is scanning his real
  drawings for the *collection cards* while leaving gameplay art code-drawn (it has to
  rotate, flash, and animate) — his actual art where you can linger on it, smooth
  animation where you can't.
- No music yet, just sound effects.

## Ideas from the drawings not built yet

The cards in the first drawing look like a **collectible card / roster screen** — there's
a real game in "collect the lotls, build a team." The second drawing has the whole team
fighting together above the lava, which points at co-op more than solo. Both are worth
asking him about.

## Tuning it with him

The live game state is on `window.LotlGame`, so you can mess with it from the browser
console while he watches — great for "what if the boss came right now?":

```js
LotlGame.stageTimer = 99999    // summon the boss
LotlGame.players[0].power = 100 // fill the power meter
LotlGame.players[0].health = 100 // refill the health bar
LotlGame.stage = 5              // jump to level 5 (set it before summoning the boss)
LotlGame.combo = 40             // fake a huge chain
```

`window.LotlKeys` is the live set of held keys, handy if one ever seems stuck.

To unlock everything instantly (or reset it all):

```js
// unlock all
JSON.parse(localStorage['lotlquest.save.v1'])            // peek
localStorage['lotlquest.save.v1'] = JSON.stringify({...JSON.parse(localStorage['lotlquest.save.v1']), orbsAll: 9999, bestStage: 8})
// wipe and start over
localStorage.removeItem('lotlquest.save.v1')
```

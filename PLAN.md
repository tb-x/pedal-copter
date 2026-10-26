# PLAN: "Pedal-Copter" (an Ugh!-like sky taxi, `pedal-copter/`, port 5229)

Drafted 2026-10-24. Status: milestones 1–5 done on 2026-10-24 (prototype, art, all 12 levels, polish and sound,
release prep). Still to do from milestone 5: a playtest on a real phone through the telemetry server (frame times,
an older phone). Milestone 6 done 2026-10-24: public repo tb-x/pedal-copter, live at https://tb-x.github.io/pedal-copter/ and on
the landing page. Recorded sound (voices, effects, music) is in, see `SOUND-PLAN.md`.

Changed after publishing (2026-10-26, at the user's request):
- No landing stones: a stop is its whole ledge; land anywhere on it and people walk over to the copter.
- Random power-ups to catch: feather (faster), log (one more seat), steak (food), flower (patience).
- A mountainside instead of a cut-through: natural rock with moss and plants, boulders, a meadow in front.
- Palms with one curved trunk of stacked, lipped segments and arching feathery fronds.
- Then: lumpy, varied rock (plates, grooves, colour patches, moss on ledges, a jagged crest), and the power-ups drop
  from palms as coconuts instead of floating up.
- Then: a real volcano shape, scree slopes, a leaning back wall with rock shelves, a pterodactyl nest (background
  flyers keep above the crags), a different doorway for each kind of cave, and a pose system that keeps passengers
  busy (idle acts, moods, excitement, riding, cheering).

Built differently from this plan (milestone 3):
- Levels 6 and 7 have four stops, not five (the rock steps and caves need the room).
- An empty food meter doesn't stop the copter climbing: it climbs and flies slowly instead, so nobody can get stuck
  on the ground far from food.
- The long-neck in level 6 lies asleep in the middle of the valley; you land on its back, not its neck.
- New stop kinds: painting cave 🎨, hot spring ♨️, wheel workshop 🛞, dino nest 🥚. Mammoth meadow, beach and
  volcano lookout aren't used.
- Arrival times and patience in the table below were starting points; the tuned values are in `LEVELS` (later levels
  got more patience, as the wide valleys take longer to cross).
- No pinch zoom on wide levels: the camera follows the copter, and arrows at the screen edges point to people waiting,
  to riders' stops and (when hungry) to food.

**The idea in one line.** A caveman pedals a wooden helicopter around a stone-age valley, picking up cave people
who wait on cliff ledges and flying each one to the ledge their bubble shows. It's Ugh! (1992), itself a stone-age
Space Taxi, made gentle for a 5-year-old and drawn like Going Up!.

**Tech.** Same as Going Up!: a single `index.html`, three.js **0.160.0** from jsDelivr through an import map, every
model built in code (no picture files, no PixelLab), synthesized Web Audio first. A `manifest.webmanifest`, `sw.js`
copied from `blast-off/sw.js` with `PREFIX = 'pedalcopter'`, icons, a home button (`../index.html`), and
`localStorage` under `pedalcopter.<key>`.

## 1. What it borrows from Going Up! (so the two feel like siblings)

| Going Up! | Pedal-Copter |
| --- | --- |
| A skyscraper with its front cut away, rooms visible | A valley diorama with the cliff fronts cut away, so you see into the cave homes |
| Floors with a colour, an icon and a big number | Ledges ("stops") with a colour, an icon and a big number on a carved stone post |
| People pop out of rooms and queue by the lifts | Cave people pop out of caves and wait on the ledge's landing stone |
| Destination bubble: colour, icon, big number | Exactly the same bubble, reused |
| Mood faces: happy, ok, grumpy, red and steaming | The same mood stages, reused |
| Gives up and takes the stairs | Gives up and stomps off down a vine ladder, or rides away on a little dinosaur |
| Lamps on the car show seats taken | Stone seats behind the pilot, a little lamp (torch) per seat |
| Stars: 3 if nobody gave up, 2 if two or fewer, else 1 | The same, with no timer and no losing |
| Level map tower, win card, hint hand, HUD bars | Reused, restyled in stone and wood |
| Camera: FOV 30, slight yaw and pitch, fits the building | The same camera, fitting the valley; wider levels follow the copter |
| Concrete cut edge with rebar dots | Rock cut edge in strata (soil, sandstone, limestone) with fossil dots (ammonites) |

**Code reuse.** Copy (don't import) the generic pieces out of `elevator/index.html`: the figure pools and rig, the
bubble and mood drawing, the HUD (top bar or side columns), the level map, the win card, the hint hand, the audio
skeleton (master gain, mute, unlock), and the test hooks. Each repo stays self-contained.

## 2. How it plays (age 5, no reading)

- A cave person comes out of a cave, walks to the landing stone at the edge of the ledge, waves and shows a bubble:
  the colour, icon and number of the stop they want.
- Fly over and **land** on their stone. They hop in by themselves. Riders keep their bubbles, so the child can see
  "3 🐟" and fly to stop 3.
- Land on the right stop and those riders hop out, cheer and walk into the cave. Anyone waiting there hops in.
- Bumping into rock, trees or a pterodactyl is never a crash: the copter boings back softly, the pilot's eyes spin,
  the riders say "whoa!". Dipping into the lake bobs the copter back up with a splash. Nobody falls out.
- People who wait too long go grumpy, then red and steaming, and finally climb down a vine ladder or ride off on a
  little dinosaur (counted next to a dino icon in the HUD). Riders in the copter get grumpy much more slowly and never
  leave.
- Each level needs a number of happy arrivals (little people at the top fill in green). No timer, no game over.

### Controls (phones and tablets first)

Two schemes are built in the prototype and tried with the child before choosing (see the open questions):

1. **Follow my finger (recommended default).** Touch and hold anywhere: the copter flies towards the finger, with
   gentle speed and turning limits. Let go: the pilot stops pedalling and the copter slowly comes down (it drifts
   to a stop sideways and sinks gently, never drops). Landing is easy: when the copter is over a landing stone and
   moving down slowly, it settles on it with a thump and a puff of dust, so letting go above a stone is a landing.
   Landing on flat ground also works. One gesture, nothing to learn, and it still feels like flying.
2. **Pedal and steer (closer to Ugh!).** A big round pedal button bottom-right (hold to pedal = climb, let go =
   sink slowly), ◀ ▶ pads bottom-left, like Heli Rescue's corner pads. Harder, so it's for older kids or a parent.

Always on, for both: a short **tap on a stop's stone** on levels 1-2 flies the copter there by itself (training
wheels, knob `AUTOPILOT_LEVELS`). Keyboard: arrows or WASD to fly, Space to pedal. Pinch and two-finger drag zoom and
pan the camera only on wide levels. `touch-action: none` on the canvas, `manipulation` on buttons; taps on the home,
map and mute buttons never reach the flying controls.

### The pilot gets hungry (Ugh!'s stamina, from level 7)

A drumstick meter by the pilot drains slowly while pedalling. When it's empty the copter can't climb any more and
sinks gently to the ground (never falls). Land at the fruit tree or the campfire stop and the pilot munches it full
again. Gentle, and it adds a little planning. It can be switched off per level.

## 3. The world and the art

A **museum diorama of a stone-age valley in warm daylight**, following Going Up's realism notes (`elevator/handoff/
ART-REALISM.md`): natural materials, believable proportions, muted colours, with saturated colour kept for bubbles,
stop signs and the copter's leaves.

- **Cliffs and caves**: layered rock with a visible cut edge (strata and fossil dots) and cave homes cut open:
  fur beds, a fire pit with a glow, cave paintings, clay pots, a drying rack with fish. Like Going Up's rooms, life
  goes on inside: a cave person stirs a pot, a kid draws on the wall, a baby sleeps.
- **Stops** (each a colour, an icon and a number on a carved stone post): cave homes 🏠, fruit tree 🍎,
  fishing lake 🐟, mammoth meadow 🦣, campfire 🔥, painting cave 🎨, hot spring ♨️, wheel workshop 🛞, dino nest 🥚,
  beach 🐚, volcano lookout 🌋.
- **Valley life**: a river and waterfall, ferns and palm trees (instanced), a sleepy brontosaurus whose neck you can
  land on, a little T-rex chasing a butterfly on the ground, pterodactyls gliding across, birds, dragonflies, a mammoth
  family, smoke from cave fires, fish jumping.
- **The copter**: a log frame lashed with vines, a stone-wheel-and-belt drive to a rotor of big leaves, a log seat,
  and the pilot's legs visibly pedalling, faster when climbing. Rider seats behind, one lamp (torch) each. Bigger
  copters on later levels (1, 2, then 3 seats; a 4-seat "bus" with a hide canopy on the last levels).
- **People**: the Going Up! figure rig in furs and hides, hair buns with a bone, beards, kids, grandmas with a walking
  stick. Mood faces as in Going Up.
- **Time and weather**: day, dusk, night (torches on the stops and the copter, glowing cave mouths, stars), wind
  (leaves blow across and the copter drifts), rain, snow (the ice-age level).

## 4. Levels (12 to start; times in seconds at normal speed)

| # | Name | Stops | Seats | Arrive every | Patience | Target | New |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Two Caves | 2 | 1 | 7 | 60 | 4 | tap-to-fly, hint hand always |
| 2 | Fruit Tree | 3 | 1 | 6.5 | 55 | 6 | tap-to-fly |
| 3 | The River | 4 | 2 | 6 | 50 | 8 | waterfall, lake splash |
| 4 | Tall Cliff | 4 | 2 | 6 | 50 | 8 | stops at very different heights, vertical pan |
| 5 | Windy Day | 4 | 2 | 5.5 | 45 | 10 | gentle gusts |
| 6 | Dino Meadow | 5 | 3 | 5.5 | 45 | 10 | pterodactyls (soft bumps), brontosaurus neck stop |
| 7 | Dusk at the Lake | 5 | 3 | 5 | 42 | 12 | hunger meter, fireflies |
| 8 | Wide Valley | 6 | 3 | 5 | 42 | 12 | two screens wide, camera follows, off-screen arrows |
| 9 | Torch Night | 6 | 3 | 4.5 | 40 | 12 | night; a stop's torch lights when someone waits |
| 10 | Rainy Day | 6 | 3 | 4.5 | 40 | 14 | rain, bigger waterfall, grandma (slow, patient) |
| 11 | Ice Age | 6 | 4 | 4 | 38 | 14 | snow, mammoth family, hunter with a big fish (2 seats) |
| 12 | Volcano Feast | 7 | 4 | 4 | 36 | 16 | smoke rings to fly through, everyone heads to the feast |

Trip mix: most trips run between cave homes and the other stops, with rushes like Going Up's ("fishing time":
four people want the lake at once).

## 5. Tuning knobs (named constants at the top of the script, listed in the README)

```
GRAVITY = 4            // units/s² pull when not pedalling (sinking feels slow and safe)
LIFT_MAX = 9           // climb acceleration at full pedal
FLY_SPEED = 6          // top speed (units/s); a ledge is ~3 units tall like a Going Up floor
FLY_ACCEL = 8          // speeding up and braking (units/s²)
FOLLOW_DEADZONE = 0.4  // follow-the-finger: closer than this and the copter just hovers
LAND_SNAP = 1.2        // horizontal distance within which a slow descent settles onto a stone
LAND_MAX_FALL = 3      // descending slower than this counts as landing (generous)
BUMP_BOUNCE = 0.5      // share of speed kept when bouncing off rock
WIND_GUST = [0, 1.5]   // gust strength (units/s) on windy levels
HUNGER_TIME = 60       // s of pedalling on a full tummy
MOOD_T = [0.35, 0.6, 0.8, 1.0]  // as Going Up
RIDE_PATIENCE_RATE = 0.3        // as Going Up
AUTOPILOT_LEVELS = [1, 2]       // levels where tapping a stop flies there
HINT_DELAY = 2 / 8              // level 1 / later
```

## 6. Code layout (banner sections in one `index.html`)

`config` → `levels` → `art: terrain and caves` → `art: copter` → `art: people` (from Going Up) → `art: animals` →
`physics` (copter, wind, bumps, landing) → `sim` (arrivals, mood, boarding, delivery, giving up) → `input`
(follow / pads / keys / autopilot) → `camera and layout` → `HUD, map, win card, hint` → `audio` → `telemetry hooks` →
`boot`. One `<style>` with the `/* Layout: ... */` comment and tokens on `:root`.

Terrain is a few merged meshes per level (cliff profile extruded with strata colours), plants are instanced, the
people use Going Up's shared pools: aim for ~150 draw calls on the biggest level, with Going Up's adaptive sharpness.

Telemetry: `window.tbxEvent?.('level-start' | 'level-end' | 'gave-up' | 'bump', {...})`, few and short.

Test hooks as in Going Up: `?level=N`, `?demo` (the autopilot plays), `?fast`, `?tick`, `?mute`, `?seed=N`,
`?ctl=follow|pads`. A Node test (`dev/simtest.mjs`) runs the bot through every level and checks every stop is
reachable and every level can be won with three stars.

## 7. Sound

Synthesized first: a creaky pedal-and-rotor whirr whose pitch follows the pedalling, a "boing" for bumps, a thump and
dust for landing, a splash, hop-in and hop-out chirps, the grumble and stomp from Going Up, a fanfare and stars, and a
bone-xylophone tune on Robot Spy's `Music` scheduler. Stop numbers spoken by `speechSynthesis`.

Optional later, only if you ask: ElevenLabs clips like Going Up's (a stock voice saying the numbers 1-12 and short
caveman lines like "Ugh!", "Ooga!", "Hooray!", plus ~15 effects and day and night music loops). Rough cost: voice
~300 credits, effects ~300, two 45 s music loops ~1,200: about **1,800 credits** of the Creator plan's 121,902 a month.
Licence rules from the root CLAUDE.md apply (clip list in the README, music means the game stays free).

## 8. Milestones

1. **Prototype (greybox).** Flat-shaded valley, 3 levels, both control schemes, land, board, deliver, mood, give up.
   **Playtest with the child on a phone and a tablet** and pick the control scheme. This is the big risk: if flying
   is too hard, lean harder on tap-to-fly and landing help.
2. **Art pass.** Cliffs, caves and their life, the copter, people, animals, day, dusk and night.
3. **All 12 levels** with wind, hunger, hazards, specials and wide levels with a following camera.
4. **Polish.** HUD, level map, win card, hint hand, sound, haptics (from `math5/haptics.js`).
5. **Release prep.** README, Home Screen copy (manifest, `sw.js`, icons), telemetry check on a real phone through the
   root server (`python _telemetry/report.py pedal-copter`), performance on an older phone.
6. **Publish (ask first).** New repo `tb-x/pedal-copter`, Pages, `.nojekyll`, leak checks, the root CLAUDE.md table
   row and the landing page `GAMES` list.

## 9. Open questions

1. Name: "Pedal-Copter", "Cave Copter" or something else?
2. Controls: default to follow-the-finger, with pedal-and-steer as an option? Decide after the prototype playtest.
3. Hunger meter: in (from level 7) or out?
4. Recorded voice and music now, later, or never?

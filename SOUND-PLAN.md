# Pedal-Copter: recorded sound with ElevenLabs (proposal)

Drafted 2026-10-24. Approved 2026-10-24: all three parts (voices, effects, music); the number of music loops is decided
later.

**Status:**
- Step 0 (audition) done 2026-10-24: the user picked **Roger** (pilot) and **Alice** (passengers).
- Step 1 (voices) done 2026-10-24: 32 clips in `assets/voice/` (four takes cut apart, one retake for "Whee!"), wired
  into the game with the browser's voice as fallback, listed in the README's Audio credits. Waiting for the user to
  listen. Credits used so far (audition, takes, retake, transcription checks): about 2,700 by the balance, more than
  the ~1,150 the text alone suggests; the pause tags and the transcriptions seem to cost more than expected.
- Step 2 (effects) done 2026-10-24: 21 effects, two takes each, the better one in `assets/sfx/` (picked silently),
  wired in with synthesized fallbacks, listed in the README; listening page `review/sfx.html` with both takes.
  About 580 credits. Waiting for the user to listen.
- Step 3 (music) done 2026-10-24: three loops (day, night, feast), 47 s each, `assets/music-*.mp3`, made seamless in
  the browser, with the synthesized tune as the fallback; listed in the README. About 1,940 credits.
- All three parts are in. In all, the sound used about 5,200 credits (47,664 → 52,868 used this month).

**Budget.** Creator plan: 74,238 credits left this month (47,664 of 121,902 used, checked 2026-10-24); it resets around
2026-11-23. This whole plan costs about **4,000–5,000 credits**, roughly 6% of what's left.

**Rules that apply** (root CLAUDE.md, "Licences for generated assets"): stock voices only, no cloning; paid-plan
audio needs no credit line; with Eleven Music in it the game must stay free (no ads or payments), which it is; the README
gets an "Audio credits" clip list (file, tool, voice, model, plan, date) and says the audio isn't licensed for reuse;
no features marked beta.

## What changes, and what stays synthesized

| Part | Now (synthesized) | Proposed |
| --- | --- | --- |
| Spoken lines | the browser's voice says numbers, "Yum yum!", "Hooray!" | **recorded voices**: a pilot and the passengers |
| Effects | tones and short noise bursts | **recorded** for the ones that sound cheap now (squawk, splash, snore, roar...) |
| Music | bone-xylophone tune from a note scheduler | **Eleven Music** loops per mood, the synthesized tune kept as the fallback |
| Rotor whirr and creak | oscillator following the pedalling | **stays synthesized**: it has to follow the pedalling every frame, and a recorded loop risks the constant hiss you disliked in Robot Spy |
| Raindrops, wind gust | soft tones, a short whoosh | stay synthesized (same hiss risk) |

Everything recorded loads as `assets/<name>.mp3?v=ASSET_V` into the game's `AudioContext`, as in Blast Off. Each sound
falls back to today's synthesized version (or the browser's voice for speech) if its file can't load, so the game never
goes quiet. The service worker caches the files with the rest, so they work offline.

## 1. Voices (about 1,000 credits)

**Two stock voices**, picked by ear from a short audition (step 0):
- **The pilot**: a warm, slightly silly caveman, say a stock voice like George or Charlie with a bit of growl. He
  reacts to what happens.
- **The passengers**: one friendly voice (e.g. Lily or Jessica) for the numbers and stop names, so they're always
  clear. (Going Up uses Will, Cuckoo Clock uses Jessica; a different voice keeps the games apart, but reusing one
  is fine too.)

**Lines** (about 450 characters in all):
- Numbers 1 to 7, said like a request: "One!", "Two!" ... "Seven!" (said when someone hops in, like Going Up's floors).
- Stop names, said after the number: "Home!", "Fruit tree!", "Fishing!", "Campfire!", "Painting cave!", "Hot spring!",
  "Wheel workshop!", "Dino nest!".
- Pilot: "Ugh!" (bump), "Ooga booga!" (lift-off, now and then), "Whoa!" (pterodactyl), "Yum yum!" (eating), "I'm
  hungry!" (food low), "Splash!" (lake), "Hop in!", "Off you go!", "Hooray!" (win), a few grunts and a happy laugh.
- Passengers: "Yay!", "Thank you!", "Wheee!" (riding), "Hmph!" (giving up), a grandma's "Oh my!", the hunter's
  "Big fish!".

**Recording**: with `eleven_multilingual_v2` (about 1 credit a character), short lines recorded together in one take
per voice with pauses between, then cut apart. Single one-word takes drift off the voice. Each clip is checked for
the words (speech to text) and for the voice (compared with the rest of the set). Two takes of everything: about
**1,000 credits**, plus about **150** for the audition.

## 2. Effects (about 900 credits)

ElevenLabs sound effects, about 10 credits a second, short clips, three takes each (short one-shot prompts often come
back quiet or with extra hits; level and length get fixed in code):

squawk (pterodactyl), splash, boing (cartoon bump), wooden thump (landing), dino snore, little T-rex roar, mammoth
trumpet, owl, birdsong chirps, volcano rumble, lava blorp, crunchy munch, bubbling hot spring, stone chisel taps,
egg wobble and tiny crack, ring "pop" sparkle, stomp, hop-in pop, kids' short "yay" cheer, fanfare sting.

About 20 effects × 1.5 s × 3 takes: **about 900 credits**.

## 3. Music (about 1,700–2,800 credits)

Eleven Music (`music_v2`, instrumental), about 750 credits a minute: playful stone-age percussion, marimba/bone
xylophone, log drums, a little flute; no vocals. Each loop is made loopable by blending its last 2 s into its start (as
Nut Run and Choo-Choo Loop).
- **Three loops** (day, night, volcano feast), 45 s each: about **1,700 credits**; dusk, rain and snow reuse day/night.
- **Or five loops** (day, dusk, night, snow, feast): about **2,800 credits**.

The music dips under speech, as now.

## Steps

0. Audition: three candidate voices say the same two lines (about 150 credits). **You pick by listening.**
1. Record the voice lines in takes, cut, check words and voice. You listen to the set.
2. Effects: three takes each, judged silently (decoded in the Browser pane, never played aloud), the best one kept.
   You listen to the picks.
3. Music loops. You listen.
4. Wire it in: clip loader with fallbacks, `ASSET_V`, volumes (`VOICE_GAIN`, `SFX_GAIN`, `MUSIC_VOL`), the README's
   Audio credits and clip list.
5. Test silently, then you play it with sound on.

I'll ask before each paid step, with its cost.

## Questions for you

1. All three parts (voices, effects, music), or only some?
2. Music: three loops or five?
3. One voice for everything, or a pilot and a passenger voice?

# Option B: rigged 3D character, what to get made and where it goes

The website side is DONE. Drop two files into `/public` and rebuild (`npm run build`):

| File | Required | What it is |
|---|---|---|
| `hero.glb` | YES | The rigged 3D model of him (spec below) |
| `hero-intro.mp3` | optional | His voice reading the introduction. Without it, the browser's built-in voice is used. |

Until `hero.glb` exists, the page shows your exact PNG (nothing is broken or empty).

## Honest expectation
A 3D model is a NEW 3D asset made to look like the man in your PNG. It will resemble him, but it will not be
pixel-identical to the photoreal PNG (face, skin and fabric will look like a 3D render). The closer you need the
likeness, the more a professional 3D artist / high-fidelity pipeline is worth it.

## How to get the GLB (note: Ready Player Me was shut down on 31 Jan 2026, so do NOT use it)
1. Photo-to-avatar tools still operating as of the last check: MetaPerson (Avatar SDK) and Avaturn. Both turn photos
   into full-body avatars and export GLB. Before paying, confirm that the export includes (a) a humanoid skeleton and
   (b) facial blendshapes / visemes (item list below), and that you can add or retarget animation clips.
2. High fidelity: Unreal MetaHuman or Reallusion Character Creator, then export GLB (needs Blender cleanup).
3. Best likeness: hire a 3D character artist, send them `Stylish_Indian_Man_Waving.png` plus this document as the brief.
   Several photos from front, side and back make the likeness much better.

## Technical requirements (give this list to the artist or tool)
- Full body, humanoid skeleton, T-pose or A-pose rest, Y-up, 1 unit = 1 metre, feet at y = 0.
- Appearance: the same man: black short-sleeve-rolled shirt, black trousers, black belt, white sneakers, black watch.
- A node or bone whose name ENDS WITH `Head` (e.g. `Head`, `mixamorig:Head`). Used for the look-at-content glance.
- Mouth: EITHER facial morph targets on the head mesh (best), OR just a jaw bone whose name contains `Jaw` (fallback:
  the mouth then simply opens/closes with the speech, with no mouth shapes). Morph names:
  `viseme_aa viseme_E viseme_I viseme_O viseme_U viseme_PP viseme_FF viseme_TH viseme_DD viseme_kk viseme_CH
  viseme_SS viseme_nn viseme_RR viseme_sil` + `mouthSmile` (or `mouthSmileLeft`/`mouthSmileRight`) +
  `eyeBlinkLeft eyeBlinkRight`. (`jawOpen` is also driven if present.)
- Four animation clips, exported inside the GLB. Names only need to CONTAIN these words:
  - `Walk`: normal walk cycle IN PLACE (no root movement, loopable). The page moves him across the screen itself.
  - `Idle`: relaxed standing, subtle breathing, loopable, matches the PNG pose.
  - `Wave`: RIGHT hand wave, about 3 s, starts and ends from the idle pose.
  - `Point`: free hand (his LEFT) gestures/points toward the screen-left (where the name and text are), about 3 s,
    starts and ends from idle.
- Texture/size: under 15 MB, Draco or Meshopt compressed, textures 2K max, under about 80k triangles.
- Delivery name: `hero.glb`.

## What the page does automatically (already built and tested)
Timeline, about 26 s: he WALKS IN from the right (Walk clip), turns to face the visitor, WAVES while saying "Hello!", and
speaks the full script with lip-sync; for the last two sentences ("Welcome to my portfolio! Let me show you my
professional journey.") he glances toward the portfolio text and POINTS; blinks and smiles throughout, bigger smile at
the end; then returns to Idle. Without a Walk clip he simply turns in place. Visuals autoplay muted; "Sound on" restarts
the sequence with voice; Replay appears at the end.

Script used: "Hello! I'm Ananya Sahiki Pechetti, a Software Development Engineer in Test. I have 3 years of experience in
software automation and quality engineering. I specialize in TestComplete, Python automation, CI/CD pipelines and Azure
DevOps. I enjoy building reliable automation solutions and improving software quality. Welcome to my portfolio! Let me
show you my professional journey."

## Practical route to a walking, waving model (how to actually get it)
1. Make a 3D body+face of him: send front/side/back photos + `Stylish_Indian_Man_Waving.png` to a 3D artist, or try an
   image-to-3D generator (e.g. Meshy, Tripo; results vary, check the face likeness before going further).
2. Rig + animate: Mixamo (mixamo.com, free with an Adobe ID, still online but in maintenance mode) auto-rigs a humanoid
   and offers Walking (use "In Place"), Idle, Waving and Pointing clips. Download each as FBX "With Skin" (first) and
   "Without Skin" (others). Alternatives: Meshy's own rigging (has walk and wave presets), or Blender + free clips.
3. Merge into ONE GLB in Blender (import the skinned FBX + each clip, rename the clips Idle / Walk / Wave / Point,
   export glTF Binary with "Animation" -> "Group by NLA Track" so all clips are inside the file).
4. Lip-sync: Mixamo rigs have no mouth shapes. Either have an artist add the viseme blendshapes (best), or leave a
   `Jaw` bone, and the page will open/close the jaw in time with speech.
5. Copy to `/public/hero.glb`, rebuild, send it to me for a tuning pass.

## Voice file (optional, recommended)
Record or generate (any text-to-speech) the exact script, export `hero-intro.mp3` (~12-16 s), and put it in `/public`.
The mouth timeline stretches to match the mp3's real length. Without the mp3 the browser voice is used and sync is approximate.

## Honest status of testing
The integration was tested with a crude stand-in rig (boxes and a sphere with the same clip and morph names): the
walk-in, turn, wave, head glance, mouth morphs, sound button, restart and Replay all ran. It has NOT been tested with a real
character model, because none exists yet. Expect to spend a short round tuning gesture timing and camera framing once
the real GLB arrives; send it to me and I will do that.

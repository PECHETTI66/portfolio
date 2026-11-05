# Hero animation: what is needed to animate YOUR exact character

## Why the PNG alone is not enough
`Stylish_Indian_Man_Waving.png` is one flat 2D picture, front-facing, with his left arm in the pocket.
It contains no information about: the back of his head and body (needed to turn), the inside of his mouth
and teeth (needed for lip-sync), or his pocketed arm and hand (needed to point). Any code that "animates"
it would have to invent that missing detail, which changes his face/body, so it is not done here.

## Option A (recommended): a video of the SAME man, generated FROM your PNG
Use an image-to-video / talking-avatar tool (e.g. Higgsfield, HeyGen, Hedra, Runway, Kling) with your PNG
as the reference/first frame, plus your intro audio (or the script below).

Prompt: "Keep this exact man unchanged: same face, hair, beard, black shirt, black trousers, white shoes.
Full body, locked static camera, plain flat background. He turns slightly toward the viewer, waves his right
hand, looks toward the left side of the frame, points toward the left with his free hand, speaks the script
with accurate lip-sync, smiles, and returns to this exact relaxed standing pose."

Script: "Hello! I'm Ananya Sahiki Pechetti, a Software Development Engineer in Test. I have three years of
experience in software automation and quality engineering. This is my professional portfolio. Welcome!"

Export requirements
- Same framing as the PNG (full body, 2:3 portrait, e.g. 1024x1536 or 720x1080), camera locked, 24-30 fps, 8-15 s.
- Transparent background (best): WebM VP9 with alpha -> `hero.webm`; Safari: HEVC-with-alpha MP4 -> `hero.mp4`.
- If the tool cannot export alpha: render on a plain GREEN or WHITE background, then key it out:
    ffmpeg -i raw.mp4 -vf "chromakey=0x00FF00:0.12:0.05,format=yuva420p" -c:v libvpx-vp9 -auto-alt-ref 0 -an hero.webm
  (keep the audio in a separate-track version, see below).
- Audio: keep the voice in the file you ship (the player starts muted; the Sound button un-mutes).
  WebM: add `-c:a libopus`. MP4: `-c:a aac`.
- First and last frame = the relaxed standing pose of the PNG, so the loop/hand-off is seamless.

## Option B: rigged 3D GLB (real-time, fully interactive)
A GLB of this man (e.g. made from your photo/PNG with Ready Player Me, Avaturn, or a 3D artist) needs:
- Humanoid skeleton (Mixamo/Unreal compatible), full body, textured, black shirt/trousers/white shoes.
- Facial blendshapes: the 15 ARKit/Oculus visemes (viseme_aa, _E, _I, _O, _U, PP, FF, ...) + eyeBlink + smile.
- Animation clips: Wave (right hand), Point (left arm), Idle. Audio + viseme timeline for lip-sync.
- Delivery: `hero.glb` (< 15 MB, Draco/Meshopt compressed), then integrate with @react-three/fiber.
Note: a 3D model will look like a 3D avatar of him, not identical to the photoreal PNG.

## Dropping it in (already wired up)
Copy `hero.webm` and/or `hero.mp4` into `/public` and rebuild. Nothing else to change:
- `hero.png` (your exact character) is always the base layer and the poster frame.
- The clip fades in over it, autoplays muted, and a Sound button + Replay button appear.
- No clip present = the PNG simply stays, nothing breaks.

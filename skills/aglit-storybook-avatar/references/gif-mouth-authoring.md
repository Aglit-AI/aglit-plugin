# GIF and speech-mouth authoring

## Choose an honest animation strategy

1. Best illustrated result: separate face/mouth layers or a sprite atlas containing closed, narrow, medium and wide speech shapes, with matching face/background transparency. A future renderer must explicitly support those shapes before claiming phoneme selection.
2. Existing supported talking GIF: draw a short sequence of small, medium and open shapes, return to closed between syllables, and keep its pose aligned to the idle image. Set talkingUrl; the runtime switches it only during assistant speech.
3. Lightweight fallback: keep the transparent idle GIF/portrait and calibrate the runtime mouth overlay. Avoid a large baked smile beneath the overlay. Inspect for doubled lips and floating mouth pixels. The current Lucas fallback uses this mechanism.

A volume envelope is not phoneme recognition. Do not claim exact A/E/O timing without a real phoneme/viseme source.

## Prepare the artwork

Preserve the approved character, clothes, hair and face. Ask image generation for actual transparent alpha, with a neutral closed mouth and the same full canvas, camera, shoulders and baseline for every frame. For visual changes use the available image-generation skill/tool; do not substitute unrelated artwork. Keep the background in its own asset.

Retain PNG/WebP source frames. GIF has binary transparency: inspect hair/hoodie edges against light, dark and the intended office background. Matte-colored halos must not be accepted as transparency. Do not bake a checkerboard into the image.

## Encode and inspect

Use an installed encoder such as FFmpeg with palettegen/paletteuse, reserving a transparent palette entry. Keep per-frame disposal and loop timing correct; test the exported GIF, not only the source PNG. Use a modest target size appropriate to the UI (often 480–512 px), preserve proportions, and record the encoder and frame timing. No encoder is assumed to exist.

Opening a single PNG and saving it as GIF changes encoding only; it adds no blink, breathing or talking animation. Keep that distinction in the report. Do not add a silent audio file or second voice loop to drive a demonstration.

## Calibrate after final export

Measure the mouth center and maximum extent in the final image:
x=100*centerX/imageWidth; y=100*centerY/imageHeight;
width=100*mouthWidth/imageWidth; height=100*mouthHeight/imageHeight.
For sprite atlases use the dimensions of one cell. Check the jaw center in idle and all blink frames. Reject position changes when switching frames. Do not reuse Lucas percentages on a different crop or character.

Keep small openings subtle, widen only slightly for stressed vowels, and close between syllables. Use fast opening and slower release without leaving the mouth open after stop/interruption. Output amplitude zero must close the mouth even while a speech-start flag is still active. Null amplitude may use a restrained fallback cadence while confirmed speech is active. Reduced motion suppresses decorative head/body motion, not meaningful speech feedback.

## Verify through the installed tool

Read the current scene, import the asset, set_avatar with its calibrated mouth or talkingUrl, then get the same scoped state. Test in actual voice mode on aglit.app. Record open/medium/closed frames from real assistant output, interruption closure, Happy expiry, GIF/VRM replacement and live thumbnails. Preserve original evidence; label isolated rendering tests separately from live audio verification.

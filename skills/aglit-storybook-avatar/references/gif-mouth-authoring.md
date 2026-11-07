# GIF and speech-mouth authoring

## Customize a user-owned copy

Before editing an existing avatar, read its scene and fork its assets and editable sources into a new user-owned version. Never overwrite bundled defaults, another user's assets, or the original source. Keep the prior scene and source files so the change can be reverted.

For a multi-frame GIF or sprite sheet, edit every existing frame affected by the requested change, across idle, talking, listening, thinking and action variants. Prefer a consistent edit to the shared source layers; otherwise extract composited frames, apply the same character edit to each, and reassemble. Preserve frame order, durations, loop count, disposal behavior, canvas alignment and transparent alpha unless the user requested a change. Inspect the complete exported loops for flicker and character drift, not only one representative frame.

For the current Lucas portrait/GIF plus runtime mouth overlay, edit the base artwork and any existing variants, then recalibrate the mouth. A single-frame source has no hidden speech frames to fork. Do not invent missing source layers or claim that re-encoding a still created animation. Switch only the requesting user's scoped avatar after verifying the new assets; preserve unrelated scene and voice settings.

## Idle, listening and thinking states

Author a small coherent animation set alongside lip movement: a quiet idle loop with blinking or subtle breathing, a brief looking-around variation, a listening GIF with an attentive look or slight head tilt, and a thinking GIF with a subtle gaze shift. Keep the same character, transparent canvas, scale and mouth position across every state.

When incoming user text is received, briefly show the listening GIF to acknowledge it. During a voice call, use the existing user-speech detection to show listening while the user speaks; do not start another microphone or use microphone amplitude for the assistant's mouth. When the assistant is actually processing a turn, periodically play the thinking animation with quiet pauses; do not show perpetual thinking while idle. Assistant output takes precedence and uses talking frames or the speech-driven mouth. Stop/interruption closes the mouth and returns to the appropriate listening or idle state. Disable decorative loops for reduced motion.

Inspect the installed renderer and tool schema before binding states. The current public asset contract exposes idle URL, optional talkingUrl, sprite metadata and bounded actions; it does not yet expose automatic listening/thinking URL bindings. A GIF can contain idle variations and supported actions can play clips, but that alone does not implement event-driven listening or periodic thinking. If these bindings are absent, report the runtime extension needed; do not invent tool fields or claim that authoring GIFs wired the behavior. Verify transitions using real incoming text, user speech, processing and assistant output.

## Default expression set

For each newly authored avatar, provide neutral/idle, smiling/Happy, frowning and surprised expressions by default, together with looking, listening, thinking and talking variants. Preserve the requested character across the whole set; do not substitute a generic face. Treat expressions and speech as composable where the renderer supports it: each expression needs a compatible closed mouth and speaking treatment, with aligned eyes, jaw and canvas. A Happy bounce alone is not a smiling expression.

Use smiles for appropriate positive responses, a concerned frown for appropriate negative or empathetic responses, and surprise only when context warrants it. Keep expression changes restrained and bounded, returning to neutral rather than cycling emotions randomly. Offer these defaults without adding UI action buttons. Do not silently regenerate or replace an existing customized avatar just to add missing states; preserve it and create user-owned variants when requested.

Record the authored state files and their intended triggers. Install only fields/actions supported by the discovered schema. Missing expression assets or runtime bindings must be reported explicitly; static artwork, a procedural bounce and documentation alone do not establish that these defaults are implemented.

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

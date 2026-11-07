---
name: aglit-storybook-avatar
description: Create transparent GIF avatars, improve speech-reactive mouths, and customize Aglit voice-mode GIF/image/VRM scenes using an available authenticated avatar tool.
---

Use the existing authenticated employee/chat/device scope and discover the installed `p1_avatar` schema before changing anything. This skill supplies instructions, not an audio transport, MCP connection, or runtime installation. If the tool or a format is absent, report that boundary; do not invent equivalent endpoints or bypass tool assignments.

## Existing character and voice

Preserve the user's approved Lucas character and custom assets. Use `get` first, then the schema's `set_avatar`, `set_background`, `set_voice`, `create_action`, or `play_action` action. Confirm the result with `get` in the same scope. `import_asset` imports a file from the selected runtime device; `download_asset` downloads an authorized public HTTPS asset; `export` provides the scene settings. Never execute downloaded models.

The shared format contract is GIF/image/VRM, with optional `talkingUrl`, `spriteSheet`, and percentage `mouth: {x,y,width,height}`. Preserve original VRM bytes. Raw Live2D model3.json/moc3 assets need a Cubism adapter and are not currently supported by that contract.

Reuse the existing Codex voice harness. Do not create another microphone, TTS service, provider account, or conversation. Mouth movement follows confirmed assistant output, never microphone input. Voice selection is cosmetic and applies at the next call. Web/mobile clients connected to a host use that host's capabilities; phone-local tool availability must be checked separately.

Customization must fork user-owned assets and preserve all existing animation frames. Author neutral, smiling/Happy, frowning and surprised defaults, plus idle/looking, listening and thinking variants alongside speech; read the guide for editing and event-binding requirements. Do not claim unsupported automatic state transitions.

## GIF authoring and mouth quality

For the detailed asset process, read [the GIF and mouth guide](references/gif-mouth-authoring.md).

Keep lossless transparent sources. Match canvas, face placement, baseline and scale across idle, talking, blinking and action frames. Generate/edit artwork with an available image-generation workflow; use media encoders only for assembly and format conversion. Prefer a separate mouth layer or genuinely drawn speech frames over a dark oval placed on top of a baked smile.

A talking GIF is a speech-state animation, not phoneme-accurate lip sync. A single-frame GIF is a still portrait even if the file extension is GIF. Do not claim that conversion created blinking or a speech loop.

The default runtime mouth combines output amplitude with articulation, a quick attack and softer release. Silence gates movement; stop and interruption close it immediately. Keep the head steady rather than moving the entire character to imitate a jaw. Preserve this behavior when authoring adapters or GIF variants.

Known current asset calibrations: male Lucas mouth center (52.7%, 68.1%), maximum width 6%, height 3%; female Lucas center (51.3%, 64.2%), width 7%, height 3%. These values apply only to their matching original canvases. Remeasure after resizing with padding, cropping, changing the face or switching sources.

## Defaults, actions and UI

New generated names use an explicit masculine/feminine catalog; do not infer gender from arbitrary names. Match new profile defaults to the selected catalog and preserve explicit voice choices. Current Codex defaults are masculine Cove and feminine Juniper, subject to the installed voice catalog.

The default backdrop is the bundled office scene. Keep character GIFs transparent and the background separate. The background spans the avatar and live-stream thumbnail area, ending before the standard chat input. Use the normal header and composer. The opt-in person switch sits just above the input and appears only in voice mode.

Happy is a bounded bounce action. Procedural wave/nod/bounce moves the character; it is not a skeletal or facial animation. GIF/image action replacements should preserve mouth calibration or supply their own speech frames. Do not add action buttons or replay expired actions on reopening. Honor reduced motion: disable decorative movement while retaining speech mouth feedback.

## Acceptance

Verify on https://aglit.app through its normal signed-in Console, or the actual requested native app. A component preview does not establish acceptance. Preserve accounts, device identity, trust prompts, OS permissions and other sessions.

Check idle, real assistant speech, silent intervals, interruption, end-call closure, action expiry and asset replacement. Confirm the old asset is restored when appropriate, the input stays standard and live thumbnails remain visible. For speech evidence, capture multiple mouth states during actual output and a closed mouth afterward; one static screenshot does not prove animation. Report source tests, product rendering, audio playback and physical mobile acceptance separately.

# Rolling dev review

Use this as the DESCRIPTION of the one rolling dev PR: the overview at the top of its Conversation tab. Keep the index short and link each screen to its change heading in this same description. Embed actual images/videos and concise E2E results below; links to proof comments alone are insufficient. Comments may contain supporting diagnostics. Preserve other owners' sections and re-read the overview after every update. Replace example fields before publishing. If evidence is unavailable, write **Not captured / not accepted** with the actual blocker, not a dummy URL or a checked box.

| Screen / change | Changed code and commits | Visual evidence | E2E status |
|---|---|---|---|
| Screen name and changed behavior | File/diff permalink and commit | Same-description change heading with embedded media below | Verified / in progress / blocked |

## Change: screen and user-visible behavior

Repeat this section in the PR description for each changed screen or coherent flow. Use a stable descriptive heading so other owners can preserve it.

**Why:** concrete problem and expected result.

**Code:** exact changed-file/diff links and relevant commit(s).

**Try it:** existing authorized dev entrypoint, a few actions, and the expected visible result. Do not include private tokens.

### Before and after

Capture the same screen, viewport/device, account state and interaction point. Use actual product captures; asset previews and mockups are not runtime proof.

| Before | After |
|---|---|
| Embed the baseline screenshot here | Embed the updated screenshot here |
| Baseline source/build and capture context | Updated source/build and capture context |

Use Markdown images, for example `![Before: settings panel](ATTACHMENT_URL)` and `![After: settings panel](ATTACHMENT_URL)`, replacing each URL with the actual supported attachment/evidence URL. Verify previews render for the reviewer. Do not leave example URLs in a submitted entry.

### Recordings for interactions or slowness

Required when still images cannot establish the behavior: animation, focus/navigation, multi-step interactions, loading, reconnect, or performance claims. Otherwise state why static proof is sufficient.

**Before recording:** attach/embed the actual baseline video using the host's supported video attachment flow.

**After recording:** attach/embed the updated video of the same action and comparable state. Verify playback; an inaccessible filesystem path is not an embedded recording.

| Observation | Before | After |
|---|---|---|
| Action / workload | Same defined action | Same defined action |
| Time from stated trigger to visible completion | Observed time, or not measured | Observed time, or not measured |
| Device / network / viewport | Actual conditions | Actual conditions |
| Cache / warm-up | Cold or warm and how established | Comparable state, or explain difference |

State measurement method, repetitions and limitations. Do not infer speed from source changes or advertise incomparable runs as an improvement.

### Actual E2E and regression check

**Tested:** source commit/build, original device/platform, app/browser version when relevant, private origin/source binding, and date.

| Scenario | Action | Observed result | Evidence |
|---|---|---|---|
| Requested behavior | Actual user action | Passed / failed / not run, with concrete result | Image/video or appropriate runtime evidence |
| Adjacent UI regression | Relevant navigation, layout, focus, scrolling or other affected behavior | Actual result | Capture/recording where useful |
| Persistence / restart / reconnect | Relevant save/reopen/reconnect action, or explain not applicable | Actual result | Evidence |

For a live-source setup, show one observed edit-to-screen update and auth/state continuity on every device claimed to support HMR. A successful build, HTTP health check, mock or isolated asset preview is not full acceptance.

### Status and limits

**Review status:** verified / in progress / blocked. List missing before/after captures, untested devices, failed flows and exact external handoffs. Never mark complete while required evidence is missing.

Keep evidence in the PR and approved attachments/storage, with private details excluded or appropriately redacted. Add repository documentation only when the product or developer workflow genuinely needs durable instructions. Default-branch merge and production deployment are not implied by this record.

# Rolling dev PR

Keep the overview short with optional links to individual proof comments. Each owner updates one comment per coherent change. Use the compact format below; normally 100-200 words of explanation excluding media. Replace all placeholders. Use the canonical ticket ID, otherwise the native chat ID, and a real workflow-provided URL. If none exists, write the exact ID and “task link unavailable”; never invent a ticket or URL.

## [TASK_ID](TASK_URL) — screen / change

**Change:** one sentence explaining the problem and result. **Code:** commit/diff link.

| Before — source/build | After — source/build |
|---|---|
| ![Before: screen](ACTUAL_BASELINE_ATTACHMENT_URL) | ![After: screen](ACTUAL_UPDATED_ATTACHMENT_URL) |

**Recording:** actual playable attachment for animation, navigation, slowness or other multi-step behavior. Include comparable before/after clips and observed timings for performance claims. Otherwise explain briefly why still images suffice.

**Tested:** revision/build · device/platform · date · verified source binding.

**E2E:** requested action → observed result; relevant persistence/reconnect and adjacent regression result. State pass/fail/not run explicitly.

**Limits:** concrete remaining failures, missing baseline/video or untested devices. Missing evidence means **unfinished**, not accepted. Old-revision media and local paths do not qualify as current proof.

Keep table rows on single Markdown lines. Put prose, videos and lists outside cells; escape literal pipes. Do not manufacture a missing baseline or replay an uncertain mutation. Preserve private information and authentication/approval handoffs.

Before delivery, open this published comment and verify attachments load/play and demonstrate the named behavior/source/device. Link this exact proof comment in the final response. Upload useful partial captures during validation, not only at the end. Builds/health/text claims are not E2E. Use native request-input for a concrete user-only blocker; retain resource waits as resource waits. No new proof/status documents, automatic merge or production deployment.

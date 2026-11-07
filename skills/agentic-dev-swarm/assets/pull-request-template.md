# Rolling dev PR

Keep the PR description brief: batch purpose, actual acceptance limits, and optional bullet links to proof comments. No shared overview table is required. Use the change section below in one self-contained comment per coherent change; each owner updates their own comment. Putting proof in the overview is also allowed, without duplicating it in both places.

## Change: screen and visible behavior

**Why:** concrete problem and resulting behavior.

**Code:** exact file/diff links and commit(s).

**Try it:** authorized dev entrypoint and a few actions with expected visible results. Exclude private tokens.

### Before

Embed the actual baseline screenshot here, followed by its source/build, device, viewport and state. If unavailable, say **Not captured**; do not invent a baseline.

### After

Embed the actual updated screenshot here with comparable source/build/device/state labels. Use supported attachment URLs and verify the image renders in the published comment. Asset previews and mockups are not runtime proof.

### Interaction or performance recording

Embed a playable recording when stills cannot establish the behavior: animation, navigation, loading, reconnect or multi-step interactions. For a performance comparison, include actual before and after recordings of the same action and workload, observed timing, device/network, cold/warm state, measurement method and limitations. No source-only speedup claims. If missing, say **Not captured / not accepted** and identify the actual blocker.

### Actual E2E

**Tested source/build/device/date:** exact revision, original platform and verified served source or native build.

- **Requested flow:** actions and observed result, with image/video evidence.
- **Adjacent regressions:** relevant layout, focus, navigation or scrolling and actual results.
- **Persistence/reconnect:** relevant save/reopen/reconnect result, or explain why not applicable.
- **HMR:** observed edit-to-screen adoption and auth/state continuity on each claimed device.

Builds, syntax checks, health and mocks do not establish product acceptance.

### Status and limits

**Verified / in progress / blocked:** distinguish implementation from actual acceptance. List missing media, failed flows, untested devices and exact external dependencies. Keep private details out of captures and PR text. Use the current harness's native input tool for a concrete missing user answer or user-only action; final prose saying blocked does not create Needs input. Keep resource waits distinct.

Keep evidence in this PR and supported attachments/storage, not new proof/status documents. Default-branch merge and production deployment remain separate actions.

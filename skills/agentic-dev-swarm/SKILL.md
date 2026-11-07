---
name: agentic-dev-swarm
description: Set up development on one device or multiple authorized devices over a company private network or Tailscale, verify hot reload, and publish one dev branch with code-linked before/after image or video proof in a rolling PR. Use for Agentic Dev Swarm setup and ongoing development in that setup.
---

# Agentic Dev Swarm Setup

Deliver an inspectable edit-to-screen loop: one shared `dev` branch, one rolling PR to the actual default branch, and real visual/E2E proof in that PR. This skill supplies instructions, not a network, credentials or runtime.

## Explain the setup before startup

Inspect repository instructions, existing dev commands, devices, live services and authorized access. Tell the user which checkout/device owns source; which devices run the UI/API/mobile clients; what services, private URLs/listeners or projections will start or be reused; and what remains running for review. Explain which edits HMR, which need a controlled restart, and which native/dependency changes need an authorized build/sign/install. Do not promise every edit can hot reload.

Explain that coherent commits will be pushed to `dev` and visual proof published in one rolling PR. Default-branch merging and production deployment are separate actions. This explanation is not a new confirmation gate for already-authorized reversible work. Obtain missing authorization before enrolling a device, altering network access policy, exposing a public service or performing a protected account/OS action.

## Establish private hot reload

Verify each original host, account, repository and selected device. Honor the repository's canonical source location. For Aglit enrollment/access only, use [Connect devices](../aglit-connect-devices/SKILL.md) when needed; inspect installed tools rather than invent CLI operations. Network reachability does not grant application authorization.

For multiple devices read [private network and HMR](references/private-network-hmr.md). Prefer existing company networking/VPN or a tailnet. Reuse listeners, profiles, ports, identities and accounts. Keep one authoritative source location; runners consume a selected revision or an explicitly scoped, verified live projection. Avoid competing authoring clones and bidirectional sync loops. Do not synchronize secrets, private state, Git metadata, dependencies or build outputs.

Start the narrow existing dev entrypoint. Verify client URL, API/auth origin and live-update transport from every claimed device: remote localhost is the client device, not the source host. Preserve host/origin allowlists, certificates, authentication and WebSocket routing. Use the installed framework/version, without disabling security checks to make HMR connect.

Observe an owned reversible edit or the requested change through watcher rebuild, actual client update and visible behavior. Preserve login, mounted state, selections, saved work and approvals. Processes, HTTP health and source sync alone do not prove HMR. Verify each claimed platform separately; native changes use the repository's authorized update path.

## One branch and one review surface

Detect the actual default branch. Reuse the existing checkout/dev state without switching, resetting or stashing under another writer. Preserve staged, dirty, untracked and unpublished work. All authors work on `dev`, with coherent owned commits and meaningful pushes at least every 15 minutes during active editing and before long validation phases; no empty commits.

Use one shared atomic repository/device-scoped Git mutex for complete short commit/push windows. Preserve other owners' staged work with scoped or isolated-index commits. Separate devices need the same authoritative mutex, not independent locks. Do not hold it across builds, E2E or approval waits.

Maintain one rolling `dev` to default-branch PR, with a short index linking changed screens, code/diffs and evidence comments. Before posting proof and after each push, discover the current OPEN PR for that exact head/base; never reuse a stored PR number without checking its state. If the prior PR merged/closed and pending changes exist, open the next rolling PR automatically within the authorized dev workflow. Each owner updates its own concise change comment instead of overwriting other owners' descriptions. No per-feature review branches/PRs or cherry-pick queues. Keep UI and supporting backend changes together.

Split large work into independent bounded chats only when the user authorizes creating chats. Initial prompts carry scope, nonoverlapping file ownership, device/resource allocation and this convention. All use the same dev branch and PR. No cross-chat status/request/acknowledgement loops: subsequent parent/child/peer/coordinator messaging needs fresh specific user approval. Use existing PR/resource records and the mutex; do not wait for another chat to authorize routine progress. Preserve reservations and serialize physical-device input. Dispatch alone is not integrated acceptance.

## Actual visual proof, directly in the PR

Use the bundled [PR template](assets/pull-request-template.md) for the rolling PR and each coherent change's evidence comment. Replace the example fields with accessible embedded media and actual results before marking the entry ready. A list of changed files or promises to capture evidence is not visual acceptance.

Each UI change entry contains:

- Screen and changed behavior, linked to exact files/diff and commits.
- Actual before/after screenshots of the same screen, viewport/device and comparable state, identifying baseline and updated source/build. Embed usable previews through supported attachments, not just filesystem paths or a plan. Do not use mockups or fabricate a missing baseline; label gaps honestly.
- Short comparable recordings for slowness, animation or multistep behavior. Use the same action, workload and device; label cold/warm state, observed elapsed times and limitations. Still images do not establish improved speed.
- Steps to try it, expected/observed results, tested source/build/device and real E2E gaps. Exercise relevant saves, persistence, restart/reconnect and failure behavior.

Use supported attachments or existing evidence storage with appropriate access. Keep credentials/private account state out of PRs, media, source and logs. Redact sensitive portions without obscuring the tested behavior. Do not add acceptance/status/ownership/proof-plan files to docs just to show activity. Genuine product/developer documentation may change when needed. Report already-merged implementation's validation in the existing PR instead of manufacturing code changes. Nonvisual work needs appropriate behavioral evidence; documentation-only work needs no invented UI proof.

Continue fixing and exercising the actual product until changes are pushed and the PR entry is reviewable with real E2E proof. Syntax, mocks, builds and health checks alone are insufficient. For a genuine external dependency/auth/approval blocker, preserve work, identify the exact blocker and partial result in the PR, and continue independent authorized work. Read owner-authorized private credential instructions before requesting PINs/passwords; use only the exact original device/service credential through the permitted transport. Never guess or bypass handoffs. Do not wait for user review after delivering a tested result.

## Rebase merges and consolidation

Fetch/compare the default branch at resumption, after a rolling PR merges, and before opening/updating the next review. If already an ancestor of dev, leave history alone. Otherwise the default reconciliation is the [safe isolated-index replay](references/safe-replay.md), inside one authoritative short Git mutation window: replay only pending non-equivalent commits onto the current default branch, preserve individual authors/messages, and require the rebuilt final tracked tree to equal the current dev tree exactly. This history-only path never switches the checkout or changes the real index, dirty/staged/untracked files, runtime or another writer's source.

Archive the exact old tip and publish only with an exact remote lease and compare-and-swap local ref update. Recheck local/index/remote/default snapshots before publication. If patches conflict, merge commits need interpretation, the final tree differs, the index changes or a ref moves, stop without publishing the rewrite. Real content reconciliation requires a quiescent-writer maintenance window, explicit conflict review and appropriate product validation; do not reset/stash/force through it or start a messaging loop. A clean history-only replay proves preservation, not feature E2E.

Merge only the user-approved batch using rebase-and-merge, preserving individual commits; no squash or auto-merge. Never rewrite the default branch. Reconcile dev after changed merge commit IDs, preserve remaining work and open the next rolling PR when needed. Remove unwanted code through explicit corrective commits. Dev/proof publication is not production deployment.

Return private dev entrypoints, the rolling PR, observed edit-to-screen behavior per device and remaining limits. Keep intended review services running; release task-owned input/locks and stop only unneeded task-owned helpers. Skill publication, HMR setup, feature acceptance, merge and deployment are distinct results.

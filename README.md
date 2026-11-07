# Aglit Plugin

Connect to your supported computers through Aglit, then discover and register their phones, simulators, and other device control interfaces when the installed runtime supports them.

Start with the [connection guide](skills/aglit-connect-devices/references/connection-guide.md) for computer enrollment. Use the [device integration guide](skills/aglit-connect-devices/references/device-integration.md) for physical iPhone/Android devices, simulator keyboard setup, and registering a vendor app or localhost interface such as a printer controller. The [agent skill](skills/aglit-connect-devices/SKILL.md) routes Claude Code or Codex to the relevant procedure.

## Agentic Dev Swarm Setup (0.4.5)

Use [Agentic Dev Swarm](skills/agentic-dev-swarm/SKILL.md) for development on one device or multiple authorized devices over an existing company private network or Tailscale. The agent first explains hosts, services, private URLs, what can hot reload and what requires a native rebuild, then verifies actual edit-to-screen updates on every claimed device.

Work accumulates on one `dev` branch with frequent pushes and one rolling PR. Review code links alongside real before/after screenshots, and comparable recordings/timings for slowness or interactions. The skill includes a [media-rich PR template](skills/agentic-dev-swarm/assets/pull-request-template.md) with before/after tables, recordings, timing context and regression results. Evidence belongs in the PR, not extra status documents. User-approved merges use rebase-and-merge with individual commits preserved. No per-feature branch bookkeeping or cross-chat status loops. After a rebase merge, the default reconciliation replays pending commits in an isolated index and requires an identical final tracked tree, an unchanged shared index, an archived old tip and exact leased ref updates. Conflicts or moved refs stop the rewrite. Agents discover the current open dev PR after pushes and create its replacement when the previous PR has merged.

Example: "Use $agentic-dev-swarm to start private development on my laptop and test phone, verify hot reload, and publish a dev PR with before/after proof."

For standalone Codex loading, install/copy `skills/agentic-dev-swarm` into the configured skills directory. Both plugin manifests discover it under `skills/`. This is reusable guidance, not an automatic VPN installer or a completed development session. Device/network enrollment, authentication, native builds and production publication retain their authorization boundaries. Package validation does not establish a new multi-device HMR or feature E2E run.

## Install in Claude Code

Run inside Claude Code:

```text
/plugin install aglit-plugin --marketplace Aglit-AI/aglit-plugin
```

Or add the marketplace first, then install:

```text
/plugin marketplace add Aglit-AI/aglit-plugin
/plugin install aglit-plugin@aglit
```

Claude requires a repository, URL, or path when adding a third-party marketplace; the bare `marketplace add aglit` is not a global registry lookup. After adding it, the marketplace is named `aglit` and the plugin is named `aglit-plugin`.

If you installed the earlier preview as `aglit@aglit` or `aglit-device-access@aglit-plugins`, install the new name and disable the older plugin to avoid duplicate skills. Previous GitHub repository links redirect to this repository.

Then ask: “Help me connect to my Office Mac through Aglit.” This is Aglit's own GitHub marketplace; it does not imply an official directory listing.

## Local testing

For Claude Code, load this folder using `claude --plugin-dir /absolute/path/to/aglit-plugin`. Ask: “Help me connect to my Office Mac through Aglit.” Validate the package with `claude plugin validate /absolute/path/to/aglit-plugin` where Claude Code is installed.

For Codex skill testing, copy `skills/aglit-connect-devices` into the configured Codex skills directory, then start a new session and ask to use `$aglit-connect-devices`. The `.codex-plugin/plugin.json` manifest also packages the skill for local plugin distribution; follow the current host's local marketplace installation flow.

This is an instruction-only package. It does not install the Aglit CLI automatically, declare an MCP endpoint, or connect a chat to a user's computer by itself. The target-side Aglit enrollment and account authentication remain necessary.

## Publication status

Public preview package. Official directory approval is separate from GitHub distribution. Commands were checked against installed CLI 1.0.17488; no live enrollment or two-device connection was performed while authoring it.

Peripheral guidance describes the shared registry contract; agents must check the installed runtime schema before using it. Documentation publication is separate from native adapter availability and live-device acceptance. Camera playback and input must be verified separately from displaying an app window.

Before advertising production readiness, validate it in both hosts and exercise enrollment plus an actual target connection on each advertised platform. Keep private SSH conditional until compatible release availability and a real two-Mac session are verified.

The package icon is the public Aglit-AI GitHub organization avatar. This repository contains guidance only, without credentials or the proprietary Desktop runtime.

Suggested listing title: **Aglit Plugin**

Suggested description: **Connect to your Mac or Windows PC, discover attached phones and simulators, and register device control interfaces through compatible Aglit runtimes.**

Example prompts:

- Help me connect to my Office Mac through Aglit.
- Set up my Windows PC so I can access it remotely.
- Find my registered Aglit computers.
- Discover the simulators and USB phones attached to this employee's control host.
- Configure simulator keyboard input and verify it in the Aglit viewer.
- Register my printer's vendor app or localhost control UI as a device.
- Diagnose why my Aglit computer is enabled but disconnected.

## Sources

- Aglit CLI 1.0.17488: shipped README, `aglit --help`, and `src/ssh.cjs`.
- [OpenAI plugin packaging](https://developers.openai.com/plugins/build/plugins)
- [OpenAI submission](https://developers.openai.com/plugins/deploy/submission)
- [Claude manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference)
- [Claude directory submission](https://claude.com/resources/articles/build-plugins-for-claude)

## Storybook avatars (0.3.2)

The [avatar skill](skills/aglit-storybook-avatar/SKILL.md) covers transparent GIF authoring, mouth calibration, output-driven speech movement, voice/background selection, bounded actions, and GIF/image/VRM replacement. Its [GIF and mouth guide](skills/aglit-storybook-avatar/references/gif-mouth-authoring.md) distinguishes real speech loops from still GIF encodings and explains actual-product verification.

This remains an instruction-only plugin. Discover the authenticated `p1_avatar` schema on the selected runtime before invoking it. Phone-local support and raw Live2D models are not implied. Runtime code tests, live audio acceptance, plugin package publication, and directory approval are separate outcomes.

# Aglit

Connect to your supported computers through Aglit, with guided setup, device discovery, Console access, and connection troubleshooting.

Start with the [connection guide](skills/aglit-connect-devices/references/connection-guide.md). The [agent skill](skills/aglit-connect-devices/SKILL.md) teaches Claude Code or Codex how to apply it.

## Install in Claude Code

Run inside Claude Code:

```text
/plugin install aglit --marketplace Aglit-AI/aglit
```

Or add the marketplace first, then install:

```text
/plugin marketplace add Aglit-AI/aglit
/plugin install aglit@aglit
```

Claude requires a repository, URL, or path when adding a third-party marketplace; the bare `marketplace add aglit` is not a global registry lookup. After adding it, the marketplace and plugin are both named `aglit`.

If you installed the earlier preview as `aglit-device-access@aglit-plugins`, install the new name and disable the older plugin to avoid duplicate skills. Previous GitHub repository links redirect to this repository.

Then ask: “Help me connect to my Office Mac through Aglit.” This is Aglit's own GitHub marketplace; it does not imply an official directory listing.

## Local testing

For Claude Code, load this folder using `claude --plugin-dir /absolute/path/to/aglit`. Ask: “Help me connect to my Office Mac through Aglit.” Validate the package with `claude plugin validate /absolute/path/to/aglit` where Claude Code is installed.

For Codex skill testing, copy `skills/aglit-connect-devices` into the configured Codex skills directory, then start a new session and ask to use `$aglit-connect-devices`. The `.codex-plugin/plugin.json` manifest also packages the skill for local plugin distribution; follow the current host's local marketplace installation flow.

This is an instruction-only package. It does not install the Aglit CLI automatically, declare an MCP endpoint, or connect a chat to a user's computer by itself. The target-side Aglit enrollment and account authentication remain necessary.

## Publication status

Public preview package. Official directory approval is separate from GitHub distribution. Commands were checked against installed CLI 1.0.17488; no live enrollment or two-device connection was performed while authoring it.

Before advertising production readiness, validate it in both hosts and exercise enrollment plus an actual target connection on each advertised platform. Keep private SSH conditional until compatible release availability and a real two-Mac session are verified.

The package icon is the public Aglit-AI GitHub organization avatar. This repository contains guidance only, without credentials or the proprietary Desktop runtime.

Suggested listing title: **Aglit**

Suggested description: **Set up and connect to your Mac or Windows PC through Aglit. Find your registered computers, open remote desktop access, and troubleshoot connections.**

Example prompts:

- Help me connect to my Office Mac through Aglit.
- Set up my Windows PC so I can access it remotely.
- Find my registered Aglit computers.
- Diagnose why my Aglit computer is enabled but disconnected.

## Sources

- Aglit CLI 1.0.17488: shipped README, `aglit --help`, and `src/ssh.cjs`.
- [OpenAI plugin packaging](https://developers.openai.com/plugins/build/plugins)
- [OpenAI submission](https://developers.openai.com/plugins/deploy/submission)
- [Claude manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference)
- [Claude directory submission](https://claude.com/resources/articles/build-plugins-for-claude)

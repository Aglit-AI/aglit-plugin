---
name: aglit-connect-devices
description: Connect to the user's computers through Aglit. Use to set up remote computer access, enroll another Mac or Windows PC, find registered devices, open Aglit Console, or troubleshoot a connection. Includes private SSH between supported Macs when available.
---

# Connect to your devices with Aglit

Help the user reach a specific computer they own or are authorized to use. Distinguish the **target computer** being accessed from the **controller** where the user or agent starts the connection. Running setup on the controller does not enroll a different target.

## Check the environment

Read [the connection guide](references/connection-guide.md) for commands, platform limits, troubleshooting, and verification. This skill was checked against `@aglit-ai/cli` 1.0.17488. Run `aglit --help` before relying on commands in a different version.

The documented target platforms are Apple silicon macOS and Windows x64 with a compatible published Desktop runtime. Do not promise Linux, Intel Mac, Android, iOS, or arbitrary IoT device enrollment. A browser controller is distinct from a supported target; this skill does not establish phone-browser compatibility.

If a terminal executes in a cloud sandbox, do not install Aglit there and describe it as the user's computer. Supply the target-side commands for the user to run locally. This package supplies instructions, not a remote transport or MCP endpoint.

## Choose the connection

- For desktop access, enroll the target, then use its returned `employeeUrl` in Aglit Console signed into the owning account.
- For an already registered device, inspect the accessible device list and select the intended computer. Do not re-enroll unnecessarily.
- For remote shell access, use the conditional macOS private SSH workflow in the guide. A command appearing in help does not prove the installed runtime supports it.

## Complete and verify

Carry out the requested setup where a local terminal is available. Let the user complete browser sign-in and native OS permission prompts. Preserve their account and security preferences. A request to connect does not authorize disabling two-factor access, accepting unsigned software, sharing the device with a team, or uninstalling it.

Report the target name, the connection URL or selected device ID, the connection method, and the actual verification performed. Registration, an enabled flag, a connected tunnel, a working desktop session, and a successful remote command are different milestones. Only claim those observed. If blocked by user sign-in, native consent, an unsupported platform, or unavailable runtime support, state the specific next step.

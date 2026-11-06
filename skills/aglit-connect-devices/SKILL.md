---
name: aglit-connect-devices
description: Connect to computers and their registered devices through Aglit. Use for Mac or Windows enrollment, physical iPhone or Android connections, simulator discovery and keyboard setup, registering a vendor app or localhost device UI, and connection troubleshooting. Device adapters require a compatible authenticated runtime.
---

# Connect to your devices with Aglit

Distinguish the **controller**, the **original control host**, the **employee**, and the **registered device**. Enrolling the controller does not enroll another computer. A phone or simulator is attached to an existing control host; an arbitrary peripheral can expose its controls through a vendor app or localhost UI.

## Choose the workflow

- For computer enrollment, Console access, or conditional private SSH between Macs, read [the connection guide](references/connection-guide.md). Its CLI examples were checked against `@aglit-ai/cli` 1.0.17488; inspect the installed help before using another version.
- For physical phones, existing or new simulators, or registering a printer/drone/other device, read [the device integration guide](references/device-integration.md). It includes discovery, input setup, the shared interface, registration examples, and viewer URLs.
- For an existing registration, reuse its exact employee/device identity instead of enrolling a duplicate.

Computer installer support and peripheral adapter support are separate. The documented computer installer targets Apple silicon macOS and Windows x64 with a compatible Desktop release. iOS/Android peripherals run through their supported original host adapters; this does not establish standalone phone or arbitrary IoT runtime enrollment.

## Check the available tools

This plugin supplies instructions, not a transport or MCP endpoint. Use only the authenticated tools actually available for the selected host. Read the installed `p1_employee` schema before using its device actions. When those actions are absent, check the compatible runtime version or use the existing Console Equipment flow; do not invent equivalent CLI commands, raw endpoints, or credentials.

If a terminal is in a cloud sandbox, do not install Aglit there and call it the user's computer. Use an existing authorized host connection or explain the target-side step.

## Complete and verify

Use the existing private tunnel, device trust, and revocation. Preserve native sign-in, USB trust, OS permissions, other tasks' sessions, and active equipment jobs. A localhost address or device ID selects a target; it grants no authority.

Expand the selected device before input. Verify the intended live surface and a harmless declared action with visible readback. Report discovery, registration, live viewing, input, and hardware actions separately. A visible interface, icon, or camera thumbnail does not prove its controls work. If only viewing succeeds, say so.

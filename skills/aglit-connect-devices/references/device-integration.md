# Integrate simulators, physical phones, and other devices

Use this guide after the original control host is enrolled and the account can access it. This is guidance for compatible device-registry runtimes, not a claim that every published Desktop release contains these adapters. Inspect the installed tool schema and setup guidance first.

## Discover and assign

An employee can have many registered devices and can have no primary laptop. Each adapter still needs its authorized private control host. Keep the persistent employee ID, registered device ID, and host/runtime ID distinct; a simulator UDID, USB serial, display name, or shared assistant template is not an employee identity.

With an authenticated Aglit tool connection, use these `p1_employee` actions only when present in its schema:

| Action | Purpose |
| --- | --- |
| `list_devices` | Read accessible employees and each employee's registered devices. |
| `discover_devices` | Inspect the current original host for supported native devices and return setup guidance. |
| `attach_device` with `device_id` | Attach the exact discovered device to the current employee. |
| `detach_device` with `device_id` | Forget that peripheral registration; this does not delete its simulator or revoke USB trust. |
| `list_device_interfaces` | List live native app/window interfaces for another kind of device. |
| `register_device` | Register or update that exact interface under a stable logical device key. |

These are tool arguments, not shell commands. For example, pass `{"action":"discover_devices"}` to the installed `p1_employee` tool. Inspect its schema for required reason/context fields and use the caller's real authenticated context.

In Console, the equivalent flows are **Agent Settings → Equipment → Phone or existing simulator** and **Register another device**. Discovery inspects supported devices on that host; it does not discover every network appliance, boot a simulator, pair a phone, or configure its input driver. Read the returned setup guidance, attach the exact result, and confirm it with a fresh `list_devices` read.

## Physical iPhone and Android

For iPhone, connect it to its original control Mac, unlock it, and complete the device's Trust/pairing prompts. Use the installed iPhone bridge's instructions for capture, developer/automation permissions, and input prerequisites. Do not assume USB detection alone supplies interactive control or install a second bridge over an active one.

For Android, connect to the intended original host and complete USB debugging authorization for that host. The adapter needs the exact authorized ADB serial. Use installed Android platform tools and the runtime's capture/video setup guidance; do not expose ADB over the network or substitute an emulator's port. If a phone is unauthorized or offline, resolve that state on the original device before attaching it.

Refresh discovery after setup and read back the resulting capabilities. Use `p1_iphone` or another native tool only if its installed schema supports that exact target; a physical-phone selector must not receive a simulator UDID. Otherwise use the registered viewer. Preserve other tasks' phone reservations and accounts.

## Simulators and emulators

Inventory existing devices, boot state, ownership, memory, and disk first. Reuse an idle configured runtime. Create a simulator only when needed for the task, using an installed compatible runtime and recording its exact UDID or AVD/serial. Start only the required few devices; an offline definition is not a running simulator. Preserve another task's active runtime, data, and companion.

### iOS keyboard and input

On the original Mac, use Xcode's Simulator tooling to choose or create the intended simulator. In Simulator.app, check **Connect Hardware Keyboard** and **Send Keyboard Input to Device** when using GUI keyboard input.

For Aglit's idb-based simulator adapter, screenshots from `simctl` are not an input driver. The installed bridge needs an idb client and a verified companion for that exact uppercase UDID, configured through `AGLIT_IOS_SIMULATOR_IDB_PATH` and `AGLIT_IOS_SIMULATOR_COMPANIONS`. Follow the serving runtime's configuration format; verify companion identity, runtime, and geometry. Keep its existing Unix-socket or loopback transport. Source-checkout helper scripts are not assumed to ship in the public CLI.

A GUI preference is not proof that the bridge can type. After configuring the adapter, refresh discovery and reattach the same registration so its advertised capabilities update. A simulator may correctly remain view-only when input is unavailable.

### Android emulators

Use the installed emulator manager to select or create the intended AVD. Boot that exact AVD on the original host, match it to its actual ADB serial, and use the same discovery/attach flow as a physical Android device. Verify authorization and the installed capture/input backend rather than inferring identity from a port number.

### Verify setup

In the actual viewer, expand the device and verify text containing spaces and punctuation, Backspace, Enter, Tab, tap/click, drag/scroll, and each declared device button with settled visible readback. Use a harmless test surface. Inspect the result before retrying timed-out input. Release reservations and stop only task-owned runtimes or companions. Detaching a registration does not shut down or delete the underlying simulator.

## Register an arbitrary device interface

A printer, drone, or other peripheral can use an existing vendor app. For a device exposing only an API, build a minimal localhost UI with a full-screen video/status area and controls, then open it in a dedicated native browser window on the original control host. Keep the device API and authentication behind that local UI.

First connect or pair the real device through that surface and verify its selected identity and live status. Then:

1. Call `list_device_interfaces` and select the exact live `app_id`, `window_id`, and returned `control_interface`. Use the numeric native window ID returned as a string.
2. Call `register_device` with those values, a stable `device_key`, a recognizable `display_name`, and `kind: "printer"` or `"peripheral"`.
3. Read back `list_devices`, select the device in **Assigned device**, expand it, and verify live media plus its declared input.
4. After an app relaunch or window replacement, register the new exact window using the same logical `device_key`. Do not accumulate duplicate registrations.

Example tool arguments, after replacing the app/window placeholders with fresh discovery values:

```json
{
  "action": "register_device",
  "device_key": "workshop-printer",
  "display_name": "Workshop printer",
  "kind": "printer",
  "app_id": "<app_id from list_device_interfaces>",
  "window_id": "<window_id from list_device_interfaces>",
  "control_interface": {
    "inputs": [
      "focus", "move", "mouseDown", "mouseUp", "mouseDrag",
      "click", "drag", "scroll", "type", "key", "clipboard"
    ],
    "actions": []
  }
}
```

Retain only inputs implemented by the chosen adapter. The current generic app adapter supports these desktop events; it does not implement native multitouch gestures, file drop, or held-key events. The viewer's pinch gesture zooms the displayed image locally.

Custom toolbar buttons use `{id, label, icon, event}`. Their event is a verified `type: "key"` press, with optional `modifiers` (`shift`, `control`, `option`, `command`) or `repeat`. If supplied, `keyEvent` is `"press"`. For example, a localhost UI that implements an unmodified R key to refresh status can declare:

```json
{
  "id": "refresh-status",
  "label": "Refresh",
  "icon": "refresh",
  "event": { "type": "key", "key": "r" }
}
```

Test the real key handler before advertising the action. Available icons include `home`, `back`, `power`, `volume-up`, `volume-down`, `camera`, `play`, `pause`, `stop`, `refresh`, `settings`, and `custom`. An icon does not implement an operation; leave actions empty when no mapping has been verified. Power or motion buttons require actual device-specific implementation and authorization.

For Bambu, use the already paired Bambu Studio window (`com.bambulab.bambu-studio`) and verify the selected printer and current status. There is no special Bambu tool to install. Preserve active prints. Verify camera playback separately from the app-window stream, and test a harmless navigation action before claiming input works.

Registration reuses the existing authenticated exact-window capture/input and private tunnel. It registers the visible window, not an arbitrary raw URL proxy, and grants no additional device or account access.

## Open the selected employee and device

Prefer the control link returned by the runtime. Where the installed UI supports this convention, append the following path to the Console base injected/configured for that deployment:

```text
/devices/<nativeRuntimeId>/chats?employeeId=<employeeId>&registeredDeviceId=<registeredDeviceId>&desktopFullscreen=1
```

URL-encode IDs. The path identifies the native control runtime; `employeeId` identifies the persistent employee and `registeredDeviceId` the assigned peripheral. For a laptop-backed employee, `list_devices` supplies the native route ID as `device_id`. With no primary laptop, retain the authenticated runtime URL or use the backend-provided control link; do not invent a route from the employee ID or peripheral serial.

Use the deployment's actual Console base or returned `employeeUrl`, including for Dev/self-hosted instances. Never hard-code a production domain or put tokens, access codes, credentials, or private transport addresses in a viewer link. The link still requires normal sign-in, device trust, and assignment checks. It does not create a chat turn or boot a device.

Collapsed views show selection and expansion; expand before input or custom actions. If registration, playback, input, or camera access fails, report the failing step and observed runtime/schema. Installing this documentation does not update a native runtime, supply missing drivers, or prove end-to-end control.

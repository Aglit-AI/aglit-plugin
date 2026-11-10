# Aglit device connection guide


## Self-hosted account server and private networking

Use the company Aglit HTTPS address in **Account server** on Desktop or **Use a company server** on Mobile. A saved choice overrides the app's environment default. Use the same server on the computers and phones that should share an account.

For most cloud, VPC, and VPN deployments, leave the server on its default outbound relay. Put one HTTPS reverse proxy or load balancer in front of the server's loopback listener. Desktops connect to that endpoint after the user enables Anywhere Access and completes OS approval. Phones connect to the same endpoint. There are no per-computer DNS records or inbound listeners in relay mode.

On a server release with transport discovery, operators can set `AGLIT_APP_ORIGIN=https://aglit.example.com` and `AGLIT_DEVICE_TRANSPORT_PROVIDER=relay|cloudflare|tailscale|custom`. Cloudflare and custom VPN/VPC deployments use an operator-configured device HTTPS hostname template. The network operator can publish a private CNAME to a reachable hostname, or an A/AAAA record for an address. The HTTPS certificate must cover the hostname the Aglit server actually uses.

For Tailscale device transport, enable MagicDNS and HTTPS certificates for the tailnet. Set `AGLIT_TAILSCALE_DOMAIN` to the tailnet's actual `.ts.net` suffix. Sign in to Tailscale on the server and each participating desktop using the normal installation flow. Allow the Aglit server to reach the desktops on the advertised Serve port. On a compatible Desktop, enable Anywhere Access. The app configures a private Serve listener, registers its address with the server, and refreshes it automatically. An incompatible listener on the same port or missing local Serve permission appears as a setup error.

For a custom **account server** hostname such as `aglit.example.com`, add an A/AAAA record in your private DNS zone pointing to the Aglit reverse proxy's tailnet address. Configure that zone's nameserver as a restricted nameserver in [Tailscale DNS](https://tailscale.com/docs/reference/dns-in-tailscale). Install a TLS certificate valid for the custom name on the proxy. Tailscale Serve's own certificate covers the MagicDNS hostname.

On the server host, run:

```sh
tailscale up
tailscale status
tailscale ip -4
```

If you use Caddy for the custom hostname, install its certificate and key on the proxy, then use a site block like this:

```caddyfile
aglit.example.com {
    tls /etc/aglit/tls/fullchain.pem /etc/aglit/tls/privkey.pem
    reverse_proxy 127.0.0.1:59090
}
```

Validate and start the proxy with `caddy validate --config /etc/caddy/Caddyfile` and `caddy run --config /etc/caddy/Caddyfile`, or reload the existing Caddy service after validation.

After setting the private DNS record and starting the HTTPS proxy, check from another enrolled tailnet device:

```sh
dig +short aglit.example.com
curl --fail https://aglit.example.com/health
```

On a desktop, `tailscale serve status` can confirm the app created the listener. Verify Anywhere Access from a second device by opening the enrolled desktop and completing its usual verification. Repeat after restarting the desktop app to confirm recovery.

## What connects to what

Install Aglit Computer Use on each supported computer you want to reach. Setup registers that computer to your signed-in Aglit account and enables Anywhere Access. Connect through Aglit Console using that account. Repeat enrollment on each additional target; use distinct names such as “Office Mac” and “Home PC”.

This guide reflects the installed CLI 1.0.17488 README, help, and SSH implementation, inspected November 6, 2026. Desktop availability is independent of CLI publication. The documented installer supports Apple silicon Macs and Windows x64; a qualified runtime release is required. Other target platforms are not established by these sources.

## Enroll a computer

Run in a terminal **on the target computer**, with Node.js 22 or later and pnpm available:

```sh
pnpm add -g @aglit-ai/cli
aglit init
```

The guided flow asks for a computer name and provider, opens Aglit's Google sign-in, requests required OS permissions, and enables Anywhere Access. It detects existing Codex/Claude logins and asks when multiple usable providers exist. A detected login is not proof that model inference works.

To supply the name and provider explicitly:

```sh
aglit init --name "Office Mac" --provider codex
```

Use `--provider claude` when that is the user's choice. On another target, choose its own name. Sign into the same owning Aglit account to collect personal computers under that account.

For non-interactive callers:

```sh
aglit init --json --name "Office Mac" --provider codex --account owner@example.com
```

Replace the example email with the user's actual account. `--account` checks the signed-in identity; it does not sign in as that address. JSON mode skips prompts but does not bypass first-time login or OS consent. On macOS, grant Screen Recording and Accessibility to the Aglit app when requested. If consent is denied, remote access stays disabled; grant it in System Settings and rerun setup. A screen-recording grant may require an app restart.

Windows releases can require explicit unsigned-release acceptance. Explain the installer result and let the user choose before using `aglit install --accept-unsigned-windows`; do not put that opt-in in the default setup command.

## Connect through Console

Successful registration returns a direct Console link as `employeeUrl` in JSON output. Use that actual URL and deployment origin rather than fabricating a domain or device/employee ID. Open it on the controller and sign into the owning account. Complete any requested security-code challenge through the supported UI.

On a configured local installation, these commands open Console or inspect accessible computers:

```sh
aglit console
aglit pipelines devices
```

A device appearing in this list does not prove its pipeline or desktop session is connected. If there are multiple similarly named computers, resolve the intended target before acting.

On the target, inspect or manage access:

```sh
aglit status --json
aglit ui
aglit enable
aglit disable
```

`status` distinguishes local access enablement from tunnel connectivity and reports permissions/provider state. Verify a desktop connection by observing the intended target's live view and, when authorized, a harmless interaction. Do not report end-to-end control merely because setup succeeded.

Closing the Desktop window leaves Aglit running in the background. Explicitly quitting from its tray/menu stops the app. A sleeping, offline, or stopped computer may require local attention.

## Phones, simulators, and other peripherals

After enrolling the control host, use the [device integration guide](device-integration.md) for discovery and assignment, iOS simulator keyboard/idb setup, Android USB authorization, and registering a vendor app or localhost device UI. These capabilities require the corresponding installed runtime schema and adapters. They do not imply standalone iOS, Android, or IoT computer enrollment.

## Private SSH between Macs — conditional

The installed README labels this a source implementation requiring a compatible native release and a real two-device acceptance run. The CLI implementation restricts outgoing connections to macOS and documents Windows SSH as unavailable. Treat this as conditional until the actual runtime and a connection succeed.

On each Mac, after enrollment:

```sh
aglit ssh status --json
aglit ssh on
```

Only enable SSH when requested. On the target, obtain its device ID and SHA-256 identity fingerprint from local status or another trusted channel. On the controller:

```sh
aglit ssh list
aglit ssh DEVICE_ID --fingerprint VERIFIED_SHA256
```

Replace both placeholders with verified target values. The CLI expects the fingerprint as 64 lowercase hexadecimal characters. Discovery is for selection; independently verify the identity before connecting.

For an authorized, harmless identity check:

```sh
aglit ssh DEVICE_ID --fingerprint VERIFIED_SHA256 --command "hostname"
```

If the target requires an Anywhere Access security code, connect interactively and enter the code displayed on that target. Codes are not printed in its terminal. The prompt requires a logged-in graphical desktop session. Do not turn off two-factor access to work around a prompt.

This path uses Aglit's authenticated private transport; it does not require router port forwarding or enabling macOS System Remote Login. Preserve strict host verification. Disable SSH on a computer with `aglit ssh off` when the user requests it.

## Troubleshooting

| Observation | Next step |
|---|---|
| `aglit` is missing | Install the CLI on the intended supported computer; confirm Node.js 22+ and the package-manager binary path. |
| Unsupported platform or no qualified Desktop release | State the platform/runtime limit. Installing the CLI alone does not add support. |
| Sign-in or native permissions incomplete | Have the user complete that local step, then rerun `aglit init`. |
| Wrong signed-in account | Rerun interactive `aglit init` and use its account step. Account switching revokes the old cloud access. |
| Enabled but no connected tunnel | Inspect target runtime, network, and account status; do not call it a working session. |
| Runtime stopped | Open it with `aglit ui`, then recheck status. |
| Command missing from the runtime | Inspect `aglit update --check`; update when appropriate, then retry. CLI and native runtime releases are distinct. |
| Browser extension missing | Run `aglit browser status --json`, then `aglit browser setup` if browser control is needed. Enrollment can succeed without extension setup. |
| Browser setup succeeds but control does not | Extension installation is separate from browser-control connectivity; verify an actual session. |
| SSH unavailable or unknown command | Check compatible macOS runtime support; use Console desktop access where available. |
| SSH fingerprint mismatch | Stop and verify the target's identity locally or through a trusted channel. |
| Login opens an existing ordinary Aglit Desktop | The README documents a shared-port coexistence limitation. Report it; do not invent a callback-port workaround or terminate unrelated sessions. |

## Ownership and access boundaries

Setup creates a Personal computer. Team invitations and membership do not automatically share it. `aglit teams init` and team invitations are separate workflows and do not prove an authoritative team-device access binding.

Installing this skill in ChatGPT or Claude does not install a runtime on another computer, create an authenticated MCP connection, or grant access to the user's devices. Use only tools actually configured for the selected target; do not assume an existing Aglit MCP integration points at that target.

`aglit uninstall` revokes access and purges this installation's private state. It is not ordinary connection troubleshooting. Use `aglit disable` to stop Anywhere Access when that is the requested outcome.

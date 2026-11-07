# Private network and HMR

Use when development spans devices/private proxies. Inspect installed tools and repository configuration before choosing settings. This guide does not authorize network enrollment or company policy changes.

## Smallest useful topology

- One device: reuse loopback dev/watch services and the existing app/browser profile.
- Multiple devices: serve frontend/API from one authoritative source host over the existing private network, including the live-update transport.
- Platform runner: use existing source-sync or an explicit revision projection and a native dev client. Record its actual source/renderer binding. Desktop UI, mobile JavaScript, embedded WebView and backend can have different watchers.

Explain hosts, services, private URLs, source binding, listener lifetime and native rebuild limits before startup. Do not silently install a VPN, join a tailnet, enable subnet/exit-node routing, alter DNS/access policy or create a public tunnel. Use authorized routes or request the exact missing action.

## Company network or Tailscale

Inspect identity, reachability, current access rules and service ownership using installed tools/help. Prefer established private networking/proxies. Tailscale Serve can privately front a loopback service within a tailnet; inspect existing mappings before changes. Tailscale Funnel is public exposure and is outside this private setup. Retain app/device authorization.

Keep listeners on loopback behind the private proxy, or on the explicitly selected private interface required by the project. A broad public bind is not a default shortcut. Verify clients can reach the chosen origin; source-host loopback URLs are wrong in remote clients. Preserve other work's listeners/routes.

## Verify live updates

Use installed Vite/Next/Metro/backend configuration, not settings pasted from another version. Proxies must carry live updates as well as page/API traffic. For Vite inspect its version's host/origin allowlist and WebSocket client-address options; allow the intended private hostname, not every host. A page loading with a failed update connection is not HMR acceptance.

Check watched paths, symlinks/projections, client cache/source binding, certificates, proxy upgrades, origin-scoped cookies and the receiving app. Observe an owned edit through rebuild and visible adoption; restore diagnostic edits without discarding other work. Native dependencies, entitlements and compiled code need the authorized update path. A mobile dev shell must actually receive its JavaScript/renderer source.

Exercise the feature after the edit on each claimed device, recording source/build, visible result and auth continuity in the PR. Timing/interaction claims need comparable recordings. If only one device was tested, say so. Preserve sessions/prompts; do not disable the user's private network or another task's runtime to test privacy.

## Primary references

- [Vite server configuration](https://vite.dev/config/server-options): installed-version host/origin and update-transport settings.
- [Tailscale Serve](https://tailscale.com/docs/features/tailscale-serve): privately front a local service in a tailnet.
- [Serve CLI](https://tailscale.com/docs/reference/tailscale-cli/serve): current syntax and mappings.

These explain tools, not this user's enrollment/permissions or a successful multi-device session.

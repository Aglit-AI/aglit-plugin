# Simulator development, channels and host admission

These are agent procedures for compatible installed runtimes, not drivers or a promise that runtime admission is implemented. Read the selected project's current instructions and installed tool schema first. This plugin does not ship the source helpers named below.

## Inventory and reserve before booting

Identify the original host, host architecture, Xcode/Android SDK version, exact physical-device identity or simulator UUID/AVD, runtime version, boot state, installed bundle IDs and current owner. A simulator, a phone-local employee profile and a physical phone are different things. Count **all booted simulators/emulators on the host**, including other tasks' devices. Do not take over a production app, erase a profile or stop another owner's device to make room.

Acquire the project's existing atomic per-host resource/device lease before a boot or emulator launch. Under that same lease, refresh the inventory and resource readings, admit one device, record its identity/owner/budget, then boot and verify the result. Recheck after boot and release only your reservation on failure. Discovery and input adapters may require an already-booted simulator and cannot enforce this boot policy themselves. If no shared atomic lease facility exists, serialize through the host owner; do not create independent per-chat locks or start parallel devices while admission is uncertain.

### Conservative configurable agent policy

Until a project supplies measured budgets, use the following **provisional manual admission policy**, not a hardware guarantee or a claim of automatic enforcement:

- Reserve at least **8 GiB RAM and 2 logical CPU cores** for the OS, controller and existing services.
- Budget at least **4 GiB RAM and 2 logical CPU cores per active emulator/simulator**. Raise these for large device profiles, Android heaps, graphics workloads or measured peaks; never lower them merely to pass admission.
- A planning ceiling is `max(0, min(floor((physicalRAMGiB - 8 - otherReservedGiB) / 4), floor((logicalCores - 2 - otherReservedCores) / 2)))`. For example, an otherwise idle 16 GiB/8-core host plans at most two active devices. This is a ceiling, not permission to boot two.
- Also require at least 4 GiB currently available RAM after the estimated new allocation, noncritical OS memory pressure, and enough disk for the selected runtime's normal guard. Defer a new boot while a native build or another reserved workload consumes that margin, or sustained host CPU exceeds 80%. Never substitute these illustrative thresholds for a stricter project guard.
- Count existing booted devices against the ceiling even when they are idle or belong to someone else. Reserve concurrent builds separately. Do not stop foreign devices; queue work or choose another authorized host.

Record the chosen configuration and observed measurements with the lease. These numbers are conservative starting policy introduced by this guide, **not benchmark-derived safe maxima**. Calibrate upward from actual peak usage. The audited Aglit iOS discovery/input adapters do not boot devices or implement a device-count limiter; the audited Android development launcher can start the first AVD without this resource reservation. Provider capacity/CPU-pressure metrics and build disk guards are separate and must not be described as a hard simulator cap. Missing runtime enforcement remains an implementation gap.

## Set up and prove input

Use the installed source simulator setup procedure when available; do not assume this instruction-only plugin installs `idb`, its companion or private helper scripts. Match the idb client and companion to the exact simulator UUID and SDK. Prefer an existing Unix socket or loopback transport. Never restart a shared physical-phone bridge to fix simulator typing.

Enable Simulator's **Connect Hardware Keyboard** and **Send Keyboard Input to Device** where required. In a task-owned harmless field, verify letters, spaces, punctuation, Backspace, Enter, Tab, tap, drag and scroll with visible readback. A screenshot or successful click acknowledgement does not prove text delivery. Do not replay a mutation whose completion is uncertain. See [device integration](device-integration.md) for registration and input details.

For a physical iPhone blocked specifically by an XCTest automation approval prompt, compatible Aglit source installations may provide:

```text
node <authorized-source-root>/packages/desktop-app/scripts/iphone/host-ios-device-client.js authorize-xctest <exact-udid>
```

This is a **source-only, version-dependent helper**, not a public `aglit` CLI command. Confirm that the installed helper documents and implements protected stdin input, exact original host/UDID checks and a fresh native approval prompt. Read the owner's authorized credential instructions before asking again. Transfer only the exact device credential through the documented protected stdin/opaque handoff; never place it in argv, ordinary tool arguments, logs, screenshots or repository files. Do not invent an equivalent endpoint if the helper is absent. Preserve unrelated approvals and accounts. Check the subsequent actual source/input result: an immediate helper timeout is not proof that the credential was rejected, and does not authorize replay.

## Install genuine Dev and Staging side by side

Use the project's official channel preparers and registrations. For Aglit the distinct registered bundle IDs are `ai.aglit.mobile.dev` and `ai.aglit.mobile.staging`; verify current manifests rather than inferring a channel from its display name. Keep production installed and preserve its app data, accounts and keychain groups. Do not rename a production app or copy its tokens to manufacture Dev/Staging.

- A Simulator needs a genuine **simulator** Mach-O slice for the host/runtime architecture, compatible native components and a simulator build of the selected channel. A device IPA or TestFlight build is not a simulator artifact. Do not assume an arm64 simulator build works on an Intel host.
- A physical phone uses the authorized signed device distribution path. TestFlight requires the correct App Store Connect app, uploaded/processed build and tester/group eligibility or a real enabled invitation. A Google callback registration, local development provisioning profile or production TestFlight listing does not establish Dev/Staging TestFlight availability. If no channel build is offered, report the missing distribution step; do not downgrade production or issue invitations/publish builds without authorization.
- Verify each installed artifact's actual bundle ID, signature/entitlements, API/auth origin, Google callback roles, native architecture/platform and emitted JavaScript source. Dev and Staging must have separate registered callback and storage scopes. Use normal account sign-in and protected OS/account handoffs; never copy production auth state.
- Dev may use its registered compatible Metro producer. Staging should consume its official staging API and bundled selected-master JavaScript unless the project explicitly defines another supported mode. A Debug simulator that silently falls back to localhost:8081 is not Staging acceptance. Use the official Release/bundled configuration where required.

Record actual installation and launch separately from source preparation or a queued build. Do not claim an unmerged Dev feature is present in a master-based Staging app.

## Verify the actual JavaScript update mechanism

Distinguish runtime-UI Vite HMR, React Native Fast Refresh and a guarded complete-bundle reload. Aglit's compatible development shell can poll for an admitted full bundle; a 15-second poll serving `dev=false`/`hot=false` is **not Fast Refresh**. Describe the mode actually observed.

Check the registered producer's canonical source/projection, emitted JavaScript, native compatibility fingerprint, public API/auth origin and channel callbacks against the installed app. A healthy Metro `/status` does not prove adoption. Native fingerprint mismatches must reject the update. A reviewed source-only compatibility layer must pin the actual compiled baseline and exact approved deviations with before/after input checks; never stamp an old hash onto arbitrary new native source or disable the guard.

Reuse the existing registered producer/listener. Verify a reversible source edit through actual emitted bundle and receiving-app adoption while preserving the account and native process where supported. Native ABI/dependency/entitlement changes still require a genuine build. JavaScript-only edits do not justify repeated manual reinstalls when the compatible registered producer can deliver them normally.

## Proxy and transport scope

Use only the existing authenticated private network/proxy. HTTP reverse proxies must preserve the intended origin and WebSocket upgrade; a SOCKS proxy is not interchangeable with an HTTP proxy or a device registration. Do not assume Simulator or native networking honors browser/environment proxy variables: verify the installed runtime's supported configuration and actual page, API and live-update paths separately.

For Android, scope ADB to the exact authorized serial. Where the existing project uses `adb -s <serial> reverse tcp:<port> tcp:<port>`, inspect and preserve existing mappings; it exposes a host service to that device, not a general private network. Do not enable network ADB or broad public listeners as a shortcut. Apple's RSD/pairing transports require the supported original-host toolchain and device trust; this guide supplies no generic RSD proxy or pairing bypass. Missing HTTP/SOCKS/RSD support must be reported explicitly, not invented.

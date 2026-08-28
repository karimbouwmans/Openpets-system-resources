---
name: openpets-system-resources
description: Build and maintain the OpenPets system-resources plugin (CPU, RAM, GPU, SSD HUD plus local metrics sidecar). Use for plugin changes, sidecar, LaunchAgent, or unpack-load in OpenPets.
---

# OpenPets system-resources

## Plugin

- Manifest: `openpets.plugin.json` (`openpets.system-resources`, SDK v3). License: MIT.
- Entry: `index.js`. HUD via satellite-pet links of the default pet (`pets.spawn` + `speak({ pin: true, hud })`). Never pin on the default pet: that overwrites Virtual Pet.
- CPU/RAM from `ctx.system.metrics`. GPU/SSD from sidecar GET `http://127.0.0.1:37647/metrics?platform=`.
- Sidecar fetch always in try/catch; missing meters as `null` / label `—`. Status line gets `sidecar offline` when GPU/SSD do not arrive. Fetch timeout 2500ms.
- GPU/SSD need the sidecar. OpenPets plugin load does not install it. First time: `npm run install-sidecar` (detects mac/linux/win). Check `curl -s http://127.0.0.1:37647/metrics`.
- Config `sidecarHint` (text, default `npm run install-sidecar`) is a Configure hint only; `readConfig` ignores the value.
- Config `os`: `auto|mac|windows|linux`. Auto follows `ctx.system.info().platform`.
- Languages: `locales/{en,nl,fr,de}.json`. Config `language` overrides host locale for HUD/speech.

## Sidecar

- `sidecar/collect.mjs` + `sidecar/metrics-server.mjs`. Bind only `127.0.0.1:37647`.
- macOS: LaunchAgent. Linux: systemd --user + linger. Windows: Scheduled Task AtLogOn. Dispatcher: `scripts/install-sidecar.mjs`.
- GPU: ioreg (mac), nvidia-smi / GPU Engine (win), nvidia-smi / sysfs (linux). SSD: df / WMIC / df.

## Test

```bash
npm test
```

Then sidecar health: `curl -s http://127.0.0.1:37647/metrics`.

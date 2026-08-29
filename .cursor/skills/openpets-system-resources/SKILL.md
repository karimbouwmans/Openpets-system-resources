---
name: openpets-system-resources
description: Build and maintain the OpenPets system-resources plugin (CPU/RAM catalog HUD, optional GPU/SSD sidecar). Use for plugin changes, catalog ZIP, sidecar, LaunchAgent, or unpack-load in OpenPets.
---

# OpenPets system-resources

## Plugin

- Manifest: `openpets.plugin.json` (`openpets.system-resources`, SDK v3). License: MIT.
- Entry: `index.js`. HUD via satellite-pet links of the default pet (`pets.spawn` + `speak({ pin: true, hud })`). Never pin on the default pet: that overwrites Virtual Pet.
- Catalog is CPU/RAM from `ctx.system.metrics`. GPU/SSD only when a sidecar already answers GET `http://127.0.0.1:37647/metrics?platform=`.
- Sidecar fetch always in try/catch. Without sidecar, HUD stays 2 items; no `sidecar offline` status. Fetch timeout 2500ms.
- Catalog ZIP cannot run `npm run install-sidecar`. Do not put that command in Configure.
- Languages: `locales/{en,nl,fr,de}.json`. Config `language` overrides host locale for HUD/speech.

## Catalog package

```bash
npm test
npm run package:catalog
```

Allowlist: `openpets.plugin.json`, `index.js`, declared `assets/`, `locales/*.json`, `LICENSE`.

## Sidecar (unpacked / developer)

- `sidecar/collect.mjs` + `sidecar/metrics-server.mjs`. Bind only `127.0.0.1:37647`.
- macOS: LaunchAgent. Linux: systemd --user + linger. Windows: Scheduled Task AtLogOn. Dispatcher: `scripts/install-sidecar.mjs`.
- GPU: ioreg (mac), nvidia-smi / GPU Engine (win), nvidia-smi / sysfs (linux). SSD: df / WMIC / df.

## Test

```bash
npm test
```

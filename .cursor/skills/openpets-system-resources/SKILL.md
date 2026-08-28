---
name: openpets-system-resources
description: Bouw en onderhoud de OpenPets system-resources plugin (CPU, RAM, GPU, SSD HUD plus lokale metrics-sidecar). Gebruik bij plugin-wijzigingen, sidecar, LaunchAgent, of unpack-load in OpenPets.
---

# OpenPets system-resources

## Plugin

- Manifest: `openpets.plugin.json` (`openpets.system-resources`, SDK v3).
- Entry: `index.js`. HUD via satellite-pet links van de default pet (`pets.spawn` + `speak({ pin: true, hud })`). Nooit pinnen op de default pet: dat overschrijft Virtual Pet.
- CPU/RAM uit `ctx.system.metrics`. GPU/SSD uit sidecar GET `http://127.0.0.1:37647/metrics?platform=`.
- Sidecar-fetch altijd in try/catch; ontbrekende meters als `null` / label `—`. Statusregel krijgt `sidecar uit` als GPU/SSD niet binnenkomen. Fetch-timeout 2500ms.
- GPU/SSD vereisen de sidecar. OpenPets-pluginload installeert die niet. Eerste keer: `npm run install-sidecar` (detecteert mac/linux/win). Check `curl -s http://127.0.0.1:37647/metrics`.
- Config `sidecarHint` (tekst, default `npm run install-sidecar`) is alleen een Configure-hint; `readConfig` negeert de waarde.
- Config `os`: `auto|mac|windows|linux`. Auto volgt `ctx.system.info().platform`.
- Talen: `locales/{en,nl,fr,de}.json`. Config `language` overrulet host-locale voor HUD/spraak.

## Sidecar

- `sidecar/collect.mjs` + `sidecar/metrics-server.mjs`. Bind alleen `127.0.0.1:37647`.
- macOS: LaunchAgent. Linux: systemd --user + linger. Windows: Scheduled Task AtLogOn. Dispatcher: `scripts/install-sidecar.mjs`.
- GPU: ioreg (mac), nvidia-smi / GPU Engine (win), nvidia-smi / sysfs (linux). SSD: df / WMIC / df.

## Testen

```bash
npm test
```

Daarna sidecar health: `curl -s http://127.0.0.1:37647/metrics`.

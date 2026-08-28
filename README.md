# OpenPets System Resources

Live CPU, RAM, GPU en SSD **links van je pet**. Virtual Pet (food, energy, play, bond) blijft op de pet.

GPU/SSD komen uit een lokale sidecar. Die zit **niet** in de OpenPets plugin-load: één keer installeren per machine, daarna start-ie bij login.

## Eerste installatie

Vereist: [Node.js](https://nodejs.org/) in PATH, daarna in deze map:

```bash
npm run install-sidecar
```

Dat kiest zelf het OS:

| OS | Wat er wordt gezet | Na reboot / login |
|---|---|---|
| macOS | LaunchAgent `nl.axtro.openpets-metrics` | `RunAtLoad` + `KeepAlive` |
| Linux | systemd user `openpets-metrics.service` | `enable --now` + linger indien mogelijk |
| Windows | Scheduled Task `OpenPetsMetricsSidecar` | trigger `AtLogOn` |

Check: `curl -s http://127.0.0.1:37647/health` → `{"ok":true}` (Windows: `Invoke-RestMethod http://127.0.0.1:37647/health`).

Zonder deze stap: CPU/RAM werken, GPU/SSD blijven `—` / status `sidecar uit`.

Foreground zonder persistente service: `npm run sidecar`.

## Laden in OpenPets

1. Sidecar zoals hierboven.
2. OpenPets → Plugins → Developer Mode → **Load unpacked plugin folder**.
3. Keur `system:metrics`, `network:local`, `pets:manage` en `pet:move` goed.

Handmatig per OS: `npm run install-sidecar:mac` / `:linux` / `:windows`.

## Config

- **Besturingssysteem**: automatisch (OpenPets) / macOS / Windows / Linux — GPU/SSD-bronnen van de sidecar
- **Taal**: automatisch / Nederlands / English / Français / Deutsch
- HUD aan/uit, poll 5–60s, alert-drempel, spreken bij hoge load

## Commands

- Show / hide resource HUD
- Read resources (of klik de pet)

## Licentie

[MIT](LICENSE) — gebruik, kopiëren, wijzigen, verkopen: allemaal oké. Alleen de copyrightregel laten staan.

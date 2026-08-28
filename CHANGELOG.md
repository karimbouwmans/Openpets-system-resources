# Changelog

## 1.3.3

- Config toont bovenaan een hint voor GPU/SSD: één keer `npm run install-sidecar` (kopieerbaar tekstveld), met uitleg per OS en dat het daarna bij login start.

## 1.3.2

- Eerste install: `npm run install-sidecar` detecteert macOS, Linux of Windows en zet de persistente sidecar (LaunchAgent / systemd --user / Scheduled Task).
- Linux herstart na login via `Restart=always` en probeert `loginctl enable-linger`. Windows-taak start bij `AtLogOn`.

## 1.3.1

- GPU en SSD ontbraken omdat de metrics-sidecar niet draaide. LaunchAgent-installatie zet PATH voor `ioreg`/`df`, cache van 4s, plugin-fetch 2,5s.
- macOS-GPU leest ook `Renderer Utilization %` als `Device Utilization %` ontbreekt.
- Statusregel meldt `sidecar uit` als GPU/SSD niet binnenkomen.

## 1.3.0

- Resource-HUD pakt de pinned slot van Virtual Pet niet meer. Meters staan op een satellite-pet links van de hoofdpet (eigen pin-slot).
- Food, energy, play en bond blijven op de pet; CPU/RAM/GPU/SSD volgen mee bij slepen.
- Extra permissies: `pets:read`, `pets:manage`, `pet:move`, `pet:animate`. Na update in Control Center opnieuw goedkeuren en Refresh.

## 1.2.2

- Plugin is weer één entry-bestand: geen relatieve ESM-imports meer, want de sandbox kan `./i18n.js` niet resolven.

## 1.2.1

- Catalog-icoon gewijzigd naar `plugin`; `activity` is geen toegestane host-icon.

## 1.2.0

- Sidecar werkt op macOS, Windows en Linux met per-OS GPU- en SSD-bronnen.
- Plugin-config heeft een OS-keuze (automatisch / macOS / Windows / Linux) die als `platform` naar de sidecar gaat.
- Windows: `nvidia-smi` of GPU Engine-counters, schijf via WMIC/CIM. Linux: `nvidia-smi` of sysfs `gpu_busy_percent`, `df` voor `/`.
- Installatiescripts voor systemd (Linux) en Scheduled Task (Windows).

## 1.1.0

- Franse en Duitse vertalingen toegevoegd (`locales/fr.json`, `locales/de.json`).
- Taal is kiesbaar in de plugin-config: automatisch, Nederlands, English, Français, Deutsch.
- HUD, statusregel en pet-spraak volgen de gekozen taal, niet alleen de OpenPets-hostlocale.

## 1.0.0

- Eerste OpenPets-plugin met een pinned HUD voor CPU, RAM, GPU en SSD.
- CPU en RAM komen uit `ctx.system.metrics`; GPU en SSD via een lokale sidecar op poort 37647.
- Commands om de HUD te tonen, te verbergen of de meters te laten voorlezen.
- Waarschuwing bij hoge load, assistant-capability `resources.get`, en LaunchAgent-installatie voor de sidecar.

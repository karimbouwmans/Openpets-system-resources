# OpenPets System Resources

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Live CPU and RAM to the left of your pet. On OpenPets versions that support
extended system metrics, the same HUD also shows aggregate GPU use and system
volume usage. Virtual Pet stats stay on the main pet.

The plugin does not install or run a sidecar. It uses the read-only
`system:metrics` capability supplied by OpenPets, so catalog installs work
without Node.js, a loopback listener, or a persistent background service.

## Install

Install **System Resources** from OpenPets → Plugins and approve the requested
pet controls plus **System metrics**. GPU and system-volume readings appear
when supported by the host OS and hardware; otherwise the HUD remains a useful
CPU/RAM monitor.

## Commands

- Show / hide resource HUD
- Read resources (or click the pet)

## Development

```bash
npm test
npm run package:catalog
```

The catalog package contains only the manifest, entry point, declared icons,
locales, and license.

## License

[MIT](LICENSE) — use, copy, modify, and sell with the copyright notice.

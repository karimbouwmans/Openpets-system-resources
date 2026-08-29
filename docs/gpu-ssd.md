# See GPU and SSD on the pet

Catalog install of this plugin shows **CPU and RAM only**. The OpenPets sandbox cannot read GPU or disk, and the catalog ZIP does not include the sidecar.

GPU and SSD appear after you install a **local metrics sidecar** from this GitHub repo. The plugin then GETs `http://127.0.0.1:37647/metrics`. That works for both catalog and unpacked plugin installs.

## Install (once per machine)

You need:

| What | Why |
|---|---|
| [Node.js](https://nodejs.org/) LTS (18+) on `PATH` | Runs `sidecar/metrics-server.mjs` |
| Git | Clone this repository |
| This repo, not the catalog ZIP | Contains `scripts/` and `sidecar/` |

macOS / Linux:

```bash
git clone https://github.com/karimbouwmans/Openpets-system-resources.git
cd Openpets-system-resources
node -v
npm run install-sidecar
```

Windows (PowerShell):

```powershell
git clone https://github.com/karimbouwmans/Openpets-system-resources.git
cd Openpets-system-resources
node -v
npm run install-sidecar
```

That command detects the OS and installs a login service:

| OS | Installed | Starts again at login |
|---|---|---|
| macOS | LaunchAgent `nl.axtro.openpets-metrics` | yes (`RunAtLoad` + `KeepAlive`) |
| Linux | systemd user `openpets-metrics.service` | yes (`enable --now`, linger if possible) |
| Windows | Scheduled Task `OpenPetsMetricsSidecar` | yes (`AtLogOn`) |

Force one OS:

```bash
npm run install-sidecar:mac
npm run install-sidecar:linux
npm run install-sidecar:windows
```

Success prints: `Sidecar listening on http://127.0.0.1:37647/metrics`.

Do **not** delete or move the clone afterwards: the service runs `node` against `sidecar/metrics-server.mjs` inside that folder.

## Run / check

Health:

```bash
curl -s http://127.0.0.1:37647/health
```

Expect `{"ok":true}`.

Windows:

```powershell
Invoke-RestMethod http://127.0.0.1:37647/health
Invoke-RestMethod "http://127.0.0.1:37647/metrics?platform=windows"
```

Metrics (pick your OS: `mac`, `windows`, or `linux`):

```bash
curl -s "http://127.0.0.1:37647/metrics?platform=mac"
```

You want numbers, not `null`:

```json
{
  "gpuPercent": 12,
  "ssdUsedPercent": 41,
  "platform": "mac"
}
```

Foreground without a login service (useful for debugging):

```bash
npm run sidecar
```

Leave that terminal open. Stop with Ctrl+C.

## OpenPets

1. Install **System Resources** from the catalog, or load this folder unpacked (Developer Mode).
2. Approve **`network:local`** (and `system:metrics`). Without `network:local` the sidecar can be healthy and the HUD still stays CPU/RAM-only.
3. Wait one poll (default **10 seconds**). No OpenPets restart required.

| Sidecar | HUD left of the pet |
|---|---|
| Not running | CPU, RAM |
| `GET /metrics` returns GPU/SSD percents | CPU, RAM, GPU, SSD |

## If GPU stays `—`

1. `curl` `/health` fails → sidecar is not running. Re-run `npm run install-sidecar` from the clone, or `npm run sidecar` in a terminal.
2. `/health` ok but `gpuPercent` is `null` → this machine has no collector the sidecar knows:
   - macOS: `ioreg` GPU utilization (`Device Utilization %` or `Renderer Utilization %`)
   - Windows / Linux: NVIDIA `nvidia-smi` on `PATH` (Windows can also use GPU Engine counters; Linux can use sysfs `gpu_busy_percent`)
3. `ssdUsedPercent` is `null` → disk query failed (`df` / WMIC). Root/data volume only; not a process list.
4. Sidecar ok, HUD still two meters → `network:local` not approved, or wait for the next poll.

macOS logs: `/tmp/axtro-openpets-metrics.err`.
Linux: `journalctl --user -u openpets-metrics.service`.
Windows: Task Scheduler → `OpenPetsMetricsSidecar`.

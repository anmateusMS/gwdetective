# GW Detective

> A single-file, browser-only analyser for **Power BI On-premises Data Gateway** log `.zip` bundles.

GW Detective is a self-contained `index.html` page that lets you drop in a gateway log archive and immediately get a structured, searchable view of every event, query, request and configuration item the gateway emitted. All processing happens **locally in your browser** — no data is ever uploaded.

![Dark theme, Cool Slate palette](https://img.shields.io/badge/theme-dark-292929) ![Single file](https://img.shields.io/badge/deploy-single%20file-eab308) ![No backend](https://img.shields.io/badge/backend-none-15803d)

---

## Quick start

1. Download or clone this repo.
2. Open `index.html` in any modern browser (Chrome, Edge, Firefox).
3. Drag a Power BI On-premises Data Gateway log `.zip` onto the drop zone, or click **Open Zip File**.
4. Wait a few seconds while the bundle is parsed in-browser, then explore the tabs.

No build step, no server, no install. The page works fully offline once loaded (CDN scripts can be inlined with the snippet at the top of `index.html`).

## What it does

GW Detective ingests a raw gateway log bundle — typically hundreds of `.log` files — and turns it into a navigable analytics console:

- **Dashboard** — gateway identity, version, machine info, totals (events, errors, warnings, queries, failures, average duration).
- **Errors / Info** — every event with severity filtering, full-text search, correlation-ID tracing (click any row to follow a request across files).
- **Network** — timeline of HTTP / WebSocket / SignalR traffic with status, latency and host.
- **Mashup** — Power Query mashup events parsed from the mashup engine logs (data sources, evaluations, refresh activity).
- **Queries** — every query the gateway ran, with duration, source product, query text and a per-query attribution column.
- **Power Platform** — purpose-built tab that joins `QueryStartReport_*.log` CSV data against the gateway requests to attribute traffic to:
  - **Client distribution** — PowerAutomate, LogicApps, PowerQueryOnline, PowerBI, PowerApps, "Other".
  - **Connector types** — HTTP, SQL, Oracle, SharePoint, FILE, **PowerPlatformDataflows**, etc.
  - **Top flows / logic apps / apps** by call volume, with the workflow ID, environment, top endpoint and unique-run count.
  - **Top endpoints** by call volume with their connector type.
- **Performance** — CPU / memory / network counter charts derived from `PBIEGPerformanceCounterReading*.log` files.
- **Ports** — parsed results of the gateway's port connectivity tests, newest first, with stale/fresh badges.
- **Installer** — installer and update history pulled from setup logs.
- **Config** — full gateway configuration dump with collapsible groups.

### Correlation tracing

Clicking any row in Errors, Info or Queries promotes that event's correlation ID into a trace panel that shows every other log line — across every source file — sharing that ID, with timestamps and severities aligned. Each request also gets its `RootActivityId` / `CurrentActivityId` exposed so cross-file traces work even when the correlation ID rotates.

### Power Platform attribution colours

Throughout the UI, Power Platform clients and connectors are shown as colour-coded pills so you can scan a long table at a glance:

| Pill | Colour | Used for |
|---|---|---|
| Purple | `#a855f7` | PowerAutomate |
| Blue | `#3b82f6` | LogicApps |
| Yellow | `#ca8a04` | PowerBI / Power BI Datasets |
| Emerald | `#10b981` | PowerQueryOnline |
| Pink | `#ec4899` | PowerApps |
| Green | `#22c55e` | PowerPlatformDataflows |

## Privacy

> **Your files never leave your browser.** The page has no backend, no telemetry and no outbound network calls. Parsing, indexing, charting and searching all happen inside the current tab.

The only external requests the page makes are to public CDNs to fetch [JSZip](https://stuk.github.io/jszip/), [Chart.js](https://www.chartjs.org/) and the [Chart.js date-fns adapter](https://github.com/chartjs/chartjs-adapter-date-fns). To make the page **fully offline**, run the PowerShell snippet at the top of `index.html` once to inline all three libraries.

## Architecture

`index.html` is the entire deliverable. Inside you'll find:

| Section | What's in it |
|---|---|
| `<style>` | Cool Slate dark theme variables, all UI components (tabs, tables, cards, pills, charts) |
| `<body>` | Static layout for the dropzone, header, tab bar and every tab's container |
| `<script>` (last) | The full app: zip parsing, log parsers (gateway, mashup, performance counters, ports, installer, config, QueryStartReport CSV), attribution index, sortable table engine, chart builders, correlation tracer |

External dependencies (loaded from CDN, can be inlined):

- [JSZip 3.10.1](https://stuk.github.io/jszip/)
- [Chart.js 4.4.9](https://www.chartjs.org/)
- [chartjs-adapter-date-fns 3.0.0](https://github.com/chartjs/chartjs-adapter-date-fns)

## Supported log files

GW Detective recognises and parses the standard Power BI On-premises Data Gateway log set, including:

- `Gateway*.log` — main gateway service events
- `GatewayNetwork*.log` — network / HTTP traffic
- `GatewayPipelineErrors*.log` — pipeline error stream
- `Mashup[Pbi]*.log` — Power Query mashup engine
- `QueryStartReport_*.log` — Power Platform attribution CSV
- `PBIEGPerformanceCounterReading*.log` — performance counters
- `GatewayPortsTestResults*.log` — port test results
- `GatewayInstaller*.log` — installer / update history
- `GatewayConfigurator*.log` / config dumps — full gateway configuration

Unknown files are still loaded and made available for full-text search.

## Browser support

Tested in current Chrome and Edge. Should work in any browser with reasonable ES2020 + `File` / `Blob` support.

## License

Internal Microsoft tooling. Not for redistribution outside the organisation.

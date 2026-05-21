# GW Detective

A single file, browser based analyzer for Power BI On-premises Data Gateway log zip bundles.

GW Detective turns a raw gateway log package into an interactive investigation console with filtering, correlation tracing, performance insights, and Power Platform traffic attribution.

## Live Site

GitHub Pages: https://anmateusms.github.io/gwdetective/

## Why this project exists

Gateway log bundles can contain hundreds of files and thousands of lines. GW Detective helps you answer practical questions quickly:

- What failed and when?
- Which systems and endpoints were involved?
- Which client types generated the most activity?
- Where are bottlenecks and spikes in duration or resource use?
- Which configuration or connectivity signals might explain the issue?

All parsing happens locally in your browser tab.

## Key capabilities

- Dashboard summary with key counts and gateway metadata
- Searchable Errors and Info views with severity filtering
- Correlation tracing across log files using activity IDs
- Network timeline analysis (HTTP/WebSocket/SignalR)
- Mashup log parsing for data source and refresh behavior
- Query analysis with duration, source, and attribution context
- Power Platform attribution from QueryStartReport logs
- Performance charts from gateway performance counter logs
- Ports and installer history views
- Config viewer with grouped/collapsible content

## Supported log families

- Gateway*.log
- GatewayNetwork*.log
- GatewayPipelineErrors*.log
- Mashup[Pbi]*.log
- QueryStartReport_*.log
- PBIEGPerformanceCounterReading*.log
- GatewayPortsTestResults*.log
- GatewayInstaller*.log
- GatewayConfigurator*.log

Unknown files are preserved for search context when possible.

## Quick start

1. Open [index.html](index.html) in a modern browser.
2. Drag and drop a gateway log zip file, or click Open Zip File.
3. Wait for parsing to complete.
4. Use tabs, filters, and correlation tracing to investigate.

No build step and no backend are required.

## Privacy and security

- Files are processed in the browser.
- No upload pipeline or backend service is required.
- No telemetry endpoint is used by the app itself.

Note: if you use the default CDN script references, browser requests for library files go to those public CDNs. You can inline libraries for offline use.

## Offline mode

A PowerShell snippet is included in [index.html](index.html) comments to inline JSZip and Chart.js assets. After inlining, the app can run as a fully self contained file.

## Project structure

- [index.html](index.html): Complete application UI, styles, parsing logic, and visualization code
- [README.md](README.md): Project documentation
- [gatewaytracer.code-workspace](gatewaytracer.code-workspace): Local VS Code workspace file

## Deploying updates to GitHub Pages

1. Commit and push changes to the main branch.
2. GitHub Pages rebuilds automatically.
3. Refresh the site URL after deployment completes.

## Browser compatibility

Tested primarily in recent Chrome and Edge versions. Any modern browser with File API and ES2020 support should work.

## Known limitations

- Large zip files may consume significant browser memory.
- Extremely large datasets can make table rendering slower.
- Parsing depends on expected gateway log shapes; uncommon formats may be partially parsed.

## Roadmap ideas

- Export filtered findings to CSV/JSON
- Saved investigation snapshots
- Additional parser coverage for edge-case formats
- Optional timeline overlays for cross-tab event alignment

## License

Internal Microsoft tooling.

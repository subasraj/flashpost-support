# Changelog

All notable changes to Flashpost are documented in this file.

## [2.0.15] - 2026-08-21

### Features
- **Split Editor Right** — `Cmd+\` or the "Split Editor Right" button now works on Flashpost webview panels, moving the active request to a side-by-side editor group
- **Response Timing Breakdown** — Hover over response time to see DNS Lookup, TCP Handshake, SSL Handshake, and Transfer timing with colored proportional bars
- **Response Size Breakdown** — Hover over response size to see request/response split into headers and body (with directional arrows)
- **Variables in Request** — New panel in the response area showing all variables used in the current request
- **History Panel improvements** — Updated UI and flow for the History panel
- **Environment Panel improvements** — Updated UI and logic for the Environment/Variables panel
- **Response Panel UI refresh** — Updated icons and toolbar layout for code snippet, save, and menu actions

---

## [2.0.14] - 2026-08-20

### Features
- **Saved Response Examples** — Save API responses as child nodes under requests in the collection tree (Postman-style examples)
- **Copy & Paste** — Cmd+C/Ctrl+C and Cmd+V/Ctrl+V keyboard shortcuts for collection tree items; also available in right-click context menu for collections, folders, requests, and examples
- Added Postman collection import support for saved response examples
- Response timing data is now persisted when saving response examples

### Bug Fixes
- Fixed focus-related issues in the UI

---

## [2.0.13] - 2026-08-19

### Features
- **Cookie Jar V2** — Postman-compatible cookie jar with individual cookie storage, full RFC attribute support, domain/path/secure matching, and scripting API (`fp.cookies.*`)
- Added Reload button to Run Collection
- Added test summary at the top of Run Collection results

### Bug Fixes
- Fixed `.to.be.true` assertion issue in script-based tests
- Fixed Run Collection crash when a request has a NULL URL
- Fixed history tab not updating after request execution
- Fixed history and variable panel selection highlighting

---

## [2.0.12] - 2026-08-17

### Features
- **Change Environment shortcut** — Quick switch via Command Palette (`Ctrl+Shift+E` / `Cmd+Shift+E`) (Issue #63)
- **OAuth2 Password Credential Grant Type** (Issue #71)
- Added Delete All button to Environments
- Modified tree sorting; Attached Environment combo box styled consistently with Authorization Type

### Bug Fixes
- Fixed text box input reversing issue (Issue #96)

---

## [2.0.10] - 2026-08-16

### Features
- **Collection/Folder level Scripts** — Pre-request and post-response scripts at collection and folder level with inheritance
- **Attach Environment to Collection** — Set a default environment via Collection Settings
- Added keyboard navigation (arrow keys) and Delete key for tree items

---

## [2.0.9] - 2026-08-15

### Features
- **Script-based Testing** — `fp.test()` / `fp.expect()` with Chai-style assertions in pre-request and post-response scripts
- Added script import from Postman collections
- **Scripting Engine** — Pre-request and post-response scripts with sandboxed execution (Node.js `vm` module)
- Script function enhancements and UI improvements
- **Workspace Mode** — Use JSON files only for git-friendly storage

### Bug Fixes
- Fixed Content-Type header override when user explicitly sets it (Issue #85)
- Fixed `maxContentLength size of Infinity exceeded` error (Issue #70)
- Fixed issue #87

---

## [2.0.1] - 2026-08-14

### Breaking Changes
- **Database migrated from LokiJS to SQLite** (via sql.js WebAssembly) — Automatic one-time migration on first launch

### Features
- **Export/Import all collections and variables** — Full backup and restore
- Updated UI look and feel
- Added migration notification popup for v2 upgrade
- Version number now sourced from `package.json`

### Bug Fixes
- Fixed deployment issue with antivirus flagging `.wasm` files
- Fixed migration folder popup issue
- Fixed curl import issues
- Fixed right-click menu alignment (positioned at mouse cursor)
- Fixed Pass/Fail summary display (Issue #93)
- Fixed response wrapping (Issue #90)

---

## Summary of Version 2 Highlights

| Feature | Description |
|---------|-------------|
| SQLite Database | Replaced LokiJS with sql.js (WebAssembly) for faster, more reliable storage |
| Scripting Engine | Pre-request/post-response scripts with `fp`, `pm`, `bru`, `tc` APIs |
| Script Testing | `fp.test()` / `fp.expect()` with Chai-style assertions |
| Cookie Jar V2 | Postman-compatible cookie management with RFC attributes |
| Saved Response Examples | Save and organize API responses under requests |
| Collection Scripts | Scripts at collection and folder level with inheritance |
| Attached Environments | Bind an environment to a collection |
| OAuth2 Password Grant | Additional OAuth2 grant type support |
| Workspace Mode | Git-friendly JSON file storage |
| Export/Import | Full collection and variable backup/restore |
| Quick Env Switch | `Ctrl+Shift+E` command palette shortcut |

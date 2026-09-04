# Changelog

All notable changes to Flashpost are documented in this file.

## [2.0.22] - 2026-08-29
### Improvements
- **Uniform Run Collection buttons** — The Run, Reload, Cancel, and Export buttons in the Collection Runner now share a consistent width instead of sizing to their label text.
- **Documentation** — Reformatted README.md with a table of contents, feature summary table, and cleaner structure.

## [2.0.21] - 2026-08-26
### Features
- **OpenAPI Required/Optional Parameter Validation (Issue #76)** — Parameters imported from OpenAPI specs now carry `required` and `description` metadata. Required params show a red dot indicator (●) and description tooltip (ⓘ) in Query Params, Headers, and Path Variables tables. Clicking Send with missing required values shows a categorized modal popup (e.g., "Query Params: include", "Path Variables: userId", "Body: name").
- **Required Body Parameter Validation** — Body parameters marked as `required` in OpenAPI schemas are validated before sending. Removing a required key from the JSON body or clearing the body entirely triggers a warning popup.
- **Duplicate Path Variable Detection** — If the same path variable name appears twice in the URL (e.g., `:userId/:userId`), a warning popup appears on Send.
- **Persistent Parameter Metadata** — Path param and query param descriptions/required flags are stored in separate DB columns (`path_param_meta`, `query_param_meta`) so they survive removal and re-addition of params in the URL.
- **Script Import Confirmation** — Importing collections containing scripts (pre-request/test) now shows a modal warning: "Import with Scripts", "Import without Scripts", or Cancel. Protects against untrusted collection scripts.

### Improvements
- **Categorized Validation Messages** — The required-param popup now groups missing values by category (Query Params, Headers, Path Variables, Body) instead of a flat list.
- **Tooltip Theme Compatibility** — All react-tooltip instances now use VS Code CSS variables (`--vscode-editorWidget-background`, `--vscode-editorWidget-foreground`, `--vscode-editorWidget-border`) for full light/dark/high-contrast theme support.
- **Tooltip Opacity & Placement** — Collection sidebar description tooltips are now fully opaque (no bleed-through) and positioned above items.

### Bug Fixes
- **Path param metadata lost on URL edit** — Fixed the reducer's `updatePathParams` and `updateQueryParams` losing `description`/`required` fields when URL was edited (rebuilt params from URL without preserving metadata).
- **Duplicate path params renaming all entries** — Fixed `updatePathParams` matching the same param entry multiple times when duplicate names existed. Now tracks consumed indices.
- **Non-required params triggering validation** — Fixed validation popup firing for path params without values even when they weren't marked as required.

## [2.0.20] - 2026-08-25

### Features
- **New Request Dropdown** — "New Request" button now shows a dropdown menu with **HTTP** and **GraphQL** options. Selecting GraphQL opens the request with the Body tab active and the GraphQL sub-tab pre-selected.
- **Responsive Layout** — The request/response panel layout now auto-switches between Vertical Split and Horizontal Split based on window width (≤900px switches to horizontal, wider restores vertical).

### Bug Fixes
- **Dynamic variables without environment** — Fixed `{{$randomXxx}}` faker variables not resolving when no environment variables were configured or the variable data was empty.
- **JSON body control characters** — Fixed "Bad control character in string literal" error when faker values containing newlines or tabs (e.g., `{{$randomLoremLines}}`, `{{$randomPhrase}}`) were used in JSON request bodies.
- **Delete key on hidden panel** — Fixed Delete/Backspace key firing on collection items even when the Collections tab was not visible (e.g., when History or Environment tab was active). Now checks parent panel visibility before processing the keystroke.
- **Multiple new request tabs** — Fixed clicking "New Request" multiple times opening only one tab. Each new request now gets a unique panel identifier using a timestamp suffix.
- **Panel reopen from History** — Fixed requests not reopening from the History tab after being saved and closed. The dispose handler now correctly unregisters both the initial and final panel IDs.
- **Sort context menu position** — Moved Sort submenu after Delete in the context menu. Sort options now expand inline below the trigger to avoid being clipped by panel edges.
- **Sort click toggling tree** — Fixed clicking "Sort ▸" in the context menu unintentionally expanding/collapsing the collection or folder tree node.

### Improvements
- **Smaller extension bundle** — Reduced extension.js from 13MB to 8MB by importing only the English locale from Faker.js instead of all locales.

## [2.0.18] - 2026-08-24

### Features
- **Sort Collections & Folders** — Right-click context menu with "Sort ▸" sub-menu offering: Folders First Default, Folders First A to Z, Folders First Z to A.
- **OpenAPI Description Import** — Endpoint descriptions, parameter descriptions, and default values are now imported into the Notes field when importing OpenAPI/Swagger specs. Body property descriptions from the schema are also included.
- **Connection Retry** — Failed connections (ECONNREFUSED, ETIMEDOUT, ECONNRESET, ECONNABORTED) now automatically retry every 2 seconds until the configured timeout is reached. Cancel button stops retries immediately.

### Improvements
- **Collection tree spacing** — Added padding above the first collection item for easier drag-and-drop to top position.

### Bug Fixes
- **Timeout error calculation** — Fixed timeout comparison that was multiplying milliseconds by 1000 again, causing incorrect "unable to connect" vs "timed out" detection.

## [2.0.17] - 2026-08-23

### Features
- **Rename from Tab** — Right-click any open request or environment tab to rename it directly. Works for collection requests, history requests, and environment variables.
- **Logs Keyboard Shortcut** — `Ctrl+Shift+L` / `Cmd+Shift+L` to quickly open Flashpost logs panel.
- **New Icon** — Redesigned Flashpost icon with a modern diagonal lightning bolt, speed lines, and API endpoint dots on a transparent background.

### Bug Fixes
- **URL-Encoded checkbox toggle** — Fixed `onSelectChange` in URL-Encoded body writing to `body.formdata` instead of `body.urlencoded`, causing silent data corruption when toggling checkboxes.
- **Auth conditional comparison** — Fixed `setAuthValue` using assignment (`=`) instead of comparison (`===`) for `auth.addTo`, which prevented the `removeHeaders()` branch from ever executing when switching away from inherited API Key auth.

### Improvements
- **New collection ordering** — When a new collection is created, it now appears at the top of the sidebar list.
- **Extension loading** — Improved activation guard to prevent double-initialization.
- **Error logging** — Cleaned up build-time warning messages and improved error log output.

## [2.0.16] - 2026-08-22

### Features
- **Delete All Collections** — New menu option to permanently delete all collections, folders, requests, and saved examples with double-confirmation warning dialog
- **Version in logs** — Activation logs now display the extension version (`Flashpost v2.0.16 extension activation...`)

### Improvements
- **Collection ordering** — "Save to Collection" dropdown now respects drag-and-drop order (`relative_index`)
- **Immediate reorder sync** — Drag-and-drop changes in the sidebar immediately refresh the "Save to Collection" panel if open
- **Import active variable** — When importing "All Collections" with variables, the imported active variable flag is preserved and existing active flags are cleared
- **Timeout error messages** — Error messages now show actual elapsed time instead of the configured timeout limit

### UI Improvements
- Updated Run All Collections UI
- Fixed Collection settings environment refresh issue
- Cleaned up "Save to Collection" UI
- Updated Environments UI and flow
- Modified collection settings UI
- Unified import to use single import flow for all collection types
- "Delete All Collections" menu item shown in red, hidden when no collections exist

### Bug Fixes
- Fixed tab switching state management

---

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

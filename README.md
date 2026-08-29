# Flashpost — REST API Client for Visual Studio Code

[![Version](https://vsmarketplacebadges.dev/version/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Installs](https://vsmarketplacebadges.dev/installs/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Rating](https://vsmarketplacebadges.dev/rating/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Open VSX](https://img.shields.io/open-vsx/v/VASubasRaj/flashpost)](https://open-vsx.org/extension/VASubasRaj/flashpost)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Flashpost is a lightweight REST API client for Visual Studio Code. Design, test, and debug APIs without leaving your editor — no context switching to external tools, no bloat.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-search.gif?raw=true" alt="Flashpost quick start demo"/>
</div>

---

## Table of Contents

- [Highlights](#highlights)
- [Getting Started](#getting-started)
- [Collections & Organization](#collections--organization)
- [Scripting](#scripting)
- [Testing](#testing)
- [Collection Runner](#collection-runner)
- [Saved Response Examples](#saved-response-examples)
- [Response Insights](#response-insights)
- [Code Generation](#code-generation)
- [Environments & Variables](#environments--variables)
- [Workspace Storage](#workspace-storage-git-friendly)
- [Migration](#migration-from-other-tools)
- [Configuration](#configuration)
- [Requirements](#requirements)
- [Support](#support)

---

## Highlights

| Capability | Description |
|------------|-------------|
| **Lightweight & Fast** | Minimal-overhead client built specifically for VS Code |
| **Collections & Environments** | Organize requests hierarchically and manage multiple environments |
| **Broad Import Support** | Migrate from Postman, Thunder Client, and OpenAPI/Swagger with descriptions and required-field metadata |
| **cURL Integration** | Import and execute cURL commands directly |
| **Scriptless & Scripted Testing** | GUI-based assertions or Chai-style `fp.test` / `fp.expect` scripts |
| **Pre/Post Scripts** | JavaScript hooks at collection, folder, and request levels |
| **Cookie Jar** | Postman-compatible cookie handling with automatic capture and domain/path matching |
| **Saved Response Examples** | Persist responses as reusable examples under requests |
| **Response Insights** | Detailed timing (DNS, TCP, SSL, Transfer) and size breakdowns on hover |
| **Code Generation** | Export requests as snippets in multiple languages |
| **GraphQL Support** | Dedicated GraphQL request mode |
| **Collection Runner** | Batch-execute requests with script and test result reporting |
| **Local & Git-Friendly Storage** | SQLite by default, or human-readable JSON committed to your workspace |

---

## Getting Started

1. **Install** — Search for "Flashpost" in the VS Code Extensions Marketplace.
2. **Open** — Click the Flashpost icon in the Activity Bar.
3. **Create a request** — Click **New Request** and choose **HTTP** or **GraphQL**.
4. **Send** — Enter your endpoint, configure headers, and hit Send.

For a complete walkthrough, see the **[User Manual](https://github.com/subasraj/flashpost-support/blob/main/USER-MANUAL.md)**.

---

## Collections & Organization

Group related requests into collections and folders for cleaner project management.

- Access collection operations from the collection icon next to the filter.
- Build nested folder hierarchies.
- Import existing collections from Postman or Thunder Client.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-new-collection.png?raw=true" alt="Creating collections"/>
</div>

### Export All Collections

Export your entire workspace — collections, folders, requests, and environment variables — into a single JSON file.

1. Open the **Collections** tab in the sidebar.
2. Click the menu icon and select **Export All Collections**.
3. Choose a save location.

The exported file includes the full folder hierarchy, every request with its complete configuration (headers, body, auth, tests, set variables), and all environment variables with their active state. Re-import it with **Import Collections** to restore everything in order.

### Copy & Paste

Duplicate items in the collection tree using keyboard shortcuts or the context menu.

- **Cmd+C / Ctrl+C** — Copy the selected collection, folder, or request.
- **Cmd+V / Ctrl+V** — Paste into the focused folder (or as a sibling when a request is focused).
- Pasting a folder or collection creates a deep copy including all children and examples.

### Sort Collections & Folders

Right-click a collection or folder to sort its contents:

- **Folders First, Default** — Folders on top, original order preserved.
- **Folders First, A to Z** — Alphabetical.
- **Folders First, Z to A** — Reverse alphabetical.

### Rename from Tab

Right-click any open request or environment tab to rename it. The sidebar tree, database, and tab title update together.

---

## Scripting

Write JavaScript that runs before a request is sent or after a response is received. Scripts can modify requests, read responses, and manage variables.

**Usage:** Open a request → **Script** tab → select **Pre Request** or **Post Response** → write code → **Send**.

### Variable Management (`fp` / `bru` / `pm` / `tc`)

All four objects are interchangeable and expose `request` and `response` sub-objects (`fp.request.*`, `fp.response.*`, etc.).

| Method | Description |
|--------|-------------|
| `fp.getEnvVar(key)` | Get an environment variable |
| `fp.setEnvVar(key, value)` | Set an environment variable (persisted) |
| `fp.hasEnvVar(key)` | Check if an env variable exists |
| `fp.deleteEnvVar(key)` | Delete an environment variable |
| `fp.getAllEnvVars()` | Get all env variables as an object |
| `fp.getEnvName()` | Get the current environment name |
| `fp.getGlobalEnvVar(key)` | Get a global variable |
| `fp.setGlobalEnvVar(key, value)` | Set a global variable (persisted) |
| `fp.getVar(key)` | Get a runtime variable |
| `fp.setVar(key, value)` | Set a runtime variable (session only) |
| `fp.interpolate(string)` | Resolve `{{variables}}` and `{{$randomName}}` |

### Request Object (`req`, Pre-Request only)

| Method | Description |
|--------|-------------|
| `req.getUrl()` / `req.setUrl(url)` | Get/set request URL |
| `req.getMethod()` / `req.setMethod(method)` | Get/set HTTP method |
| `req.getHeader(name)` / `req.setHeader(name, value)` | Get/set a header |
| `req.getHeaders()` / `req.setHeaders(obj)` | Get/set all headers |
| `req.deleteHeader(name)` | Remove a header |
| `req.getBody()` / `req.setBody(data)` | Get/set the request body |
| `req.getHost()` / `req.getPath()` | Get URL parts |
| `req.headerList` | Full PropertyList interface |

### Response Object (`res`, Post-Response only)

| Method | Description |
|--------|-------------|
| `res.status` / `res.getStatus()` | HTTP status code |
| `res.body` / `res.getBody()` | Parsed response body |
| `res.headers` / `res.getHeaders()` | Response headers object |
| `res.getHeader(name)` | Get a specific response header |
| `res.responseTime` / `res.getResponseTime()` | Response time in ms |
| `res.getSize()` | Response size in bytes |
| `res.getRawBody()` | Raw response string |

### Utilities

| Method | Description |
|--------|-------------|
| `console.log(value)` | Log to the Flashpost output panel |
| `atob(str)` / `btoa(str)` | Base64 decode/encode |
| `fp.interpolate("{{$randomFirstName}}")` | Generate random data |
| `fp.sha256(data)` | SHA-256 hash (hex string) |
| `fp.md5(data)` | MD5 hash (hex string) |
| `fp.hmacSha256(data, key)` | HMAC-SHA256 (hex string) |

### Examples

**Pre-request script**
```javascript
// Add a timestamp header
req.setHeader("X-Request-Time", new Date().toISOString());

// Set auth token from environment
const token = fp.getEnvVar("auth_token");
req.setHeader("Authorization", "Bearer " + token);

console.log("Sending to:", req.getUrl());
```

**Post-response script**
```javascript
// Save token from response
const body = res.getBody();
if (body.access_token) {
  fp.setEnvVar("auth_token", body.access_token);
}

console.log("Status:", res.getStatus());
console.log("Time:", res.getResponseTime(), "ms");
```

**AWS Lambda Function URL / CloudFront OAC**
```javascript
// Add x-amz-content-sha256 header (required for Lambda Function URL / CloudFront OAC)
const body = req.getBody({ raw: true }) || "";
req.setHeader("x-amz-content-sha256", fp.sha256(body));
```

### Behavior Notes

- Scripts run in a sandboxed environment with a 5-second timeout.
- Modifications to `req` affect only the current execution — they are **not** saved to the database.
- Variable changes via `fp.setEnvVar` / `fp.setGlobalEnvVar` **are** persisted.
- `console.log` output appears in **View → Output → Flashpost**.
- `request` and `response` are aliases for `req` and `res`.
- Postman's `postman.setEnvironmentVariable(key, value)` / `getEnvironmentVariable(key)` are supported.
- `CryptoJS` and `crypto` are both available (SHA256, HmacSHA256, enc.Base64, etc.).

### Collection & Folder Scripts

Define scripts at the collection or folder level via **Settings → Scripts**. They run automatically for all contained requests:

- **Pre-Request order:** Collection → Folder → Request
- **Post-Response order:** Request → Folder → Collection
- Postman collection/folder-level scripts are preserved on import.

### Collection Environment

Attach an environment to a collection via **Collection Settings → Environment**:

- Requests in the collection use the attached environment's variables, overriding the active environment.
- Variable highlighting reflects the attached environment in real time.
- Variables set by scripts save to the attached environment.
- Save settings with **Ctrl+S / Cmd+S**.

---

## Testing

### Scriptless Testing

Create and manage tests through a GUI — no test code required.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-scriptless-testing.png?raw=true" alt="Scriptless testing"/>
</div>

### Script-Based Testing (`fp.test` / `fp.expect`)

Write Chai-style assertions in post-response scripts. Results appear in the **Tests** tab.

- **`fp.test(name, fn)`** — Define a named test case.
- **`fp.expect(value)`** — Create a chainable assertion.

| Assertion | Description |
|-----------|-------------|
| `.to.equal(val)` | Strict equality (`===`) |
| `.to.eql(val)` | Deep equality (objects, arrays) |
| `.to.include(val)` | Contains (string/array/object) |
| `.to.be.a(type)` | Type check (`"string"`, `"number"`, `"array"`, etc.) |
| `.to.be.above(n)` / `.below(n)` | Numeric comparisons |
| `.to.be.at.least(n)` / `.at.most(n)` | Inclusive comparisons |
| `.to.be.within(min, max)` | Range check |
| `.to.be.ok` / `.true` / `.false` / `.null` / `.undefined` / `.empty` | Truthiness |
| `.to.have.property(key)` | Property exists |
| `.to.have.length(n)` | Array/string length |
| `.to.have.members(arr)` | Same array members |
| `.to.have.status(code)` | Response status |
| `.to.have.header(name)` | Response header exists |
| `.not.equal(val)` | Negation (works with all assertions) |

```javascript
fp.test("Status is 200", () => {
  fp.expect(res.getStatus()).to.equal(200);
});

fp.test("Body structure", () => {
  const body = res.getBody();
  fp.expect(body).to.have.property("data");
  fp.expect(body.data).to.be.an("array").and.not.be.empty;
  fp.expect(body.data[0]).to.have.all.keys(["id", "name", "email"]);
});

fp.test("Performance", () => {
  fp.expect(res.getResponseTime()).to.be.below(500);
});
```

See the **[Complete Script API Reference](https://github.com/subasraj/flashpost-support/blob/main/SCRIPT-API.md)** for full details.

---

## Collection Runner

Execute multiple requests in sequence.

- Right-click any folder or collection and select **Run All**.
- Runs all requests and test cases in order.
- Executes pre-request and post-response scripts at collection, folder, and request levels.
- Reports comprehensive results and test outcomes.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-runtests.png?raw=true" alt="Collection runner"/>
</div>

---

## Saved Response Examples

Persist API responses as reusable examples under requests, similar to Postman's Examples.

1. Send a request and receive a response.
2. Click **Save Response** in the response panel toolbar.
3. The response is saved as a child node under the request, showing its HTTP status badge in the sidebar.

- **Full snapshot** — request and response data (URL, method, headers, body, status, timing) are persisted.
- **Open in tab** — click an example to open it in its own panel.
- **Drag-and-drop** — reorder examples within their parent request.
- **Duplicate** — duplicating a request also duplicates its examples.
- **Delete cascade** — deleting a request removes all its examples.
- **Rename** — right-click to rename; open tabs update automatically.
- **Workspace mode** — examples are exported as part of `responses.json` for git-friendly storage.

---

## Response Insights

### Timing Breakdown

Hover over the **Time** value in the response status bar for a phase-by-phase breakdown.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-timing-breakdown.png?raw=true" alt="Response timing breakdown"/>
</div>

- **DNS Lookup** — time to resolve the domain name.
- **TCP Handshake** — time to establish a TCP connection.
- **SSL Handshake** — time to complete TLS negotiation (HTTPS only).
- **Transfer** — server processing time plus response download.

Each phase includes a proportional colored bar. Phases using a reused connection show "Cache".

### Size Breakdown

Hover over the **Size** value for a complete request/response size breakdown.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-size-breakdown.png?raw=true" alt="Response size breakdown"/>
</div>

- **↓ Response Size** — total, with headers and body split.
- **↑ Request Size** — total, with headers and body split.

---

## Code Generation

Generate ready-to-use snippets in multiple languages. Click the `</>` icon in the request view to open the **Code** tab.

| Language | Libraries |
|----------|-----------|
| C# | HttpClient, RestSharp |
| Go | Native HTTP client |
| Java | AsyncHttp, Unirest, OkHttp, NetHttp |
| JavaScript | Axios, Fetch, jQuery, XMLHttpRequest |
| PHP | cURL, native |
| Python | Requests |
| Shell | cURL |

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-code-snippet.png?raw=true" alt="Code generation"/>
</div>

---

## Environments & Variables

### Environment Management

Manage development, staging, and production environments side by side.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-environment-setactive.png?raw=true" alt="Environment management"/>
</div>

### Quick Environment Switch

- **Command Palette:** `Flashpost: Change Environment`
- **Shortcut:** `Ctrl+Shift+E` (Windows/Linux) / `Cmd+Shift+E` (macOS)
- Displays all environments with the active one marked ⭐, updating the sidebar, panels, and highlighting immediately.

### Variables from Responses

Extract values from response data, headers, and cookies, then reuse them across requests.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-env-var-from-response.png?raw=true" alt="Environment variables from response"/>
</div>

### Dynamic Variables

Generate realistic sample data at runtime using [Faker.js](https://www.npmjs.com/package/@faker-js/faker). Dynamic variables start with `$` and regenerate on each request.

```
$guid          // Unique GUID
$timestamp     // Current timestamp
$randomName    // Random person name
$randomEmail   // Random email address
$randomPhone   // Random phone number
```

See the **[Dynamic Variables Reference](https://github.com/subasraj/flashpost-support/blob/main/random-variables.md)** for the complete list.

### History

New requests are saved to the **History** tab. Control the number of retained items with the **History Limit** setting.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-history.png?raw=true" alt="Request history"/>
</div>

---

## Workspace Storage (Git-Friendly)

Enable **Save Data To Workspace** to store all data as human-readable JSON instead of a binary SQLite database — making collections easy to commit, diff, and share.

```
flashpost-tests/
  requests.json           # All API requests
  collection_tree.json    # Collection/folder hierarchy
  history.json            # Request history
  variables.json          # Environment variables
  cookies.json            # Legacy cookie storage
  cookies_v2.json         # Cookie jar (individual cookies with attributes)
  responses.json          # Saved response examples
  user_preferences.json   # User preferences
```

- All files are formatted JSON (2-space indent) for clean diffs.
- SQLite runs in-memory for fast queries; JSON files are the persistence layer.
- No binary files — everything is readable and mergeable.
- Teammates opening the project automatically load the shared data.

---

## Migration from Other Tools

### From Postman

1. Export your Postman collections and environments.
2. Click the import icon in Flashpost.
3. Select the exported files (multiple collections and environments at once).

### From Thunder Client

1. Export your Thunder Client data.
2. Use Flashpost's import feature and select the Thunder Client format.
3. Import collections and environments.

---

## Configuration

Access settings via the gear icon in the top-right corner of the sidebar.

| Setting | Description |
|---------|-------------|
| **Custom Data Location** | Where collections and data are stored |
| **Save to Workspace** | Store request data in the workspace as git-friendly JSON |
| **Workspace Relative Path** | Custom relative path for workspace data |
| **History Limit** | Number of historical requests to keep |

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-extension-settings.png?raw=true" alt="Extension settings"/>
</div>

**Quick logs access:** `Ctrl+Shift+L` (Windows/Linux) / `Cmd+Shift+L` (macOS).

**Split editor:** view two requests side by side with Split Editor Right (`Cmd+\`).

---

## Requirements

- **VS Code:** 1.109.0 or higher
- **Operating System:** Windows, macOS, or Linux
- **Node.js:** Not required — the extension is self-contained

---

## Support

- **Bug Reports:** [GitHub Issues](https://github.com/subasraj/flashpost-support/issues)
- **Feature Requests:** [GitHub Discussions](https://github.com/subasraj/flashpost-support/discussions)
- **Documentation:** [Support Repository](https://github.com/subasraj/flashpost-support)

## License

Licensed under the [MIT License](LICENSE).

---

If Flashpost improves your workflow, consider starring the project, leaving a review on the VS Code Marketplace, and sharing it with your team.

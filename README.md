# Flashpost - REST API Client for VS Code

[![Version](https://vsmarketplacebadges.dev/version/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Installs](https://vsmarketplacebadges.dev/installs/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Rating](https://vsmarketplacebadges.dev/rating/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Open VSX](https://img.shields.io/open-vsx/v/VASubasRaj/flashpost)](https://open-vsx.org/extension/VASubasRaj/flashpost)

Flashpost is a lightweight REST API client extension that enables you to develop and test your REST APIs directly from Visual Studio Code. Perfect for developers who want to stay in their IDE without switching to external tools.

## ✨ Key Features

- **🚀 Lightweight & Fast** - Minimal overhead REST API client built specifically for VS Code
- **🎯 Simple Interface** - Clean, intuitive UI that gets out of your way
- **📁 Collections & Environments** - Organize your requests and manage multiple environments
- **📥 Import Support** - Seamlessly migrate from Postman and Thunder Client
- **🔄 cURL Integration** - Import and execute cURL commands directly
- **🧪 Scriptless Testing** - GUI-based testing without writing test scripts
- **💾 Local Storage** - All data stored locally in SQLite with customizable storage location
- **🏢 Workspace Integration** - Save requests to your current workspace
- **🔧 Environment Variables** - Support for .env files and dynamic variables
- **📋 Code Generation** - Generate code snippets in multiple languages
- **🍪 Cookie Management** - Postman-compatible cookie jar with automatic capture, domain/path matching, and scripting API
- **💾 Saved Response Examples** - Save API responses as reusable examples under requests (Postman-style)
- **⏱️ Response Timing** - Detailed request timing breakdown (DNS, TCP, SSL, Transfer) on hover
- **📊 Size Breakdown** - Request and response size details with headers/body split on hover
- **✂️ Copy & Paste** - Keyboard shortcuts (Cmd+C/V) and context menu for collection tree items
- **↔️ Split Editor** - View two requests side by side using Split Editor Right (Cmd+\)
- **📊 Collection Runner** - Execute multiple requests in batch with script execution and test results
- **🔍 GraphQL Support** - Built-in GraphQL query support

## 🚀 Quick Start

1. **Install the Extension** - Search for "Flashpost" in the VS Code Extensions marketplace
2. **Open Flashpost** - Click the Flashpost icon in the Activity Bar
3. **Create Your First Request** - Click "New Request" in the Quick Access sidebar
4. **Test Your API** - Enter your endpoint, configure headers, and hit send!

📖 **[Full User Manual](https://github.com/subasraj/flashpost-support/blob/main/USER-MANUAL.md)** - Comprehensive guide covering all features in detail.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-search.gif?raw=true" alt="Flashpost Quick Start"/>
</div>

## 📁 Collections Management

Organize your API requests into collections for better project management.

- Click the collection icon next to the filter to access collection operations
- Create folders and organize requests hierarchically
- Import existing collections from Postman or Thunder Client

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-new-collection.png?raw=true" alt="Creating Collections"/>
</div>

## 📦 Export All Collections

Export your entire workspace — all collections, folders, requests, and environment variables — into a single JSON file.

- Open the Collections tab in the sidebar
- Click the menu icon and select **"Export All Collections"**
- Choose a save location — all data is exported as a single `.json` file
- The exported file includes:
  - All collections with their folder hierarchy
  - All requests with full configuration (headers, body, auth, tests, set variables)
  - All environment variables with their active/inactive state
- Import the exported file back using **"Import Collections"** — collections and variables are restored with the correct order and state

## 📝 Scripts (Pre-Request & Post-Response)

Write JavaScript scripts that run before a request is sent or after a response is received. Scripts can modify requests, read responses, and manage variables.

### Usage
1. Open any request and click the **Script** tab
2. Select **Pre Request** or **Post Response**
3. Write your JavaScript code in the editor
4. Click **Send** — scripts execute automatically

### Available API

**`fp` / `bru` / `pm` / `tc` object — Variable Management**

All four objects (`fp`, `bru`, `pm`, `tc`) are interchangeable. They also expose `request` and `response` sub-objects:
- `fp.request.*` / `bru.request.*` / `pm.request.*` / `tc.request.*` — same as `req.*`
- `fp.response.*` / `bru.response.*` / `pm.response.*` / `tc.response.*` — same as `res.*`

| Method | Description |
|--------|-------------|
| `fp.getEnvVar(key)` | Get an environment variable |
| `fp.setEnvVar(key, value)` | Set an environment variable (persisted) |
| `fp.hasEnvVar(key)` | Check if env variable exists |
| `fp.deleteEnvVar(key)` | Delete an environment variable |
| `fp.getAllEnvVars()` | Get all env variables as object |
| `fp.getEnvName()` | Get current environment name |
| `fp.getGlobalEnvVar(key)` | Get a global variable |
| `fp.setGlobalEnvVar(key, value)` | Set a global variable (persisted) |
| `fp.getVar(key)` | Get a runtime variable |
| `fp.setVar(key, value)` | Set a runtime variable (session only) |
| `fp.interpolate(string)` | Resolve `{{variables}}` and `{{$randomName}}` |

**`req` object — Request (Pre-Request only)**

| Method | Description |
|--------|-------------|
| `req.getUrl()` / `req.setUrl(url)` | Get/set request URL |
| `req.getMethod()` / `req.setMethod(method)` | Get/set HTTP method |
| `req.getHeader(name)` / `req.setHeader(name, value)` | Get/set headers |
| `req.getHeaders()` / `req.setHeaders(obj)` | Get/set all headers |
| `req.deleteHeader(name)` | Remove a header |
| `req.getBody()` / `req.setBody(data)` | Get/set request body |
| `req.getHost()` / `req.getPath()` | Get URL parts |
| `req.headerList` | Full PropertyList interface |

**`res` object — Response (Post-Response only)**

| Method | Description |
|--------|-------------|
| `res.status` / `res.getStatus()` | HTTP status code |
| `res.body` / `res.getBody()` | Parsed response body |
| `res.headers` / `res.getHeaders()` | Response headers object |
| `res.getHeader(name)` | Get specific response header |
| `res.responseTime` / `res.getResponseTime()` | Response time in ms |
| `res.getSize()` | Response size in bytes |
| `res.getRawBody()` | Raw response string |

**Utilities**

| Method | Description |
|--------|-------------|
| `console.log(value)` | Log to Flashpost output panel |
| `atob(str)` / `btoa(str)` | Base64 decode/encode |
| `fp.interpolate("{{$randomFirstName}}")` | Generate random data |
| `fp.sha256(data)` | SHA-256 hash (hex string) |
| `fp.md5(data)` | MD5 hash (hex string) |
| `fp.hmacSha256(data, key)` | HMAC-SHA256 (hex string) |

### Example: AWS Lambda Function URL / CloudFront OAC
```javascript
// Add x-amz-content-sha256 header (required for Lambda Function URL / CloudFront OAC)
const body = req.getBody({ raw: true }) || "";
req.setHeader("x-amz-content-sha256", fp.sha256(body));
```

### Example: Pre-Request Script
```javascript
// Add a timestamp header
req.setHeader("X-Request-Time", new Date().toISOString());

// Set auth token from environment
const token = fp.getEnvVar("auth_token");
req.setHeader("Authorization", "Bearer " + token);

console.log("Sending to:", req.getUrl());
```

### Example: Post-Response Script
```javascript
// Save token from response
const body = res.getBody();
if (body.access_token) {
  fp.setEnvVar("auth_token", body.access_token);
}

console.log("Status:", res.getStatus());
console.log("Time:", res.getResponseTime(), "ms");
```

### Notes
- Scripts run in a sandboxed environment (5 second timeout)
- Script modifications to `req` only affect the current execution — they are NOT saved to the database
- Variable changes via `fp.setEnvVar` / `fp.setGlobalEnvVar` ARE persisted
- Output from `console.log` appears in the Flashpost output panel (View > Output > Flashpost)
- `request` and `response` are aliases for `req` and `res`
- `postman.setEnvironmentVariable(key, value)` / `postman.getEnvironmentVariable(key)` are supported for Postman compatibility
- `CryptoJS` is available in scripts (SHA256, HmacSHA256, enc.Base64, etc.)
- `crypto` is also available with both Node.js and CryptoJS-style APIs

### Collection & Folder Scripts

Scripts can be defined at the collection or folder level via **Settings → Scripts**. These run automatically for all requests in the collection/folder:

- **Execution order (Pre-Request):** Collection script → Folder script → Request script
- **Execution order (Post-Response):** Request script → Folder script → Collection script
- When importing from Postman, collection/folder-level scripts are preserved in Settings

### Collection Environment

Attach an environment to a collection via **Collection Settings → Environment**:

- Requests in the collection automatically use the attached environment's variables
- Overrides the active (starred) environment for that collection
- Variable highlighting in request panels reflects the attached environment
- Changes are applied in real-time — no need to reload requests
- Variables set by scripts are saved to the attached environment (not the active one)
- Keyboard shortcut: **Ctrl+S / Cmd+S** to save Collection/Folder/Environment settings

### Script-Based Testing (`fp.test` / `fp.expect`)

Write assertions in Post-Response scripts using a Chai-style API. Results appear in the **Tests** tab.

**`fp.test(name, fn)`** — Define a named test case

**`fp.expect(value)`** — Create a chainable assertion

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
📋 **[Complete Script API Reference](https://github.com/subasraj/flashpost-support/blob/main/SCRIPT-API.md)**

### Quick Environment Switch

Switch the active environment without leaving the editor:

- **Command Palette:** `Flashpost: Change Environment`
- **Keyboard shortcut:** `Ctrl+Shift+E` (Windows/Linux) / `Cmd+Shift+E` (macOS)
- Shows all environments with the current active one marked ⭐
- Updates the sidebar, open request panels, and variable highlighting immediately

## 🏃‍♂️ Collection Runner

Execute multiple requests in sequence with the built-in collection runner.

- Right-click on any folder or collection and select "Run All"
- Automatically executes all requests and test cases
- Executes pre-request and post-response scripts at collection, folder, and request levels
- View comprehensive results and test outcomes

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-runtests.png?raw=true" alt="Collection Runner"/>
</div>

## 💾 Saved Response Examples

Save API responses as reusable examples directly under requests — similar to Postman's Examples feature.

### How It Works

1. Send a request and receive a response
2. Click the **"Save Response"** button in the response panel toolbar
3. The response is saved as a child node under the request in the collection tree
4. The saved example shows its HTTP status badge (e.g., `200`, `404`) in the sidebar

### Features

- **Full snapshot** — Both request and response data are persisted (URL, method, headers, body, status, timing)
- **Open in tab** — Click an example to open it in its own panel with the full request and response data
- **Drag-and-drop** — Examples can be reordered within their parent request
- **Duplicate** — When duplicating a request, its examples are also duplicated
- **Delete cascade** — Deleting a request also removes all its saved examples
- **Rename** — Right-click an example to rename it; open tabs update automatically
- **Workspace mode** — Examples are exported as part of `responses.json` for git-friendly storage

## ⏱️ Response Timing Breakdown

Hover over the **Time** value in the response status bar to see a detailed breakdown of the request timing phases.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-timing-breakdown.png?raw=true" alt="Response Timing Breakdown"/>
</div>

The tooltip shows:
- **DNS Lookup** — Time to resolve the domain name
- **TCP Handshake** — Time to establish a TCP connection
- **SSL Handshake** — Time to complete TLS negotiation (HTTPS only)
- **Transfer** — Server processing time + response download

Each phase includes a proportional colored bar for quick visual reference. Phases that use a cached/reused connection show "Cache".

## 📊 Response Size Breakdown

Hover over the **Size** value in the response status bar to see a complete breakdown of request and response sizes.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-size-breakdown.png?raw=true" alt="Response Size Breakdown"/>
</div>

The tooltip shows:
- **↓ Response Size** — Total response size with headers and body breakdown
- **↑ Request Size** — Total request size with headers and body breakdown

## ✂️ Copy & Paste (Collections)

Copy and paste items in the collection tree using keyboard shortcuts or the right-click context menu.

- **Cmd+C / Ctrl+C** — Copy the selected collection, folder, or request
- **Cmd+V / Ctrl+V** — Paste into the focused folder/collection (or as sibling if focused on a request)
- Context menu: **Copy** and **Paste** options available on collections, folders, requests, and examples
- Pasting a folder/collection creates a deep copy including all children and examples
- Examples can only be pasted within their parent request

## 🧪 Scriptless Testing

Test your APIs without writing a single line of test code.

- GUI-based testing interface eliminates boilerplate code
- Visual test creation and management
- No scripting knowledge required

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-scriptless-testing.png?raw=true" alt="Scriptless Testing"/>
</div>

## 💻 Code Generation

Generate ready-to-use code snippets in multiple programming languages:

### Supported Languages & Libraries
- **C#** - HttpClient, RestSharp
- **Go** - Native HTTP client
- **Java** - AsyncHttp, Unirest, OkHttp, NetHttp
- **JavaScript** - Axios, Fetch, jQuery, XMLHttpRequest
- **PHP** - cURL and native implementations
- **Python** - Requests library
- **Shell** - cURL commands

Click the `</>` icon in the request view to access the Code tab and copy your generated snippets.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-code-snippet.png?raw=true" alt="Code Generation"/>
</div>

## 🔧 Environment Variables

Dynamically set environment variables from API responses, headers, and cookies.

- Extract values from response data automatically
- Set variables from headers and cookies
- Use variables across different requests in your collection

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-env-var-from-response.png?raw=true" alt="Environment Variables"/>
</div>

## ⚙️ Extension Settings

Customize Flashpost to fit your workflow:

- **Custom Data Location** - Choose where to store your collections and data
- **Save to Workspace** - Store request data in your current workspace as git-friendly JSON files
- **Workspace Relative Path** - Set a custom relative path for workspace data
- **History Limit** - Control how many historical requests to keep

Access settings by clicking the gear icon in the top-right corner of the sidebar.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-extension-settings.png?raw=true" alt="Extension Settings"/>
</div>

## 💾 Workspace Storage (Git-Friendly)

When **"Save Data To Workspace"** is enabled, Flashpost stores all data as human-readable JSON files instead of a binary SQLite database. This makes your API collections git-friendly — you can commit, diff, and share them with your team.

**Files saved to your workspace:**
```
flashpost-tests/
  requests.json           ← All API requests (commit to git)
  collection_tree.json    ← Collection/folder hierarchy (commit to git)
  history.json            ← Request history (commit to git)
  variables.json          ← Environment variables (commit to git)
  cookies.json            ← Cookie storage (commit to git)
  cookies_v2.json         ← Cookie jar (individual cookies with attributes)
  responses.json          ← Saved response examples (commit to git)
  user_preferences.json   ← User preferences (commit to git)
```

- All files are formatted JSON (2-space indent) for clean diffs
- SQLite is used in-memory for fast queries — JSON files are the persistence layer
- No binary files — everything is readable and mergeable
- Team members opening the project automatically load the shared data

## 📜 History Management

- New requests will be saved in History tab
- Number of history items to display can be controlled by modifying “History Limit” extension setting.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-history.png?raw=true" alt="Request History"/>
</div>

## 🌍 Environment Management

Manage multiple environments (development, staging, production) with ease.

- Switch between environments quickly
- Set active environment with one click
- Environment-specific variable management

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-environment-setactive.png?raw=true" alt="Environment Management"/>
</div>

## 🎲 Dynamic Variables

Flashpost includes powerful dynamic variable generation using the [Faker.js library](https://www.npmjs.com/package/@faker-js/faker).

### Features
- Generate random sample data on-the-fly
- Names, addresses, emails, phone numbers, and much more
- Variables regenerate with each request execution
- Perfect for testing with realistic data

### Usage
Dynamic variables start with a `$` symbol and are evaluated at runtime:

```
$guid          // Generates a unique GUID
$timestamp     // Current timestamp
$randomName    // Random person name
$randomEmail   // Random email address
$randomPhone   // Random phone number
```

📋 **[Complete Dynamic Variables Reference](https://github.com/subasraj/flashpost-support/blob/main/random-variables.md)**

## 🚀 Migration from Other Tools

### From Postman
1. Export your Postman collections and environments
2. In Flashpost, click the import icon
3. Select your exported Postman files
4. Choose multiple collections and environments to import at once

### From Thunder Client
1. Export your Thunder Client data
2. Use Flashpost's import feature
3. Select Thunder Client format
4. Import collections and environments seamlessly

## 📋 System Requirements

- **VS Code Version**: 1.109.0 or higher
- **Operating System**: Windows, macOS, or Linux
- **Node.js**: Not required (extension is self-contained)

## 📄 License

This extension is licensed under the [MIT License](LICENSE).

## 🐛 Issues & Support

- **Bug Reports**: [GitHub Issues](https://github.com/subasraj/flashpost-support/issues)
- **Feature Requests**: [GitHub Discussions](https://github.com/subasraj/flashpost-support/discussions)
- **Documentation**: [Support Repository](https://github.com/subasraj/flashpost-support)

## ⭐ Show Your Support

If Flashpost helps you in your development workflow, please consider:
- ⭐ Starring the project
- 📝 Writing a review on the VS Code Marketplace
- 🐦 Sharing with your developer community

---

**Happy API Testing! 🚀**
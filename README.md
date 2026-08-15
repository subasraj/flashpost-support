# Flashpost - REST API Client for VS Code

[![Version](https://vsmarketplacebadges.dev/version/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Installs](https://vsmarketplacebadges.dev/installs/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)
[![Rating](https://vsmarketplacebadges.dev/rating/VASubasRaj.flashpost.svg)](https://marketplace.visualstudio.com/items?itemName=VASubasRaj.flashpost)

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
- **🍪 Cookie Management** - Automatic cookie handling and management
- **📊 Collection Runner** - Execute multiple requests in batch
- **🔍 GraphQL Support** - Built-in GraphQL query support

## 🚀 Quick Start

1. **Install the Extension** - Search for "Flashpost" in the VS Code Extensions marketplace
2. **Open Flashpost** - Click the Flashpost icon in the Activity Bar
3. **Create Your First Request** - Click "New Request" in the Quick Access sidebar
4. **Test Your API** - Enter your endpoint, configure headers, and hit send!

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

**`fp` / `bru` / `pm` object — Variable Management**

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

## 🏃‍♂️ Collection Runner

Execute multiple requests in sequence with the built-in collection runner.

- Right-click on any folder or collection and select "Run All"
- Automatically executes all requests and test cases
- View comprehensive results and test outcomes

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-runtests.png?raw=true" alt="Collection Runner"/>
</div>

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
- **Save to Workspace** - Store request data in your current workspace
- **Workspace Relative Path** - Set a custom relative path for workspace data
- **History Limit** - Control how many historical requests to keep

Access settings by clicking the gear icon in the top-right corner of the sidebar.

<div align="center">
  <img src="https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-extension-settings.png?raw=true" alt="Extension Settings"/>
</div>

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
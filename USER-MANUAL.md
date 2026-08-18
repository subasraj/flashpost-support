# Flashpost User Manual

**Version 2.0** | REST API Client for Visual Studio Code

---

## Table of Contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Getting Started](#getting-started)
4. [Interface Overview](#interface-overview)
5. [Making API Requests](#making-api-requests)
6. [Request Configuration](#request-configuration)
7. [Collections](#collections)
8. [Folders](#folders)
9. [Environment Variables](#environment-variables)
10. [Authentication](#authentication)
11. [Request Body](#request-body)
12. [Scriptless Testing](#scriptless-testing)
13. [Script-Based Testing](#script-based-testing)
14. [Pre-Request & Post-Response Scripts](#pre-request--post-response-scripts)
15. [Set Variables from Response](#set-variables-from-response)
16. [Collection Runner](#collection-runner)
17. [Code Snippet Generation](#code-snippet-generation)
18. [Cookie Management](#cookie-management)
19. [GraphQL Support](#graphql-support)
20. [Dynamic Variables](#dynamic-variables)
21. [Import & Export](#import--export)
22. [cURL Integration](#curl-integration)
23. [History](#history)
24. [Extension Settings](#extension-settings)
25. [Workspace Storage](#workspace-storage)
26. [Keyboard Shortcuts](#keyboard-shortcuts)
27. [Troubleshooting](#troubleshooting)

---

## Introduction

Flashpost is a lightweight REST API client built as a VS Code extension. It allows developers to create, test, and manage HTTP requests without leaving their IDE. Flashpost is designed for speed and simplicity, offering all the essential features of a modern API client with minimal overhead.

Flashpost stores all data locally using SQLite (via WebAssembly), ensuring your API data stays on your machine. You can optionally save data to your workspace for version control and team sharing.

---

## Installation

1. Open VS Code
2. Go to the Extensions panel (`Ctrl+Shift+X` / `Cmd+Shift+X`)
3. Search for **"Flashpost"**
4. Click **Install**
5. The Flashpost icon appears in your Activity Bar (left sidebar)

**Requirements:**
- VS Code version 1.109.0 or higher
- Works on Windows, macOS, and Linux

![Flashpost Install](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-search.gif?raw=true)

---

## Getting Started

1. Click the **Flashpost icon** in the Activity Bar to open the sidebar
2. Click **"New Request"** at the top of the sidebar
3. Enter a URL in the address bar (e.g., `https://jsonplaceholder.typicode.com/posts/1`)
4. Select the HTTP method (GET, POST, PUT, DELETE, etc.)
5. Click **Send**
6. View the response in the right panel

---

## Interface Overview

Flashpost has three main areas:

### Sidebar (Left Panel)

The sidebar contains three tabs:

- **History** - Shows recently executed requests
- **Collections** - Organized groups of requests with folders
- **Environment** - Manage environment variables

### Request Panel (Center)

The main workspace where you configure and send requests:

- **URL bar** - Enter the endpoint URL
- **Method selector** - Choose HTTP method (GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD)
- **Send button** - Execute the request
- **Tabs** - Params, Auth, Headers, Body, Tests, Set Variables, Script

### Response Panel (Right)

Displays the API response:

- **Status** - HTTP status code
- **Time** - Response time in milliseconds
- **Size** - Response payload size
- **Response tab** - Response body (JSON, XML, HTML, text, or raw)
- **Headers tab** - Response headers
- **Cookies tab** - Response cookies
- **Test Results tab** - Test execution results

---

## Making API Requests

### Basic GET Request

1. Click **New Request**
2. Keep the method as **GET**
3. Enter the URL: `https://api.example.com/users`
4. Click **Send**

### POST Request with Body

1. Click **New Request**
2. Change the method to **POST**
3. Enter the URL: `https://api.example.com/users`
4. Click the **Body** tab
5. Select **JSON** format
6. Enter your JSON payload:
   ```json
   {
     "name": "John Doe",
     "email": "john@example.com"
   }
   ```
7. Click **Send**

### Saving a Request

After sending a request:
- The request is automatically saved to **History**
- To save to a **Collection**, use the **Save** button or right-click the history item and select "Save to Collection"

---

## Request Configuration

### Params Tab

Add query parameters to your URL:

| Field | Description |
|-------|-------------|
| Key | Parameter name |
| Value | Parameter value |
| Checkbox | Enable/disable individual parameters |

Parameters are automatically appended to the URL. You can use environment variables with `{{variable_name}}` syntax.

### Path Parameters

Path parameters are automatically detected from URLs using the `:param` syntax:
```
https://api.example.com/users/:userId/posts/:postId
```

### Headers Tab

Add custom HTTP headers to your request:

| Field | Description |
|-------|-------------|
| Key | Header name (e.g., `Content-Type`) |
| Value | Header value (e.g., `application/json`) |
| Checkbox | Enable/disable individual headers |

### Bulk Edit

Toggle **Bulk Edit** mode to edit parameters or headers as plain text (one `key:value` per line).

---

## Collections

Collections are organized groups of API requests. They help you manage requests by project, feature, or workflow.

### Creating a Collection

1. Go to the **Collections** tab in the sidebar
2. Click the hamburger menu icon
3. Select **"New Collection"**
4. Enter a name for your collection

![Collections](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-new-collection.png?raw=true)

### Collection Settings

Right-click on a collection to access settings:

- **Authorization** - Set default auth for all requests in the collection
- **Headers** - Define shared headers for the collection
- **Tests** - Collection-level scriptless tests
- **Scripts** - Pre-request and post-response scripts that run for all requests
- **Environment** - Attach a specific environment to the collection

### Collection Operations

Right-click on a collection for these options:
- Rename
- Duplicate
- Delete
- New Folder
- New Request
- Run All (Collection Runner)
- Export
- Settings

### Drag and Drop

- **Requests** can be moved between folders and between collections
- **Folders** can be moved between other folders and between collections
- **Collections** can be reordered among themselves
- Requests at collection level are always positioned after all folders
- Requests within a folder can be freely reordered

---

## Folders

Folders provide sub-organization within collections.

### Creating a Folder

1. Right-click on a collection or an existing folder
2. Select **"New Folder"**
3. Enter the folder name

### Folder Behavior

- Folders always appear before requests at the same level
- When a new folder is created, it's automatically positioned before existing requests
- Folders can contain both sub-folders and requests
- Folder-level settings (auth, headers, scripts) are inherited by child requests

---

## Environment Variables

Environment variables let you store values that can be reused across requests. They're perfect for managing API keys, base URLs, tokens, and other configuration values that change between environments.

### Creating an Environment

1. Go to the **Environment** tab in the sidebar
2. Click the hamburger menu icon
3. Select **"New Environment"**
4. Name it (e.g., "Development", "Staging", "Production")
5. Add key-value pairs

### Using Variables

Reference variables in any input field using double curly braces:
```
{{base_url}}/api/users
{{auth_token}}
```

Variables with values are highlighted in **green**. Variables without values are highlighted in **red**.

### Setting Active Environment

- Click the star icon next to an environment to set it as active
- The active environment's variables are used when sending requests
- Use the keyboard shortcut `Ctrl+Shift+E` / `Cmd+Shift+E` to quickly switch environments

### Collection-Attached Environment

You can attach a specific environment to a collection:

1. Open Collection Settings
2. Go to the **Environment** tab
3. Select an environment from the dropdown

When attached, requests in that collection use the attached environment instead of the active (starred) one.

### Global Variables

The **Global** environment is always available and its variables are accessible from any request regardless of which environment is active.

### .env File Support

When using workspace storage, Flashpost can read variables from your project's `.env` file. Enable this in extension settings with "Use Env File From Workspace".

![Environment Variables](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-env-var-from-response.png?raw=true)

---

## Authentication

Flashpost supports multiple authentication types:

### No Auth
No authentication is applied.

### Basic Authentication
Enter a username and password. Flashpost encodes them as Base64 and adds the `Authorization: Basic` header.

### Bearer Token
Enter a token value and optional prefix (default: "Bearer"). Flashpost adds the `Authorization: Bearer <token>` header.

### OAuth 2.0
Configure OAuth 2.0 authentication with:
- Grant Type
- Access Token URL
- Client ID
- Client Secret
- Scope

### AWS Signature
Configure AWS Signature v4 authentication for AWS services:
- Access Key
- Secret Key
- Region
- Service

### Inherit
Inherits the auth configuration from the parent collection or folder. Useful for applying the same auth across multiple requests.

---

## Request Body

Flashpost supports multiple body formats:

### JSON
Write or paste JSON content. Syntax highlighting and formatting are available.

### Form (URL Encoded)
Add key-value pairs for `application/x-www-form-urlencoded` submissions.

### Form Data (Multipart)
Add key-value pairs or file uploads for `multipart/form-data` submissions. Supports:
- Text fields
- File uploads

### XML
Write XML content with syntax highlighting.

### Text
Send plain text content.

### Binary
Upload a binary file as the request body.

### GraphQL
Write GraphQL queries with a separate variables section.

---

## Scriptless Testing

Test your APIs without writing code using the GUI-based testing interface.

### Creating Tests

1. Open a request
2. Click the **Tests** tab
3. Add test assertions using the visual builder

### Test Assertions

Available checks:
- **Response Status** - Check status code (equals, not equals, greater than, less than)
- **Response Body** - Check if body contains, equals, or matches a pattern
- **Response Headers** - Check header values
- **Response Time** - Assert response time is under a threshold
- **JSON Path** - Extract and validate specific JSON values

### Running Tests

Tests execute automatically when you send the request. Results appear in the **Test Results** tab of the response panel.

![Scriptless Testing](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-scriptless-testing.png?raw=true)

---

## Script-Based Testing

For more complex test scenarios, use the `fp.test()` and `fp.expect()` API in Post-Response scripts.

### Writing Tests

```javascript
fp.test("Status is 200", () => {
  fp.expect(res.getStatus()).to.equal(200);
});

fp.test("Response has data array", () => {
  const body = res.getBody();
  fp.expect(body).to.have.property("data");
  fp.expect(body.data).to.be.an("array");
});

fp.test("Response time under 500ms", () => {
  fp.expect(res.getResponseTime()).to.be.below(500);
});
```

### Available Assertions

| Assertion | Description |
|-----------|-------------|
| `.to.equal(val)` | Strict equality |
| `.to.eql(val)` | Deep equality (objects/arrays) |
| `.to.include(val)` | Contains substring/element/key |
| `.to.be.a(type)` | Type check |
| `.to.be.above(n)` / `.below(n)` | Numeric comparisons |
| `.to.be.within(min, max)` | Range check |
| `.to.be.ok` / `.true` / `.false` / `.null` / `.empty` | Truthiness |
| `.to.have.property(key)` | Property exists |
| `.to.have.length(n)` | Array/string length |
| `.to.have.status(code)` | Response status shorthand |
| `.to.have.header(name)` | Response header exists |
| `.not` | Negate any assertion |

---

## Pre-Request & Post-Response Scripts

### Overview

Scripts let you execute JavaScript code before or after a request:

- **Pre-Request Scripts** - Modify the request, set headers, compute signatures
- **Post-Response Scripts** - Extract data, set variables, validate responses

### Script Levels

Scripts can be defined at three levels:
1. **Collection level** - Runs for all requests in the collection
2. **Folder level** - Runs for all requests in the folder
3. **Request level** - Runs for that specific request only

**Execution order (Pre-Request):** Collection → Folder → Request  
**Execution order (Post-Response):** Request → Folder → Collection

### Script API

**Variable Management (`fp` object):**

| Method | Description |
|--------|-------------|
| `fp.getEnvVar(key)` | Get environment variable |
| `fp.setEnvVar(key, value)` | Set environment variable (persisted) |
| `fp.getGlobalEnvVar(key)` | Get global variable |
| `fp.setGlobalEnvVar(key, value)` | Set global variable (persisted) |
| `fp.getVar(key)` / `fp.setVar(key, value)` | Runtime variables (session only) |
| `fp.interpolate(string)` | Resolve `{{variables}}` |

**Request (`req` object - Pre-Request only):**

| Method | Description |
|--------|-------------|
| `req.getUrl()` / `req.setUrl(url)` | Get/set request URL |
| `req.getMethod()` / `req.setMethod(method)` | Get/set HTTP method |
| `req.getHeader(name)` / `req.setHeader(name, value)` | Get/set headers |
| `req.getBody()` / `req.setBody(data)` | Get/set body |

**Response (`res` object - Post-Response only):**

| Method | Description |
|--------|-------------|
| `res.getStatus()` | HTTP status code |
| `res.getBody()` | Parsed response body |
| `res.getHeaders()` / `res.getHeader(name)` | Response headers |
| `res.getResponseTime()` | Time in milliseconds |

**Utilities:**

| Method | Description |
|--------|-------------|
| `console.log(value)` | Log to Flashpost output panel |
| `fp.sha256(data)` | SHA-256 hash |
| `fp.md5(data)` | MD5 hash |
| `fp.hmacSha256(data, key)` | HMAC-SHA256 |
| `atob(str)` / `btoa(str)` | Base64 decode/encode |
| `CryptoJS` | Full CryptoJS library |

**Compatibility:** `bru`, `pm`, and `tc` are aliases for `fp` (compatible with Bruno, Postman, and Thunder Client scripts).

### Example: Dynamic Auth Header

```javascript
// Pre-Request: Add timestamp and signature
const timestamp = Date.now().toString();
const body = req.getBody({ raw: true }) || "";
const signature = fp.hmacSha256(body + timestamp, fp.getEnvVar("secret_key"));

req.setHeader("X-Timestamp", timestamp);
req.setHeader("X-Signature", signature);
```

### Example: Save Token from Response

```javascript
// Post-Response: Save auth token for subsequent requests
const body = res.getBody();
if (body.access_token) {
  fp.setEnvVar("auth_token", body.access_token);
  fp.setEnvVar("token_expires", body.expires_in);
  console.log("Token saved successfully");
}
```

---

## Set Variables from Response

The **Set Variables** tab provides a GUI for extracting values from responses and saving them as environment variables — no scripting required.

### How It Works

1. Open a request and click the **Set Variables** tab
2. Add a row specifying:
   - **Variable Name** - The environment variable to set
   - **Source** - Where to extract from (Body JSON Path, Header, Cookie)
   - **Path/Key** - The JSON path or header/cookie name
3. Send the request
4. Values are automatically extracted and saved to your active environment

---

## Collection Runner

Execute multiple requests in sequence to test entire workflows.

### Running a Collection

1. Right-click on a collection or folder in the sidebar
2. Select **"Run All"**
3. Configure runner settings:
   - Select which requests to include
   - Set iteration count
   - Choose delay between requests
4. Click **Run**
5. View results including pass/fail status for each request and test

![Collection Runner](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-runtests.png?raw=true)

---

## Code Snippet Generation

Generate ready-to-use code snippets from your configured requests.

### Supported Languages

| Language | Libraries |
|----------|-----------|
| C# | HttpClient, RestSharp |
| Go | Native HTTP |
| Java | AsyncHttp, Unirest, OkHttp, NetHttp |
| JavaScript | Axios, Fetch, jQuery, XMLHttpRequest |
| PHP | cURL |
| Python | Requests |
| Shell | cURL |

### Usage

1. Configure your request (URL, headers, body, auth)
2. Click the **`</>`** (code) icon in the request panel
3. Select your target language and library
4. Copy the generated code

![Code Generation](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-code-snippet.png?raw=true)

---

## Cookie Management

Flashpost automatically handles cookies from API responses.

### Viewing Cookies

- Response cookies appear in the **Cookies** tab of the response panel
- All stored cookies are accessible via **Manage Cookies** (from the sidebar menu `...`)

### Cookie Behavior

- Cookies are stored locally and sent automatically with subsequent requests to the same domain
- You can manually add, edit, or delete cookies
- Cookie storage persists across sessions

---

## GraphQL Support

### Sending GraphQL Requests

1. Create a new request with method **POST**
2. Set the URL to your GraphQL endpoint
3. Click the **Body** tab
4. Select **GraphQL**
5. Write your query:
   ```graphql
   query {
     users {
       id
       name
       email
     }
   }
   ```
6. Add variables if needed (in the Variables section below the query)
7. Click **Send**

---

## Dynamic Variables

Generate random data at runtime using built-in dynamic variables. Variables start with `$` and regenerate with each request.

### Common Dynamic Variables

| Variable | Example Output |
|----------|----------------|
| `{{$guid}}` | `a1b2c3d4-e5f6-7890-abcd-ef1234567890` |
| `{{$timestamp}}` | `1629300000` |
| `{{$randomInt}}` | `42` |
| `{{$randomFirstName}}` | `Alice` |
| `{{$randomLastName}}` | `Johnson` |
| `{{$randomFullName}}` | `Bob Smith` |
| `{{$randomEmail}}` | `user@example.com` |
| `{{$randomPhoneNumber}}` | `+1-555-0123` |
| `{{$randomStreetAddress}}` | `123 Main St` |
| `{{$randomCity}}` | `Portland` |
| `{{$randomCountry}}` | `Canada` |
| `{{$randomCompanyName}}` | `Acme Corp` |
| `{{$randomJobTitle}}` | `Software Engineer` |
| `{{$randomUUID}}` | `550e8400-e29b-41d4-a716-446655440000` |
| `{{$randomBoolean}}` | `true` |
| `{{$randomColor}}` | `blue` |

Dynamic variables are powered by the [Faker.js](https://www.npmjs.com/package/@faker-js/faker) library. A complete list is available in the [Dynamic Variables Reference](https://github.com/subasraj/flashpost-support/blob/main/random-variables.md).

---

## Import & Export

### Importing Collections

Flashpost supports importing from:

- **Postman** - Collection v2.1 and Environment JSON files
- **Thunder Client** - Collection and Environment exports
- **OpenAPI/Swagger** - API specification files
- **cURL** - Individual cURL commands

**To import:**
1. Click the hamburger menu in the Collections tab
2. Select the appropriate import option
3. Choose your file(s)
4. Collections and environments are imported with their full structure

### Exporting

- **Single Collection** - Right-click a collection → Export
- **All Collections/Variables** - Hamburger menu → "Export All Collections/Variables"
- Exports include all requests, folders, hierarchy, and environment variables

---

## cURL Integration

### Importing cURL Commands

1. Click the hamburger menu in the sidebar
2. Select **"Import/Run Curl"**
3. Paste your cURL command
4. Flashpost parses and populates the request configuration
5. Click **Send** to execute

### Example

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer token123" \
  -d '{"name":"John","email":"john@example.com"}'
```

This creates a POST request with the URL, headers, and body automatically configured.

---

## History

Every request you send is automatically saved in the History tab.

### History Features

- View recent requests with method, name, and date
- Click any history item to reopen and resend
- Right-click for context menu options:
  - Save to Collection
  - Rename
  - Delete
- History highlight syncs with the active request panel

### History Limit

Control how many history items to keep (25, 50, 75, 100, or All) via extension settings.

![History](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-history.png?raw=true)

---

## Extension Settings

Access settings via the gear icon in the sidebar or through VS Code Settings.

| Setting | Default | Description |
|---------|---------|-------------|
| Layout | Vertical Split | Request/response layout orientation |
| Horizontal Layout | Accordion Style | Style when using horizontal split |
| Custom Data Location | `$HOME/Documents/` | Folder path for storing data |
| Save Data To Workspace | false | Store data in workspace folder |
| Workspace Data Relative Path | (empty) | Custom path within workspace |
| Use Env File From Workspace | true | Read `.env` file from workspace |
| Default Protocol | http | Protocol added when URL has none |
| Timeout | 30 sec | Request timeout duration |
| History Limit | 25 | Number of history items to display |
| Default Sidebar Tab | Collections | Tab shown when sidebar opens |
| SSL Check | true | Enable strict SSL verification |

![Settings](https://github.com/subasraj/flashpost-support/blob/main/images/flashpost-extension-settings.png?raw=true)

---

## Workspace Storage

When **"Save Data To Workspace"** is enabled, all data is stored as human-readable JSON files in your project:

```
flashpost-tests/
  requests.json           - All API requests
  collection_tree.json    - Collection/folder hierarchy
  history.json            - Request history
  variables.json          - Environment variables
  cookies.json            - Cookie storage
  user_preferences.json   - User preferences
```

### Benefits

- **Git-friendly** - JSON files produce clean diffs
- **Team sharing** - Commit to your repo for team access
- **Portable** - No binary databases to manage
- **Readable** - Human-readable formatted JSON (2-space indent)

Team members who open the project automatically load the shared API data.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Shift+E` / `Cmd+Shift+E` | Change active environment |
| `Ctrl+S` / `Cmd+S` | Save current request/settings |
| `Delete` / `Backspace` | Delete selected sidebar item |
| `Enter` | Open selected sidebar item |
| `Arrow Up` / `Arrow Down` | Navigate sidebar items |

---

## Troubleshooting

### Request Fails with SSL Error

Disable strict SSL checking in settings:
- Open Extension Settings
- Uncheck **"SSL Check"**

### Data Not Loading

If your collections don't appear:
1. Check the custom data location in settings
2. Ensure the folder exists and is accessible
3. Try reloading: Hamburger menu → "Reload Collections"

### Variables Not Resolving

- Ensure the variable exists in the active environment (or attached collection environment)
- Check that the environment is set as active (starred)
- Variables highlighted in red indicate missing values

### Migrating Data

If you previously used an older version of Flashpost (with LokiJS), data is automatically migrated to SQLite on first launch. No action required.

### Viewing Logs

For debugging issues:
1. Open Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Search for **"Flashpost: Show Logs"**
3. View the output panel for error details

---

## Support

- **Bug Reports:** [GitHub Issues](https://github.com/subasraj/flashpost-support/issues)
- **Feature Requests:** [GitHub Discussions](https://github.com/subasraj/flashpost-support/discussions)
- **Documentation:** [Support Repository](https://github.com/subasraj/flashpost-support)
- **Script API Reference:** [SCRIPT-API.md](https://github.com/subasraj/flashpost-support/blob/main/SCRIPT-API.md)
- **Dynamic Variables:** [random-variables.md](https://github.com/subasraj/flashpost-support/blob/main/random-variables.md)

# Flashpost Script API Reference

Flashpost provides scripting capabilities in two execution contexts:
- **Pre Request** — Runs before the HTTP request is sent. Can modify the request.
- **Post Response** — Runs after the response is received. Can read the response and set variables.

Scripts run in a sandboxed JavaScript environment with a 5-second timeout.

## Global Objects

The following objects are available in both Pre Request and Post Response scripts:

| Object | Description |
|--------|-------------|
| `fp` | Flashpost API (variables, utilities) |
| `bru` | Alias for `fp` (Bruno compatibility) |
| `pm` | Alias for `fp` (Postman compatibility) |
| `tc` | Alias for `fp` (Thunder Client compatibility) |
| `req` | Request object |
| `res` | Response object (data available in Post Response only) |
| `console` | Logging (`log`, `info`, `warn`, `error`, `debug`) |

### Sub-object Access

Each alias also exposes `request` and `response` sub-objects:

```javascript
fp.request.getUrl()    // same as req.getUrl()
fp.response.getStatus() // same as res.getStatus()
bru.request.getUrl()    // same as req.getUrl()
bru.response.getStatus() // same as res.getStatus()
pm.request.getUrl()    // same as req.getUrl()
pm.response.getStatus() // same as res.getStatus()
tc.request.getUrl()    // same as req.getUrl()
tc.response.getStatus() // same as res.getStatus()
```

---

## fp / bru / pm / tc — Variable Management

### Environment Variables

Variables from the currently selected environment. Changes are persisted to the database.

| Method | Returns | Description |
|--------|---------|-------------|
| `fp.getEnvVar(key)` | `string \| undefined` | Get an environment variable value |
| `fp.setEnvVar(key, value)` | `void` | Set an environment variable (persisted) |
| `fp.hasEnvVar(key)` | `boolean` | Check if an environment variable exists |
| `fp.deleteEnvVar(key)` | `void` | Delete an environment variable (persisted) |
| `fp.getAllEnvVars()` | `object` | Get all environment variables as key-value object |
| `fp.deleteAllEnvVars()` | `void` | Delete all environment variables |
| `fp.getEnvName()` | `string` | Get the current environment name |

```javascript
// Example
fp.setEnvVar("base_url", "https://api.example.com");
const url = fp.getEnvVar("base_url");
console.log("Using:", url);
```

### Global Variables

Variables from the "Global" environment. Shared across all environments. Changes are persisted.

| Method | Returns | Description |
|--------|---------|-------------|
| `fp.getGlobalEnvVar(key)` | `string \| undefined` | Get a global variable |
| `fp.setGlobalEnvVar(key, value)` | `void` | Set a global variable (persisted) |
| `fp.hasGlobalEnvVar(key)` | `boolean` | Check if a global variable exists |
| `fp.deleteGlobalEnvVar(key)` | `void` | Delete a global variable |
| `fp.getAllGlobalEnvVars()` | `object` | Get all global variables as key-value object |
| `fp.deleteAllGlobalEnvVars()` | `void` | Delete all global variables |

```javascript
// Example
fp.setGlobalEnvVar("api_version", "v2");
console.log(fp.getGlobalEnvVar("api_version")); // "v2"
```

### Runtime Variables

Temporary variables that exist only during the current session. NOT persisted.

| Method | Returns | Description |
|--------|---------|-------------|
| `fp.getVar(key)` | `string \| undefined` | Get a runtime variable |
| `fp.setVar(key, value)` | `void` | Set a runtime variable |
| `fp.hasVar(key)` | `boolean` | Check if a runtime variable exists |
| `fp.deleteVar(key)` | `void` | Delete a runtime variable |
| `fp.deleteAllVars()` | `void` | Delete all runtime variables |
| `fp.getAllVars()` | `object` | Get all runtime variables as key-value object |

```javascript
// Example: pass data between pre-request and post-response
fp.setVar("request_time", Date.now().toString());

// In post-response:
const startTime = parseInt(fp.getVar("request_time"));
console.log("Custom duration:", Date.now() - startTime, "ms");
```

### Collection Variables

Variables scoped to the current collection.

| Method | Returns | Description |
|--------|---------|-------------|
| `fp.getCollectionVar(key)` | `string \| undefined` | Get a collection variable |
| `fp.setCollectionVar(key, value)` | `void` | Set a collection variable |
| `fp.hasCollectionVar(key)` | `boolean` | Check if a collection variable exists |
| `fp.deleteCollectionVar(key)` | `void` | Delete a collection variable |
| `fp.deleteAllCollectionVars()` | `void` | Delete all collection variables |

### Utilities

| Method | Returns | Description |
|--------|---------|-------------|
| `fp.interpolate(string)` | `string` | Resolve `{{variables}}` and `{{$dynamicVars}}` in a string |

```javascript
// Resolve environment variables
const url = fp.interpolate("{{base_url}}/api/users");

// Generate random data using Faker.js dynamic variables
const name = fp.interpolate("{{$randomFirstName}}");
const email = fp.interpolate("{{$randomEmail}}");
const uuid = fp.interpolate("{{$guid}}");
console.log(name, email, uuid);
```

#### Available Dynamic Variables

| Variable | Description |
|----------|-------------|
| `{{$guid}}` / `{{$randomUUID}}` | Random UUID |
| `{{$timestamp}}` | Current Unix timestamp |
| `{{$randomFirstName}}` | Random first name |
| `{{$randomLastName}}` | Random last name |
| `{{$randomFullName}}` | Random full name |
| `{{$randomEmail}}` | Random email address |
| `{{$randomPhoneNumber}}` | Random phone number |
| `{{$randomCity}}` | Random city name |
| `{{$randomCountry}}` | Random country name |
| `{{$randomStreetAddress}}` | Random street address |
| `{{$randomIP}}` | Random IPv4 address |
| `{{$randomInt}}` | Random integer (0-1000) |
| `{{$randomBoolean}}` | Random boolean |
| `{{$randomColor}}` | Random color name |
| `{{$randomCompanyName}}` | Random company name |
| `{{$randomJobTitle}}` | Random job title |
| `{{$randomCurrencyCode}}` | Random currency code |
| `{{$randomBankAccount}}` | Random bank account number |
| `{{$randomUserAgent}}` | Random user agent string |
| `{{$randomPassword}}` | Random password |
| `{{$randomImageUrl}}` | Random image URL |

---

## req — Request Object

Available in both Pre Request and Post Response scripts. Modifications only take effect in Pre Request.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `req.id` | `string` | Request ID |
| `req.name` | `string` | Request display name |
| `req.auth` | `object` | Auth configuration object |

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `req.getId()` | `string` | Get request ID |
| `req.getName()` | `string` | Get request name |
| `req.getAuth()` | `object` | Get auth configuration |
| `req.getAuthMode()` | `string` | Get auth type (`"basic"`, `"bearertoken"`, `"apikey"`, `"aws"`, `"oauth2"`, `"noauth"`) |
| `req.getUrl()` | `string` | Get the request URL |
| `req.setUrl(url)` | `void` | Set the request URL |
| `req.getHost()` | `string` | Get hostname from URL |
| `req.getPath()` | `string` | Get path from URL |
| `req.getQueryString()` | `string` | Get raw query string |
| `req.getMethod()` | `string` | Get HTTP method (uppercase) |
| `req.setMethod(method)` | `void` | Set HTTP method |
| `req.getHeader(name)` | `string \| undefined` | Get a request header (case-insensitive) |
| `req.getHeaders()` | `object` | Get all headers as key-value object |
| `req.setHeader(name, value)` | `void` | Set a request header |
| `req.setHeaders(headers)` | `void` | Set multiple headers from an object |
| `req.deleteHeader(name)` | `void` | Remove a header |
| `req.deleteHeaders(names)` | `void` | Remove multiple headers |
| `req.getBody(options?)` | `any` | Get request body. Pass `{ raw: true }` for raw string |
| `req.setBody(body)` | `void` | Set request body (string or object) |

### req.headerList — PropertyList Interface

Rich interface for header manipulation.

**Read:**
| Method | Returns | Description |
|--------|---------|-------------|
| `req.headerList.get(name)` | `string \| undefined` | Get header value |
| `req.headerList.one(name)` | `object \| undefined` | Get header object `{ key, value }` |
| `req.headerList.all()` | `object[]` | Get all headers |
| `req.headerList.count()` | `number` | Count of headers |

**Search:**
| Method | Returns | Description |
|--------|---------|-------------|
| `req.headerList.has(name)` | `boolean` | Check if header exists |
| `req.headerList.has(name, value)` | `boolean` | Check if header with exact value exists |
| `req.headerList.find(fn)` | `object \| undefined` | Find first matching header |
| `req.headerList.filter(fn)` | `object[]` | Filter headers |
| `req.headerList.indexOf(item)` | `number` | Index of header (-1 if not found) |

**Iteration:**
| Method | Returns | Description |
|--------|---------|-------------|
| `req.headerList.each(fn)` | `void` | Iterate over all headers |
| `req.headerList.map(fn)` | `any[]` | Map headers to new values |
| `req.headerList.reduce(fn, initial)` | `any` | Reduce headers to single value |

**Transform:**
| Method | Returns | Description |
|--------|---------|-------------|
| `req.headerList.toObject()` | `object` | Convert to `{ key: value }` map |
| `req.headerList.toString()` | `string` | HTTP wire format |
| `req.headerList.toJSON()` | `object[]` | JSON-serializable array |

**Write:**
| Method | Returns | Description |
|--------|---------|-------------|
| `req.headerList.add(name, value)` | `void` | Add/update a header |
| `req.headerList.upsert(name, value)` | `boolean \| null` | Add or replace (`true`=new, `false`=updated) |
| `req.headerList.remove(predicate)` | `void` | Remove by name, object, or predicate function |
| `req.headerList.clear()` | `void` | Remove all headers |

```javascript
// Example
req.setUrl("https://api.example.com/users");
req.setMethod("POST");
req.setHeader("Authorization", "Bearer " + fp.getEnvVar("token"));
req.setHeaders({
  "Content-Type": "application/json",
  "X-Request-Id": fp.interpolate("{{$guid}}")
});

console.log("Sending", req.getMethod(), "to", req.getUrl());
console.log("Headers count:", req.headerList.count());
```

---

## res — Response Object

Available in both contexts, but only contains data in **Post Response** scripts.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `res.status` | `number` | HTTP status code |
| `res.statusText` | `string` | HTTP status message |
| `res.headers` | `object` | Response headers (lowercase keys) |
| `res.body` | `any` | Parsed response body (JSON auto-parsed) |
| `res.responseTime` | `number` | Response time in milliseconds |
| `res.url` | `string` | Final response URL |

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `res.getStatus()` | `number` | Get status code |
| `res.getStatusText()` | `string` | Get status text |
| `res.getHeader(name)` | `string \| undefined` | Get response header (case-insensitive) |
| `res.getHeaders()` | `object` | Get all response headers |
| `res.getBody()` | `any` | Get parsed body (JSON → object, else string) |
| `res.getRawBody()` | `string` | Get raw body string |
| `res.setBody(body)` | `void` | Override body for downstream scripts |
| `res.getResponseTime()` | `number` | Get response time in ms |
| `res.getUrl()` | `string` | Get response URL |
| `res.getSize()` | `object` | Get size `{ body, headers, total }` in bytes |

### res.headerList — PropertyList Interface (Read-Only)

Same read/search/iteration/transform methods as `req.headerList`, but without write methods.

```javascript
// Example: Save token from response
const body = res.getBody();
if (body && body.access_token) {
  fp.setEnvVar("auth_token", body.access_token);
  console.log("Token saved! Expires in:", body.expires_in, "seconds");
}

console.log("Status:", res.getStatus(), res.getStatusText());
console.log("Time:", res.getResponseTime(), "ms");
console.log("Size:", res.getSize().total, "bytes");
console.log("Content-Type:", res.getHeader("content-type"));
```

---

## console — Logging

Output appears in the Flashpost output panel (View > Output > select "Flashpost").

| Method | Description |
|--------|-------------|
| `console.log(...args)` | Standard log output |
| `console.info(...args)` | Info-level log (prefixed with `[info]`) |
| `console.warn(...args)` | Warning log (prefixed with `[warn]`) |
| `console.error(...args)` | Error log (prefixed with `[error]`) |
| `console.debug(...args)` | Debug log (prefixed with `[debug]`) |

Objects are automatically JSON-serialized with 2-space indentation.

```javascript
console.log("Simple message");
console.log("Status:", res.getStatus());
console.log("Body:", res.getBody()); // objects are pretty-printed
console.warn("This is a warning");
console.error("Something went wrong:", "details here");
```

---

## Built-in Utilities

| Function | Description |
|----------|-------------|
| `atob(str)` | Decode Base64 string |
| `btoa(str)` | Encode string to Base64 |
| `JSON.parse(str)` | Parse JSON |
| `JSON.stringify(obj, null, 2)` | Serialize to JSON |
| `encodeURIComponent(str)` | URL-encode a string |
| `decodeURIComponent(str)` | URL-decode a string |
| `Date.now()` | Current timestamp in ms |
| `new Date()` | Current date object |
| `Math.*` | All Math methods |
| `new URL(str)` | Parse a URL |
| `new URLSearchParams(str)` | Parse query parameters |

---

## fp.test / fp.expect — Testing & Assertions

Flashpost includes a built-in Chai-style assertion library. Use `fp.test()` to define named test cases and `fp.expect()` to write assertions. Test results appear in the **Tests** tab of the response panel.

> These APIs are available in **Post Response** scripts. They also work in Pre Request scripts, but response data will be empty.

### fp.test(name, fn)

Wraps assertions in a named test case. If `fn` throws, the test fails; otherwise it passes.

```javascript
fp.test("Status is 200", () => {
  fp.expect(res.getStatus()).to.equal(200);
});

fp.test("Response has user data", () => {
  const body = res.getBody();
  fp.expect(body).to.have.property("id");
  fp.expect(body.name).to.be.a("string");
});
```

### fp.expect(value)

Creates a chainable assertion on `value`. Use chaining words `.to`, `.that`, `.which`, `.and` for readability (they are no-ops).

---

### Equality

| Assertion | Description |
|-----------|-------------|
| `.eql(expected)` | Deep equality (objects, arrays) |
| `.equal(expected)` | Strict equality (`===`) |
| `.deep.equal(expected)` | Deep equality (alias for `.eql`) |

```javascript
fp.expect(res.getStatus()).to.equal(200);
fp.expect(res.getBody()).to.eql({ success: true, id: 1 });
fp.expect(data).to.deep.equal(expected);
```

---

### Inclusion

| Assertion | Description |
|-----------|-------------|
| `.include(expected)` | String contains substring, array contains element, or object contains subset |
| `.deep.include(expected)` | Array deep-includes an item |
| `.oneOf(list)` | Value is one of the items in list |

```javascript
fp.expect("hello world").to.include("world");
fp.expect([1, 2, 3]).to.include(2);
fp.expect({ a: 1, b: 2 }).to.include({ a: 1 });
fp.expect([{id: 1}]).to.deep.include({id: 1});
fp.expect(res.getStatus()).to.oneOf([200, 201]);
```

---

### Type Checks

| Assertion | Description |
|-----------|-------------|
| `.be.a(type)` | Assert `typeof` value (or `"array"` / `"null"`) |
| `.be.an(type)` | Alias for `.be.a(type)` |

Valid types: `"string"`, `"number"`, `"boolean"`, `"object"`, `"array"`, `"null"`, `"undefined"`

```javascript
fp.expect("hello").to.be.a("string");
fp.expect([1,2]).to.be.an("array");
fp.expect(42).to.be.a("number");
```

---

### Numeric Comparisons

| Assertion | Description |
|-----------|-------------|
| `.be.above(n)` | Greater than `n` |
| `.be.below(n)` | Less than `n` |
| `.be.at.least(n)` | Greater than or equal to `n` |
| `.be.at.most(n)` | Less than or equal to `n` |
| `.be.within(min, max)` | Value is between `min` and `max` (inclusive) |

```javascript
fp.expect(res.getResponseTime()).to.be.below(500);
fp.expect(res.getStatus()).to.be.within(200, 299);
fp.expect(data.count).to.be.at.least(1);
```

---

### Truthiness

| Assertion | Description |
|-----------|-------------|
| `.be.ok` | Truthy value |
| `.be.true` | Strictly `true` |
| `.be.false` | Strictly `false` |
| `.be.null` | Strictly `null` |
| `.be.undefined` | Strictly `undefined` |
| `.be.NaN` | Is `NaN` |
| `.be.empty` | Empty array, string, or object (no keys) |

```javascript
fp.expect(body.active).to.be.true;
fp.expect(body.deletedAt).to.be.null;
fp.expect([]).to.be.empty;
fp.expect(body.name).to.be.ok;
```

---

### Property & Keys

| Assertion | Description |
|-----------|-------------|
| `.have.property(prop)` | Object has the named property |
| `.have.property(prop, value)` | Property exists and equals value |
| `.have.keys(keyList)` | Object has all specified keys |
| `.have.all.keys(keyList)` | Object has all specified keys |
| `.have.any.keys(keyList)` | Object has at least one of the keys |

```javascript
fp.expect(body).to.have.property("id");
fp.expect(body).to.have.property("status", "active");
fp.expect(body).to.have.keys(["id", "name", "email"]);
fp.expect(body).to.have.any.keys(["error", "message"]);
```

---

### Length & Members

| Assertion | Description |
|-----------|-------------|
| `.have.length(n)` | Array or string has length `n` |
| `.have.lengthOf(n)` | Alias for `.have.length(n)` |
| `.have.members(list)` | Array has the same members (order-independent) |

```javascript
fp.expect(body.items).to.have.length(10);
fp.expect([3, 1, 2]).to.have.members([1, 2, 3]);
```

---

### Response-Specific Assertions

| Assertion | Description |
|-----------|-------------|
| `.have.status(code)` | Assert response status code (number) or status text (string) |
| `.have.header(name)` | Assert response has the header |
| `.have.header(name, value)` | Assert header includes value |
| `.have.body(expected)` | Assert raw body equals string |
| `.have.jsonBody()` | Assert response has a JSON body |
| `.have.jsonBody(prop)` | Assert JSON body has a property |

```javascript
fp.test("Response checks", () => {
  fp.expect(res).to.have.status(200);
  fp.expect(res).to.have.header("content-type", "application/json");
  fp.expect(res).to.have.jsonBody("data");
});
```

---

### Negation (`.not`)

Prefix any assertion with `.not` to negate it.

| Assertion | Description |
|-----------|-------------|
| `.not.equal(val)` | Not strictly equal |
| `.not.eql(val)` | Not deep-equal |
| `.not.include(val)` | Does not contain |
| `.not.be.undefined` | Is defined |
| `.not.be.null` | Not null |
| `.not.be.empty` | Not empty |
| `.not.be.above(n)` | Not above `n` |
| `.not.be.below(n)` | Not below `n` |
| `.not.have.property(prop)` | Property does not exist |
| `.not.have.keys(keyList)` | None of the keys exist |
| `.not.have.any.keys(keyList)` | None of the keys exist |
| `.not.oneOf(list)` | Value is not in list |

```javascript
fp.expect(body.error).not.be.undefined;
fp.expect(res.getStatus()).not.equal(404);
fp.expect(body.items).not.be.empty;
fp.expect(body).not.have.property("password");
```

---

### Chaining

Assertions are chainable. Language chains (`.to`, `.that`, `.which`, `.and`) are for readability and have no effect.

```javascript
fp.test("Full response validation", () => {
  const body = res.getBody();

  fp.expect(res.getStatus()).to.equal(200);
  fp.expect(body).to.be.an("object").and.have.property("users");
  fp.expect(body.users).to.be.an("array").and.have.length(5);
  fp.expect(body.users[0]).to.have.all.keys(["id", "name", "email"]);
  fp.expect(res.getResponseTime()).to.be.below(1000);
});
```

---

### Complete Test Example

```javascript
fp.test("Status is 200 OK", () => {
  fp.expect(res.getStatus()).to.equal(200);
});

fp.test("Response time is acceptable", () => {
  fp.expect(res.getResponseTime()).to.be.below(2000);
});

fp.test("Body has expected structure", () => {
  const body = res.getBody();
  fp.expect(body).to.have.property("data");
  fp.expect(body.data).to.be.an("array");
  fp.expect(body.data).not.be.empty;
  fp.expect(body.data[0]).to.have.all.keys(["id", "name", "email"]);
});

fp.test("Pagination works", () => {
  const body = res.getBody();
  fp.expect(body.page).to.equal(1);
  fp.expect(body.total).to.be.at.least(1);
  fp.expect(body.data.length).to.be.at.most(20);
});
```

---

## Crypto Utilities

| Method | Returns | Description |
|--------|---------|-------------|
| `fp.sha256(data)` | `string` | SHA-256 hash (hex string) |
| `fp.md5(data)` | `string` | MD5 hash (hex string) |
| `fp.hmacSha256(data, key)` | `string` | HMAC-SHA256 (hex string) |

```javascript
// AWS Signature: content hash for Lambda Function URL / CloudFront OAC
const body = req.getBody({ raw: true }) || "";
req.setHeader("x-amz-content-sha256", fp.sha256(body));

// API request signing
const payload = JSON.stringify(req.getBody());
const signature = fp.hmacSha256(payload, fp.getEnvVar("secret_key"));
req.setHeader("X-Signature", signature);
```

---

## Security & Limitations

- Scripts run in a **sandboxed** Node.js `vm` context
- **5-second timeout** — scripts that take longer are terminated
- **No network access** — cannot make HTTP requests from scripts
- **No file system** — cannot read/write files
- **No `require()`** — cannot import modules
- **No `process`** — cannot access Node.js process
- **No async** — `Promise`, `setTimeout`, `setInterval` are blocked
- Script modifications to `req` (URL, headers, body, method) only affect the **current execution** — they are NOT saved to the database
- Variable changes via `fp.setEnvVar()` / `fp.setGlobalEnvVar()` ARE persisted to the database

---

## Examples

### Pre Request: Add authentication

```javascript
const token = fp.getEnvVar("access_token");
if (token) {
  req.setHeader("Authorization", "Bearer " + token);
} else {
  console.warn("No access token found!");
}
```

### Pre Request: Generate request body

```javascript
const body = {
  name: fp.interpolate("{{$randomFullName}}"),
  email: fp.interpolate("{{$randomEmail}}"),
  timestamp: new Date().toISOString()
};
req.setBody(body);
req.setHeader("Content-Type", "application/json");
```

### Post Response: Extract and save token

```javascript
const body = res.getBody();
if (res.getStatus() === 200 && body.access_token) {
  fp.setEnvVar("access_token", body.access_token);
  fp.setEnvVar("token_expiry", body.expires_in.toString());
  console.log("Token refreshed, expires in", body.expires_in, "seconds");
} else {
  console.error("Auth failed:", res.getStatus(), body.error || "Unknown error");
}
```

### Post Response: Conditional variable setting

```javascript
const data = res.getBody();
if (data && data.id) {
  fp.setEnvVar("last_created_id", data.id.toString());
  fp.setVar("user_id", data.id.toString()); // runtime only
}

// Log all response headers
res.headerList.each((header) => {
  console.log(header.key + ":", header.value);
});
```

### Pre Request: Dynamic URL construction

```javascript
const baseUrl = fp.getEnvVar("base_url") || "https://api.example.com";
const userId = fp.getEnvVar("user_id") || "1";
req.setUrl(baseUrl + "/users/" + userId);
console.log("Requesting:", req.getUrl());
```

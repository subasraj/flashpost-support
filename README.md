# Flashpost - REST API Client for VS Code

[![Visual Studio Marketplace](https://img.shields.io/visual-studio-marketplace/v/vasubasraj.flashpost)](https://marketplace.visualstudio.com/items?itemName=vasubasraj.flashpost)
[![Downloads](https://img.shields.io/visual-studio-marketplace/d/vasubasraj.flashpost)](https://marketplace.visualstudio.com/items?itemName=vasubasraj.flashpost)
[![Rating](https://img.shields.io/visual-studio-marketplace/r/vasubasraj.flashpost)](https://marketplace.visualstudio.com/items?itemName=vasubasraj.flashpost)

Flashpost is a lightweight REST API client extension that enables you to develop and test your REST APIs directly from Visual Studio Code. Perfect for developers who want to stay in their IDE without switching to external tools.

## ✨ Key Features

- **🚀 Lightweight & Fast** - Minimal overhead REST API client built specifically for VS Code
- **🎯 Simple Interface** - Clean, intuitive UI that gets out of your way
- **📁 Collections & Environments** - Organize your requests and manage multiple environments
- **📥 Import Support** - Seamlessly migrate from Postman and Thunder Client
- **🔄 cURL Integration** - Import and execute cURL commands directly
- **🧪 Scriptless Testing** - GUI-based testing without writing test scripts
- **💾 Local Storage** - All data stored locally with customizable storage location
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

- **VS Code Version**: 1.74.0 or higher
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

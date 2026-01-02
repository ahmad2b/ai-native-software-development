<p align="center">
  <img src="https://img.shields.io/badge/Gmail-MCP%20Server-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail MCP Server">
</p>

<h1 align="center">📧 Gmail MCP Server</h1>

<p align="center">
  <strong>A powerful Model Context Protocol (MCP) server for Gmail integration</strong>
  <br>
  Built with <a href="https://gofastmcp.com">FastMCP</a> • Python 3.10+
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-features">Features</a> •
  <a href="#-installation">Installation</a> •
  <a href="#-documentation">Documentation</a> •
  <a href="#-api-reference">API Reference</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/FastMCP-2.14+-00D4AA?style=flat-square" alt="FastMCP">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/MCP-Compatible-blueviolet?style=flat-square" alt="MCP">
</p>

---

## 🎯 Overview

Gmail MCP Server enables AI assistants like Claude to interact with Gmail through natural language. Send emails, search your inbox, manage labels, create filters, and more — all through a simple conversational interface.

### Why Gmail MCP Server?

- **🔐 Dual Authentication**: Choose between simple App Password setup or full OAuth for complete API access
- **🚀 Multiple Transports**: Run as stdio (Claude Desktop), HTTP REST API, or SSE streaming
- **📦 Easy Installation**: Install via pip, uvx, or run from source with uv
- **⚡ 19 Powerful Tools**: Comprehensive Gmail management capabilities
- **🛡️ Secure**: Credentials stored locally, no data leaves your machine

---

## 🚀 Quick Start

Choose your setup method:

| Method | Installation | Setup Time | Features |
|--------|:------------:|:----------:|----------|
| [SMTP + Cloud](#-option-1-smtp--cloud-fastest) | ❌ None | 2 min | Send, read, search, delete |
| [OAuth + Cloud](#-option-2-oauth--cloud-full-features) | ❌ None | 10 min | All 12 tools (drafts, labels, filters) |
| [Local Installation](#-option-3-local-installation) | ✅ Required | 15 min | All 19 tools + offline |

---

### ⚡ Option 1: SMTP + Cloud (Fastest)

**No installation required!** Basic email operations in 2 minutes.

#### Step 1: Create App Password

1. Go to [Google Account Security](https://myaccount.google.com/security)
2. Enable **2-Step Verification** if not already enabled
3. Go to [App Passwords](https://myaccount.google.com/apppasswords)
4. Select **Mail** → Generate → **Copy the 16-character password**

#### Step 2: Add to Claude Code

Choose your scope:

<details>
<summary><strong>🌍 User Scope (Recommended) - Available in all projects</strong></summary>

**macOS/Linux:**
```bash
claude mcp add gmail --scope user -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Email: your-email@gmail.com" \
  --header "X-Gmail-Password: your-16-char-app-password"
```

**Windows (PowerShell):** *(single line)*
```powershell
claude mcp add gmail --scope user -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp --header "X-Gmail-Email: your-email@gmail.com" --header "X-Gmail-Password: your-16-char-app-password"
```

</details>

<details>
<summary><strong>📁 Project Scope - Only in current project</strong></summary>

**macOS/Linux:**
```bash
claude mcp add gmail -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Email: your-email@gmail.com" \
  --header "X-Gmail-Password: your-16-char-app-password"
```

**Windows (PowerShell):** *(single line)*
```powershell
claude mcp add gmail -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp --header "X-Gmail-Email: your-email@gmail.com" --header "X-Gmail-Password: your-16-char-app-password"
```

</details>

**Done!** 🎉 Ask Claude: *"Send an email to john@example.com about our meeting"*

> **⚠️ Important:** If you have a `.mcp.json` file in your project with a `gmail` server, it may override your `claude mcp add` config. Either delete the conflicting entry from `.mcp.json` or use a different server name.

---

### 🔐 Option 2: OAuth + Cloud (Full Features)

**No installation required!** Full Gmail API access with drafts, labels, and filters.

#### Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **Select a project** → **New Project**
3. Name it (e.g., "Gmail MCP") → Click **Create**

#### Step 2: Enable Gmail API

1. Go to [Gmail API](https://console.cloud.google.com/apis/library/gmail.googleapis.com)
2. Click **Enable**

#### Step 3: Configure OAuth Consent Screen

1. Go to [Google Auth Platform](https://console.cloud.google.com/auth/overview)
2. If prompted, click **Get Started** or **Configure Consent Screen**
3. Select **External** → **Create**
4. Fill in **Branding** (left sidebar):
   - App name: `Gmail MCP`
   - User support email: *your email*
   - Developer contact: *your email*
   - Click **Save**
5. Go to **Data Access** (left sidebar):
   - Click **Add or Remove Scopes**
   - Search for `gmail` in the filter
   - Select these scopes:
     - `https://www.googleapis.com/auth/gmail.modify`
     - `https://www.googleapis.com/auth/gmail.settings.basic`
   - Click **Update** → **Save**
6. Go to **Audience** (left sidebar):
   - Click **Add Users**
   - Add your Gmail address
   - Click **Save**
7. Go to **Summary** (left sidebar):
   - Click **Publish App** to move from "Testing" to "In Production"
   - This allows your app to request full permissions needed for all Gmail MCP tools

#### Step 4: Create OAuth Credentials

1. Go to **Clients** (left sidebar) or [Credentials](https://console.cloud.google.com/apis/credentials)
2. Click **Create Client** or **Create Credentials** → **OAuth client ID**
3. Application type: **Web application**
4. Name: `Gmail MCP`
5. Under **Authorized redirect URIs**, click **Add URI** and enter:
   ```
   https://developers.google.com/oauthplayground
   ```
6. Click **Create**
7. **Copy your Client ID and Client Secret** (save them!)

#### Step 5: Get Refresh Token (OAuth Playground)

1. Go to [Google OAuth Playground](https://developers.google.com/oauthplayground/)
2. Click ⚙️ **Settings** (gear icon, top right)
3. Check ✅ **Use your own OAuth credentials**
4. Enter your **Client ID** and **Client Secret**
5. Close settings
6. In left panel, find **Gmail API v1** and select:
   - `https://www.googleapis.com/auth/gmail.modify`
   - `https://www.googleapis.com/auth/gmail.settings.basic`
7. Click **Authorize APIs**
8. Sign in and click **Allow**
9. Click **Exchange authorization code for tokens**
10. **Copy the Refresh Token** from the response

#### Step 6: Add to Claude Code

Choose your scope:

<details>
<summary><strong>🌍 User Scope (Recommended) - Available in all projects</strong></summary>

**macOS/Linux:**
```bash
claude mcp add gmail --scope user -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Client-Id: YOUR_CLIENT_ID" \
  --header "X-Gmail-Client-Secret: YOUR_CLIENT_SECRET" \
  --header "X-Gmail-Refresh-Token: YOUR_REFRESH_TOKEN"
```

**Windows (PowerShell):** *(single line)*
```powershell
claude mcp add gmail --scope user -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp --header "X-Gmail-Client-Id: YOUR_CLIENT_ID" --header "X-Gmail-Client-Secret: YOUR_CLIENT_SECRET" --header "X-Gmail-Refresh-Token: YOUR_REFRESH_TOKEN"
```

</details>

<details>
<summary><strong>📁 Project Scope - Only in current project</strong></summary>

**macOS/Linux:**
```bash
claude mcp add gmail -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Client-Id: YOUR_CLIENT_ID" \
  --header "X-Gmail-Client-Secret: YOUR_CLIENT_SECRET" \
  --header "X-Gmail-Refresh-Token: YOUR_REFRESH_TOKEN"
```

**Windows (PowerShell):** *(single line)*
```powershell
claude mcp add gmail -- npx mcp-remote https://deep-red-marten.fastmcp.app/mcp --header "X-Gmail-Client-Id: YOUR_CLIENT_ID" --header "X-Gmail-Client-Secret: YOUR_CLIENT_SECRET" --header "X-Gmail-Refresh-Token: YOUR_REFRESH_TOKEN"
```

</details>

**Done!** 🎉 Now you have access to all Gmail features including drafts, labels, and filters!

> **⚠️ Important:** If you have a `.mcp.json` file in your project with a `gmail` server configured for SMTP, it may override your OAuth config. Either:
> - Update `.mcp.json` to use OAuth headers, or
> - Delete the `gmail` entry from `.mcp.json`, or  
> - Use a different server name (e.g., `gmail-oauth`)

**Available Tools with OAuth:**
- `send_email`, `draft_email`, `read_email`, `search_emails`
- `modify_email`, `delete_email`
- `list_email_labels`, `create_gmail_label`, `delete_gmail_label`
- `create_gmail_filter`, `list_gmail_filters`, `delete_gmail_filter`

---

### 🖥️ Option 3: Local Installation

For complete control, offline access, and all 19 tools:

#### 1. Install

```bash
# Clone and setup
git clone https://github.com/Wania-Kazmi/gmail-mcp-server.git
cd gmail-mcp-server/python
uv sync
```

#### 2. Authenticate

```bash
uv run gmail-mcp auth
```

#### 3. Run

```bash
# For Claude Desktop (stdio)
uv run gmail-mcp serve

# For HTTP API
uv run gmail-mcp serve -t http -p 8000

# For SSE streaming
uv run gmail-mcp serve -t sse -p 8000
```

---

## ✨ Features

### 📧 Email Operations

| Tool | Description | Auth Required |
|------|-------------|:-------------:|
| `send_email` | Send emails with attachments, CC, BCC, HTML support | SMTP / OAuth |
| `draft_email` | Create email drafts for later sending | OAuth |
| `read_email` | Read full email content with attachments info | SMTP / OAuth |
| `search_emails` | Search using Gmail's powerful query syntax | SMTP / OAuth |
| `modify_email` | Add or remove labels from emails | OAuth |
| `delete_email` | Permanently delete emails | SMTP / OAuth |
| `batch_modify_emails` | Bulk label operations on multiple emails | OAuth |
| `batch_delete_emails` | Bulk delete multiple emails | OAuth |
| `download_attachment` | Download email attachments to local filesystem | OAuth |

### 🏷️ Label Management

| Tool | Description | Auth Required |
|------|-------------|:-------------:|
| `list_email_labels` | List all Gmail labels and folders | SMTP / OAuth |
| `create_gmail_label` | Create new custom labels | OAuth |
| `update_gmail_label` | Update label name, color, visibility | OAuth |
| `delete_gmail_label` | Delete custom labels | OAuth |
| `get_or_create_gmail_label` | Get existing or create new label | OAuth |

### 🔍 Filter Management

| Tool | Description | Auth Required |
|------|-------------|:-------------:|
| `create_gmail_filter` | Create custom email filters with actions | OAuth |
| `list_gmail_filters` | List all email filters | OAuth |
| `get_gmail_filter` | Get filter details by ID | OAuth |
| `delete_gmail_filter` | Delete email filters | OAuth |
| `create_filter_from_template` | Create filters using pre-built templates | OAuth |

---

## 📦 Installation

### Prerequisites

- **Python 3.10 or higher**
- **[uv](https://docs.astral.sh/uv/)** - Fast Python package manager (recommended)

### Install uv

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
</details>

<details>
<summary><strong>macOS / Linux</strong></summary>

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
</details>

### Installation Methods

#### From Source (Recommended for Development)

```bash
git clone https://github.com/your-username/gmail-mcp-server.git
cd gmail-mcp-server/python
uv sync
```

#### Using pip (After PyPI Publication)

```bash
pip install gmail-mcp-server
```

#### Using uvx (No Installation Required)

```bash
uvx gmail-mcp-server serve
```

#### Using uv tool

```bash
uv tool install gmail-mcp-server
gmail-mcp serve
```

---

## 🔐 Authentication

Gmail MCP Server supports two authentication methods. Choose based on your needs:

### Comparison

| Feature | SMTP (App Password) | OAuth (Full Access) |
|---------|:-------------------:|:-------------------:|
| Setup Time | 2 minutes | 10 minutes |
| Send Emails | ✅ | ✅ |
| Read Emails | ✅ | ✅ |
| Search Emails | ✅ (Basic) | ✅ (Advanced) |
| Create Drafts | ❌ | ✅ |
| Manage Labels | ❌ | ✅ |
| Manage Filters | ❌ | ✅ |
| Batch Operations | ❌ | ✅ |
| Download Attachments | ❌ | ✅ |

---

### Option 1: SMTP with App Password (Quick Setup)

**Best for**: Basic email sending and reading, quick testing.

#### Step 1: Enable 2-Step Verification

1. Go to [Google Account Security](https://myaccount.google.com/security)
2. Under "Signing in to Google", click **2-Step Verification**
3. Follow the prompts to enable it

#### Step 2: Create App Password

1. Go to [App Passwords](https://myaccount.google.com/apppasswords)
2. Select **Mail** as the app
3. Select your device type
4. Click **Generate**
5. **Copy the 16-character password** (you won't see it again!)

#### Step 3: Configure

```bash
uv run gmail-mcp auth
```

Choose option `1` and enter:
- Your Gmail address
- The 16-character App Password

```
✅ SMTP credentials saved successfully!
Account: your.email@gmail.com
```

---

### Option 2: Google Cloud OAuth (Full Access)

**Best for**: Complete Gmail API access, all features enabled.

#### Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **Select a project** → **New Project**
3. Enter project name (e.g., "Gmail MCP Server")
4. Click **Create**

#### Step 2: Enable Gmail API

1. Go to **APIs & Services** → **Library**
2. Search for **Gmail API**
3. Click **Enable**

#### Step 3: Configure OAuth Consent Screen

1. Go to **APIs & Services** → **OAuth consent screen**
2. Select **External** user type → **Create**
3. Fill in required fields:
   - App name: "Gmail MCP Server"
   - User support email: your email
   - Developer contact: your email
4. Click **Save and Continue**
5. On **Scopes** page, click **Add or Remove Scopes**
6. Add these scopes:
   - `https://www.googleapis.com/auth/gmail.modify`
   - `https://www.googleapis.com/auth/gmail.settings.basic`
7. Click **Update** → **Save and Continue**
8. On **Test users** page, click **Add Users**
9. Add your Gmail address
10. Click **Save and Continue**
11. Go back to **OAuth consent screen** → Click **Publish App** to move from "Testing" to "In Production"
    - This allows your app to request full permissions needed for all Gmail MCP tools

#### Step 4: Create OAuth Credentials

1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth client ID**
3. Application type: **Desktop app**
4. Name: "Gmail MCP Server"
5. Click **Create**
6. Click **Download JSON** and save the file

#### Step 5: Configure

```bash
uv run gmail-mcp auth
```

Choose option `2` and provide the path to your JSON file:

```
Enter path to OAuth credentials JSON file: /path/to/credentials.json

Opening browser for authorization...
✅ OAuth credentials saved successfully!
Account: your.email@gmail.com
```

---

## 🖥️ Running the Server

### Transport Modes

| Mode | Use Case | Command |
|------|----------|---------|
| **stdio** | Claude Desktop, MCP clients | `gmail-mcp serve` |
| **HTTP** | REST API, web integrations | `gmail-mcp serve -t http` |
| **SSE** | Real-time streaming | `gmail-mcp serve -t sse` |

### Command Options

```bash
gmail-mcp serve [OPTIONS]
```

| Option | Short | Description | Default |
|--------|-------|-------------|---------|
| `--transport` | `-t` | Transport: stdio, http, sse | stdio |
| `--host` | `-h` | Host to bind (http/sse only) | 0.0.0.0 |
| `--port` | `-p` | Port to bind (http/sse only) | 8000 |

### Examples

```bash
# Claude Desktop integration (default)
uv run gmail-mcp serve

# HTTP API on port 8000
uv run gmail-mcp serve -t http

# HTTP on custom port
uv run gmail-mcp serve -t http -p 3000

# SSE streaming
uv run gmail-mcp serve -t sse -p 8080

# Bind to localhost only
uv run gmail-mcp serve -t http -h 127.0.0.1 -p 8000
```

---

## 🔌 Client Configuration

### Claude Desktop

Add to your Claude Desktop configuration file:

**Location:**
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`

#### Using uvx (Recommended)

```json
{
  "mcpServers": {
    "gmail": {
      "command": "uvx",
      "args": ["gmail-mcp-server", "serve"]
    }
  }
}
```

#### Using Local Installation

```json
{
  "mcpServers": {
    "gmail": {
      "command": "uv",
      "args": [
        "run",
        "--directory", "C:/path/to/gmail-mcp-server/python",
        "gmail-mcp",
        "serve"
      ]
    }
  }
}
```

#### Using pip Installation

```json
{
  "mcpServers": {
    "gmail": {
      "command": "gmail-mcp",
      "args": ["serve"]
    }
  }
}
```

### Claude Code (CLI)

Add the Gmail MCP server using the Claude CLI:

```bash
# Add to current project
claude mcp add gmail --transport http \
  https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Email: your-email@gmail.com" \
  --header "X-Gmail-Password: your-app-password"

# Add globally (available in all projects)
claude mcp add gmail --transport http --scope user \
  https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Email: your-email@gmail.com" \
  --header "X-Gmail-Password: your-app-password"
```

### FastMCP Cloud (Hosted)

Use the hosted version — no installation required!

**Endpoint:** `https://deep-red-marten.fastmcp.app/mcp`

The cloud version supports **two authentication methods**:

#### Option A: SMTP Authentication (Quick Setup)

Best for basic email operations. Use App Password headers:

| Header | Value |
|--------|-------|
| `X-Gmail-Email` | Your Gmail address |
| `X-Gmail-Password` | Your 16-character App Password |

**Example with curl:**

```bash
curl -X POST "https://deep-red-marten.fastmcp.app/mcp" \
  -H "X-Gmail-Email: your-email@gmail.com" \
  -H "X-Gmail-Password: your-app-password" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

#### Option B: OAuth Authentication (Full API Access)

Best for complete Gmail API access including drafts, labels, and filters.

**Step 1: Get OAuth Credentials from Google Cloud**

Follow the [OAuth Setup](#option-2-google-cloud-oauth-full-access) steps above to:
1. Create a Google Cloud Project
2. Enable Gmail API
3. Configure OAuth Consent Screen
4. Create OAuth Credentials (**Web application** type for Playground method, or **Desktop app** for local method)
5. Download the JSON credentials file

**Step 2: Get Your Refresh Token**

Choose ONE of these methods:

<details>
<summary><strong>🌐 Method A: Google OAuth Playground (No Installation)</strong></summary>

Use Google's official OAuth Playground — zero installation required!

1. Go to [Google OAuth Playground](https://developers.google.com/oauthplayground/)

2. Click the ⚙️ **gear icon** (top right) and check:
   - ✅ **Use your own OAuth credentials**
   - Enter your **Client ID** and **Client Secret** from the JSON file

3. In the left panel, find **Gmail API v1** and select:
   - `https://www.googleapis.com/auth/gmail.modify`
   - `https://www.googleapis.com/auth/gmail.settings.basic`

4. Click **Authorize APIs** → Sign in with your Google account → **Allow**

5. Click **Exchange authorization code for tokens**

6. Copy the **Refresh token** from the response

> **Note:** For OAuth Playground, your OAuth credentials must be **Web application** type with `https://developers.google.com/oauthplayground` added as an authorized redirect URI.

</details>

<details>
<summary><strong>💻 Method B: Local Authentication (Requires Installation)</strong></summary>

Run the local authentication to generate a refresh token:

```bash
cd python
uv run gmail-mcp auth
```

Choose option `2` (OAuth) and complete the browser authorization. This creates `~/.gmail-mcp/credentials.json` containing your refresh token.

</details>

**Step 3: Extract Your OAuth Values**

From your downloaded JSON file (e.g., `client_secret_xxxxx.apps.googleusercontent.com.json`):
```json
{
  "installed": {
    "client_id": "xxxxx.apps.googleusercontent.com",
    "client_secret": "GOCSPX-xxxxxxxx"
  }
}
```

From OAuth Playground or `~/.gmail-mcp/credentials.json`:
```
refresh_token: 1//09xxxxxxxx
```

**Step 4: Use OAuth Headers**

| Header | Value |
|--------|-------|
| `X-Gmail-Client-Id` | Your OAuth client ID |
| `X-Gmail-Client-Secret` | Your OAuth client secret |
| `X-Gmail-Refresh-Token` | Your refresh token |

**Example with curl (OAuth):**

```bash
curl -X POST "https://deep-red-marten.fastmcp.app/mcp" \
  -H "X-Gmail-Client-Id: 123456789-xxxxx.apps.googleusercontent.com" \
  -H "X-Gmail-Client-Secret: GOCSPX-xxxxxxxxxx" \
  -H "X-Gmail-Refresh-Token: 1//09xxxxxxxxxxxxxxxx" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"list_email_labels","arguments":{}}}'
```

**Example with PowerShell (OAuth):**

```powershell
$headers = @{
    "Content-Type" = "application/json"
    "Accept" = "application/json, text/event-stream"
    "X-Gmail-Client-Id" = "123456789-xxxxx.apps.googleusercontent.com"
    "X-Gmail-Client-Secret" = "GOCSPX-xxxxxxxxxx"
    "X-Gmail-Refresh-Token" = "1//09xxxxxxxxxxxxxxxx"
}

$body = @{
    jsonrpc = "2.0"
    id = 1
    method = "tools/call"
    params = @{
        name = "list_email_labels"
        arguments = @{}
    }
} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Uri "https://deep-red-marten.fastmcp.app/mcp" -Method Post -Headers $headers -Body $body
```

**Claude Code CLI with OAuth:**

```bash
# Add with OAuth headers (project scope)
claude mcp add gmail --transport http \
  https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Client-Id: YOUR_CLIENT_ID" \
  --header "X-Gmail-Client-Secret: YOUR_CLIENT_SECRET" \
  --header "X-Gmail-Refresh-Token: YOUR_REFRESH_TOKEN"

# Add globally with OAuth
claude mcp add gmail --transport http --scope user \
  https://deep-red-marten.fastmcp.app/mcp \
  --header "X-Gmail-Client-Id: YOUR_CLIENT_ID" \
  --header "X-Gmail-Client-Secret: YOUR_CLIENT_SECRET" \
  --header "X-Gmail-Refresh-Token: YOUR_REFRESH_TOKEN"
```

#### Authentication Comparison (Cloud)

| Feature | SMTP Headers | OAuth Headers |
|---------|:------------:|:-------------:|
| Setup Complexity | ⭐ Easy | ⭐⭐⭐ Medium |
| Send Emails | ✅ | ✅ |
| Read Emails | ✅ | ✅ |
| Search Emails | ✅ | ✅ |
| List Labels | ✅ | ✅ |
| Delete Emails | ✅ | ✅ |
| Create Drafts | ❌ | ✅ |
| Modify Labels | ❌ | ✅ |
| Create Labels | ❌ | ✅ |
| Create Filters | ❌ | ✅ |
| Manage Filters | ❌ | ✅ |

### HTTP Client

Start the server:
```bash
uv run gmail-mcp serve -t http -p 8000
```

Connect to: `http://localhost:8000/mcp`

### SSE Client

Start the server:
```bash
uv run gmail-mcp serve -t sse -p 8000
```

Connect to: `http://localhost:8000/sse`

---

## 📚 API Reference

### send_email

Send an email with optional attachments, CC, BCC, and HTML content.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `to` | string[] | ✅ | Recipient email addresses |
| `subject` | string | ✅ | Email subject line |
| `body` | string | ✅ | Plain text body |
| `html_body` | string | ❌ | HTML body content |
| `mime_type` | string | ❌ | Content type: `text/plain`, `text/html`, `multipart/alternative` |
| `cc` | string[] | ❌ | CC recipients |
| `bcc` | string[] | ❌ | BCC recipients |
| `thread_id` | string | ❌ | Thread ID for replies |
| `attachments` | string[] | ❌ | File paths to attach |

**Example:**

```
Send an email to john@example.com with subject "Meeting Tomorrow" 
and body "Hi John, let's meet at 3pm. Thanks!"
```

---

### search_emails

Search emails using Gmail's query syntax.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `query` | string | ✅ | Gmail search query |
| `max_results` | number | ❌ | Maximum results (default: 10) |

**Gmail Search Operators:**

| Operator | Example | Description |
|----------|---------|-------------|
| `from:` | `from:boss@company.com` | From specific sender |
| `to:` | `to:me@example.com` | To specific recipient |
| `subject:` | `subject:meeting` | Subject contains word |
| `is:unread` | `is:unread` | Unread emails only |
| `is:starred` | `is:starred` | Starred emails |
| `has:attachment` | `has:attachment` | Emails with attachments |
| `after:` | `after:2024/01/01` | After specific date |
| `before:` | `before:2024/12/31` | Before specific date |
| `larger:` | `larger:5M` | Larger than size |
| `smaller:` | `smaller:1M` | Smaller than size |
| `label:` | `label:work` | Has specific label |
| `in:` | `in:inbox` | In specific folder |
| `filename:` | `filename:pdf` | Attachment filename |

**Examples:**

```
Search for unread emails from john@example.com
→ query: "from:john@example.com is:unread"

Find emails with PDF attachments from last week
→ query: "has:attachment filename:pdf after:2024/12/24"

Search for emails about "project update" in inbox
→ query: "in:inbox subject:project update"
```

---

### read_email

Read the full content of an email by ID.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `message_id` | string | ✅ | Email message ID |

**Returns:**
- Subject, From, To, Date
- Body (text and/or HTML)
- Attachments list with download IDs

---

### create_gmail_label

Create a new Gmail label.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `name` | string | ✅ | Label name |
| `label_list_visibility` | string | ❌ | `labelShow`, `labelShowIfUnread`, `labelHide` |
| `message_list_visibility` | string | ❌ | `show`, `hide` |
| `background_color` | string | ❌ | Hex color (e.g., `#4285f4`) |
| `text_color` | string | ❌ | Hex color for text |

---

### create_gmail_filter

Create an email filter with automatic actions.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `from_address` | string | ❌ | Filter by sender |
| `to_address` | string | ❌ | Filter by recipient |
| `subject` | string | ❌ | Filter by subject |
| `query` | string | ❌ | Custom Gmail query |
| `add_label_ids` | string[] | ❌ | Labels to add |
| `remove_label_ids` | string[] | ❌ | Labels to remove |
| `mark_read` | boolean | ❌ | Mark as read |
| `star` | boolean | ❌ | Add star |
| `archive` | boolean | ❌ | Skip inbox (archive) |
| `forward` | string | ❌ | Forward to email |
| `trash` | boolean | ❌ | Move to trash |
| `never_spam` | boolean | ❌ | Never mark as spam |
| `important` | boolean | ❌ | Mark as important |

---

### Filter Templates

Create filters quickly with pre-built templates:

| Template | Parameter | Description |
|----------|-----------|-------------|
| `fromSender` | `sender_email` | Filter emails from specific sender |
| `withSubject` | `subject_text` | Filter by subject keywords |
| `withAttachments` | — | Filter emails with attachments |
| `largeEmails` | `size_in_bytes` | Filter large emails |
| `containingText` | `search_text` | Filter by body content |
| `mailingList` | `list_identifier` | Filter mailing list emails |

---

## 🛠️ CLI Commands

```bash
gmail-mcp --help
```

| Command | Description |
|---------|-------------|
| `serve` | Start the MCP server |
| `auth` | Set up Gmail authentication |
| `status` | Check authentication status |
| `test` | Test Gmail connectivity |
| `version` | Show version information |

---

## 📁 Project Structure

```
gmail-mcp-server/
├── python/
│   ├── gmail_mcp/              # Main package
│   │   ├── __init__.py         # Package metadata
│   │   ├── cli.py              # Typer CLI application
│   │   ├── server.py           # FastMCP server (19 tools)
│   │   ├── auth.py             # SMTP & OAuth authentication
│   │   ├── smtp_handler.py     # SMTP/IMAP email handler
│   │   ├── utils.py            # Email utilities
│   │   ├── label_manager.py    # Label management functions
│   │   └── filter_manager.py   # Filter management functions
│   ├── pyproject.toml          # Package configuration
│   ├── uv.lock                 # Locked dependencies
│   └── README.md               # Detailed documentation
├── LICENSE                     # MIT License
└── README.md                   # This file
```

---

## 🐳 Docker Deployment

### Dockerfile

```dockerfile
FROM python:3.11-slim

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app

# Copy project files
COPY python/pyproject.toml python/uv.lock ./
COPY python/gmail_mcp ./gmail_mcp

# Install dependencies
RUN uv sync --frozen --no-dev

# Expose port for HTTP/SSE modes
EXPOSE 8000

# Default command (HTTP mode)
CMD ["uv", "run", "gmail-mcp", "serve", "-t", "http"]
```

### Build and Run

```bash
# Build
docker build -t gmail-mcp-server .

# Run with credentials mounted
docker run -p 8000:8000 \
  -v ~/.gmail-mcp:/root/.gmail-mcp \
  gmail-mcp-server
```

### Docker Compose

```yaml
version: '3.8'
services:
  gmail-mcp:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - ~/.gmail-mcp:/root/.gmail-mcp
    environment:
      - GMAIL_OAUTH_PATH=/root/.gmail-mcp/gcp-oauth.keys.json
    restart: unless-stopped
```

---

## 🔧 Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `GMAIL_OAUTH_PATH` | Path to OAuth keys file | `~/.gmail-mcp/gcp-oauth.keys.json` |
| `GMAIL_CREDENTIALS_PATH` | Path to saved credentials | `~/.gmail-mcp/credentials.json` |
| `GMAIL_SMTP_PATH` | Path to SMTP credentials | `~/.gmail-mcp/smtp-credentials.json` |

---

## 🔍 Troubleshooting

### Authentication Issues

| Problem | Solution |
|---------|----------|
| "OAuth keys file not found" | Run `gmail-mcp auth` and complete setup |
| "Credentials expired" | Run `gmail-mcp auth` to refresh |
| "Access blocked: This app's request is invalid" | Verify OAuth consent screen is configured correctly |
| "Access blocked: App not verified" | Add your email as a test user in Cloud Console |
| Browser doesn't open | Manually copy the URL printed in terminal |

### Connection Issues

| Problem | Solution |
|---------|----------|
| "Connection refused" | Ensure server is running on correct port |
| "SMTP authentication failed" | Verify App Password is correct (16 characters, no spaces) |
| "Invalid grant" | Delete `~/.gmail-mcp/credentials.json` and re-authenticate |

### Runtime Issues

| Problem | Solution |
|---------|----------|
| "Module not found" | Run `uv sync` to install dependencies |
| Port already in use | Use `-p` flag to specify different port |
| Rate limit exceeded | Reduce request frequency; Gmail API has usage limits |

### Check Status

```bash
# Check authentication status
uv run gmail-mcp status

# Test connectivity
uv run gmail-mcp test
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [FastMCP](https://gofastmcp.com) - The excellent MCP framework
- [uv](https://docs.astral.sh/uv/) - Fast Python package manager
- [Typer](https://typer.tiangolo.com/) - CLI framework
- [Google Gmail API](https://developers.google.com/gmail/api)

---

<p align="center">
  <strong>Built with ❤️ for the AI community</strong>
  <br>
  <a href="https://gofastmcp.com">FastMCP</a> •
  <a href="https://docs.astral.sh/uv/">uv</a> •
  <a href="https://modelcontextprotocol.io/">MCP</a>
</p>

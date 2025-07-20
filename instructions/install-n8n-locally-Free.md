
# ✅ Setting Up n8n Locally — Complete Guide for AI Agent Builders

This guide walks you through a **tested and stable setup** of `n8n` for building and demoing AI Agents using WhatsApp + OpenAI — without billing issues or cloud limitations.

Supports both **macOS** and **Windows** users.

---

## 🖥️ For macOS Users — Install Node.js v18 via Homebrew (No `nvm` Required)

### 1️⃣ Install Homebrew (if not installed)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew update
````

---

### 2️⃣ Install Node.js v18 (LTS)

```bash
brew install node@18
```

---

### 3️⃣ Make Node.js v18 the Default

```bash
brew link --force --overwrite node@18
```

---

### 4️⃣ Verify Installation

```bash
node -v    # ✅ Should return v18.20.x
npm -v     # ✅ Should return >= 8.x
```

---

## 🪟 For Windows Users — Install Node.js v18 with Official Installer

### 1️⃣ Go to [https://nodejs.org/en/download](https://nodejs.org/en/download)

Download the **LTS version** (v18.x.x) for Windows.

### 2️⃣ Install Node.js

During installation:

* ✅ Ensure **npm** is included
* ✅ Add to **PATH** is selected

### 3️⃣ Verify Installation (in Command Prompt or PowerShell)

```bash
node -v    # ✅ Should return v18.20.x
npm -v     # ✅ Should return >= 8.x
```

---

## 🔧 Install and Run `n8n` Locally (Same for macOS & Windows)

### 1️⃣ Install n8n globally

```bash
npm install -g n8n
```

### 2️⃣ Run n8n

```bash
n8n
```

➡️ Visit: `http://localhost:5678`

---

## 🧠 Important Learnings

### ⚙️ Node Compatibility

* ❌ Do NOT use Node v20 or v21 — may break `n8n` functions.
* ✅ Always use Node v18 (LTS) for full stability.

---

### 🌐 Browser Issues with Safari (macOS Only)

If you see this error:

> “Your n8n server is configured to use a secure cookie…”

✅ Fix options:

* Use **Chrome or Firefox** instead
* OR disable secure cookie enforcement:

```bash
export N8N_SECURE_COOKIE=false
n8n
```

Windows users can use `set` in Command Prompt:

```cmd
set N8N_SECURE_COOKIE=false
n8n
```

---

### 🔐 Add Optional Login for Local Security

macOS:

```bash
export N8N_BASIC_AUTH_ACTIVE=true
export N8N_BASIC_AUTH_USER=admin
export N8N_BASIC_AUTH_PASSWORD=botcampus123
n8n
```

Windows (Command Prompt):

```cmd
set N8N_BASIC_AUTH_ACTIVE=true
set N8N_BASIC_AUTH_USER=admin
set N8N_BASIC_AUTH_PASSWORD=botcampus123
n8n
```

---

### 🔓 Activation Key Issue (Ignore)

If you try to activate n8n Community Edition and see:

> ❌ `value.toWellFormed is not a function`

It’s a known bug.
✅ Just close the popup and continue — activation is optional.

---

## 🧪 Workshop Setup Summary

| Tool     | Required Version | Notes                              |
| -------- | ---------------- | ---------------------------------- |
| Node.js  | v18.20.8         | LTS, avoid Node 20+                |
| n8n      | latest           | Installed via `npm install -g n8n` |
| Browser  | Chrome/Firefox   | Avoid Safari on localhost          |
| OpenAI   | API key ready    | GPT-3.5 or GPT-4 supported         |
| WhatsApp | Meta Cloud API   | Use verified WABA + access token   |

---

## 🧰 Bonus Tips

* Use Postman to simulate incoming messages to n8n webhook
* Export `.json` flows for sharing with participants
* Make sure OpenAI and WhatsApp nodes are credentialed properly

---

👨‍🏫 Curated by:
**Abdullah Khan** --
Founder & CEO – BotCampus AI

🔗 [www.botcampus.ai](https://www.botcampus.ai)

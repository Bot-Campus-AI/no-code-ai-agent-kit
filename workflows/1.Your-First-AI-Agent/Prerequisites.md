
---

## ✅ Prerequisites – Keys & Credentials for Your First AI Agent

---

### 1. 🧠 **OpenAI API Key (ChatGPT Integration)**

🔑 **Required**

This powers the AI agent (your ChatGPT model node).

#### 🔧 Steps:

1. Go to [OpenAI API Keys](https://platform.openai.com/account/api-keys)
2. Click **“Create new secret key”**
3. In n8n:

   * Go to **Credentials**
   * Add **“OpenAI API”**
   * Paste the key, save it
4. Attach it to your **OpenAI Chat Model** node

> 📌 Use a model like `gpt-4o` or `gpt-4-0613` for tool/function calling

---

### 2. 📧 **Google API (Gmail OAuth2 Integration)**

🔑 **Required** (for sending emails via Gmail)

This powers the **“Send a message in Gmail”** node.

#### 🔧 Steps:

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a **new project**
3. Enable the **Gmail API**
4. Go to **OAuth consent screen**

   * Choose **External**
   * Fill app name, scopes, email
5. Go to **Credentials > Create credentials > OAuth 2.0 Client ID**

   * Application type: **Web application**
   * Add redirect URI:

     ```
     https://api.n8n.io/oauth2-credential/callback
     ```
6. In **n8n**:

   * Create new **Google OAuth2** credentials
   * Enter:

     * Client ID
     * Client Secret
   * Authenticate & approve Gmail access

---

### 3. 🌤️ **Weather Tool – Open-Meteo**

✅ **No API Key Required**

Open-Meteo lets you fetch weather data **without any authentication**.

#### Example URL:

```
https://api.open-meteo.com/v1/forecast?latitude=25.276987&longitude=55.296249&current_weather=true
```

* You must provide **latitude and longitude**
* You can use a lookup service like Nominatim (OpenStreetMap) if the user types a city name (optional enhancement)

> 📍 Dubai:
> `latitude=25.276987`
> `longitude=55.296249`

---

### 4. 🧠 **Simple Memory Node**

✅ Optional — No credentials needed

* Helps the AI agent remember prior messages
* Set context window (e.g., 3 or 5)

---

## 🧪 Final Setup Checklist

| Component                     | Required    | Credentials Needed | Notes                       |
| ----------------------------- | ----------- | ------------------ | --------------------------- |
| OpenAI Chat Model             | ✅ Yes       | ✅ OpenAI API Key   | Use GPT-4o for tool calling |
| Gmail Integration (Send Mail) | ✅ Yes       | ✅ Google OAuth2    | Set up in Google Console    |
| Open-Meteo Weather API        | ✅ Yes       | ❌ No Key Needed    | Just needs lat/lon          |
| Simple Memory Node            | 🔄 Optional | ❌ None             | For short-term memory       |
| Chat Trigger Node             | ✅ Yes       | ❌ None             | Starts the conversation     |

---

Here you go — a complete step-by-step guide to **build your first AI Agent in n8n** using:

* ✅ OpenAI (GPT-based agent)
* ✅ Google Gmail (to send emails)
* ✅ Open-Meteo API (to fetch weather — no API key needed)

---

## 🛠️ Step-by-Step Guide: Build Your First AI Agent in n8n

---

### 🔹 **STEP 1: Start a New Workflow**

1. Open [n8n](https://app.n8n.io/) or your local instance
2. Create a **new workflow**
3. Drag in a **“Chat Interface”** node → name it `User Chat Input`

---

### 🔹 **STEP 2: Add the OpenAI Chat Model Node**

1. Add a node: `OpenAI Chat Model`
2. In the node:

   * Select your **OpenAI credentials** (added earlier)
   * Set **Model** to `gpt-4o` or `gpt-4-0613`
   * Set max tokens, temperature, etc.
3. Optional: Enable **Tool Calling** or **Function Calling** if you’re using tools

---

### 🔹 **STEP 3: Add a Simple Memory Node (Optional)**

1. Add `Simple Memory` node
2. Connect it between the **Chat Trigger** and the **AI Agent**
3. Set `Context Window Length` to `3` or `5`

This allows the AI agent to remember a few past messages.

---

### 🔹 **STEP 4: Add the AI Agent Node**

1. Drag in an `AI Agent` node
2. Connect:

   * **Input** from Chat Trigger (or Memory if used)
   * **Chat Model** to the OpenAI node
   * **Memory** to Simple Memory (if used)
   * **Tools**: Add weather and Gmail tool nodes

---

### 🔹 **STEP 5: Add Weather Tool (Open-Meteo API)**

1. Add `HTTP Request` node

   * Method: `GET`
   * URL (example for Dubai):

     ```
     https://api.open-meteo.com/v1/forecast?latitude=25.276987&longitude=55.296249&current_weather=true
     ```
2. Rename node to `get_weather`
3. In AI Agent node, assign this as a tool:

   * Tool name: `get_weather`
   * Description: "Get the current weather using Open-Meteo"

💡 To make city input dynamic, you can:

* Convert city to lat/lon using OpenStreetMap Nominatim API (optional)

---

### 🔹 **STEP 6: Add Gmail Integration**

1. Add `Send a message in Gmail` node
2. Authenticate with your **Google OAuth2 credential**
3. Configure:

   * `To`: an email address (or make dynamic via input)
   * `Subject`: `"Weather Report"` or similar
   * `Body`: use AI Agent’s output or a fixed message
4. Add this node as a **tool** in AI Agent:

   * Tool name: `send_gmail`
   * Description: "Send an email with the weather report"

---

### 🔹 **STEP 7: Connect Everything**

* Final flow:

```
         [Chat Interface]
                ↓
            [AI Agent]
          ↙        ↘
[Simple Memory] [OpenAI Chat Model]
                  ↙             ↘
            [get_weather]   [send_gmail]

```

---

### 🔹 **STEP 8: Test Your Agent**

1. Click **Execute Workflow**
2. Open **Chat Interface**
3. Try queries like:

   * “What’s the weather in Dubai?”
   * “Email me the weather update.”

Watch the logs and output live.

---

## ✅ You’re Done!

You now have a **fully working AI agent** in n8n that:

* Understands chat via OpenAI
* Calls external tools (weather & email)
* Optionally remembers context

---

📌 **Copyright © BotCampus AI. All rights reserved.**

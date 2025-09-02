
---

## **Slack → Google Sheets → Slack: n8n Workflow Guide**

### **1. Prerequisites**

Before starting, ensure you have:

* **n8n installed locally** (via npm, brew, or Docker)
* **Google account** with Google Sheets access
* **Slack workspace** where you can install a custom app
* **ngrok account** (for exposing your local n8n to the internet)
* **Postman** (optional, for testing)
* Basic familiarity with Slack slash commands and webhooks

---

### **2. High-Level Flow**

The workflow does the following:

1. **Trigger**: A Slack slash command `/idea` sends data (idea name + creator name) to an **n8n webhook**.
2. **Process**: n8n stores that idea into a **Google Sheet**.
3. **Notify**: n8n sends a confirmation message back into Slack.

---

### **3. Step-by-Step Setup**

#### **A. Create Google Sheet**

* Create a sheet with **exact** headers:

  ```
  Name | Creator
  ```
* Copy the **sheet URL**.

---

#### **B. Setup Slack App**

1. Go to [Slack API Apps](https://api.slack.com/apps) → **Create New App**.
2. Add:

   * **Bot Token Scopes** → `chat:write`
   * **Slash Command** `/idea`
3. In the slash command’s **Request URL**, paste your **ngrok public URL** + **n8n webhook path** (you’ll get this later).
4. Install the app to your workspace and note down:

   * **Bot User OAuth Token** (starts with `xoxb-…`)
   * **Channel ID** (from channel info page)

---

#### **C. Run n8n Locally**

```bash
n8n start
```

It will run at `http://localhost:5678`.

---

#### **D. Start ngrok**

```bash
ngrok http 5678
```

Copy the `https://…ngrok-free.app` forwarding URL.

---

#### **E. Configure n8n Workflow**

1. **Webhook Node**

   * Method: POST
   * Path: `slack-trigger`
   * This will give you:

     ```
     http://localhost:5678/webhook-test/slack-trigger
     ```
   * Replace `http://localhost:5678` with your ngrok `https://…` URL in Slack slash command config.

2. **Google Sheets Node**

   * Operation: Append or update
   * Map:

     ```
     Name → {{$json["text"]}}
     Creator → {{$json["user_name"]}}
     ```

3. **HTTP Request Node (Slack Message)**

   * Method: POST
   * URL: `https://slack.com/api/chat.postMessage`
   * Auth: Predefined Slack API credentials
   * Body:

     ```
     channel: YOUR_CHANNEL_ID
     text: New idea received ✅ Name: {{...}} Creator: {{...}} Sheet: SHEET_URL
     ```

---

### **4. Common Mistakes & Fixes**

| **Mistake**                   | **Cause**                        | **Fix**                                          |
| ----------------------------- | -------------------------------- | ------------------------------------------------ |
| `dispatch_failed` in Slack    | Slack can’t reach your local n8n | Use **ngrok** public URL in slash command config |
| Missing `channel` field error | Slack API requires a channel ID  | Use actual channel ID (`CXXXXXX`), not name      |
| Wrong URL in webhook node     | Still pointing to `localhost`    | Replace with ngrok `https://…` URL               |
| Google Sheets not updating    | Wrong mapping                    | Match **exact** header names                     |
| "not\_in\_channel" error      | Bot not in channel               | Invite bot to channel with `/invite @botname`    |

---

### **5. Testing**

* Type in Slack:

  ```
  /idea AI Agent for lead scoring
  ```
* You should see:

  * Message in channel with idea details
  * Row added in Google Sheets

---

### **6. Real-World Use Cases**

* **Idea collection** from Slack directly into a database
* **Employee suggestions box** automated into Sheets
* **Bug reporting** from Slack into a project tracker
* **Quick polls or feedback collection**
* **Sales team lead logging** directly from Slack

---

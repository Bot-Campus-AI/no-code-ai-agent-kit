
---

## 🤖 WhatsApp GPT Agent: Auto-Reply, Log to DB & File

This agent listens to a WhatsApp webhook, generates a GPT-based reply, logs it to MySQL and a flat file, and sends a response back via WhatsApp using a template.

---

### ✅ Prerequisites

1. **n8n (Self-hosted)**
2. **MySQL running locally** (ensure credentials + schema ready)
3. **WhatsApp Cloud API**

   * Business Account
   * Verified number & `PHONE_NUMBER_ID`
   * Approved message template (e.g., `visaad`)
4. **OpenAI API Key** (for GPT-4 or GPT-3.5)
5. **n8n credentials setup** for:

   * OpenAI
   * MySQL
   * HTTP Webhook (GET/POST enabled)
   * Write access to local file system

---

### 🛠️ Workflow Overview

```
[Webhook]
   ↓
[Edit Fields] → clean/extract fields
   ↓
[OpenAI Message Model] → get GPT response
   ↓
[Prepare Response] → prepare WhatsApp-ready text
   ↓
├── [HTTP Request to WhatsApp API] → sends WhatsApp message using template
├── [MySQL] → stores the message log in DB
├── [Convert to File → Write to Disk] → saves conversation locally
└── [Respond to Webhook] → closes the loop
```

---

### 🪜 Step-by-Step Setup

---

#### 1. **Webhook (Start node)**

* Method: `POST`
* URL: `/whatsapp-inbound`
* Expect: JSON body like:

  ```json
  {
    "userPhone": "+9712345678",
    "userMessage": "Hello Bot!"
  }
  ```

---

#### 2. **Edit Fields (Manual)**

* Rename incoming fields for clarity:

  * `userMessage → userMessage`
  * `userPhone → userPhone`
* (Optional) Remove noise fields if any.

---

#### 3. **OpenAI Message Model**

* **Model:** `gpt-3.5-turbo` or `gpt-4`
* **Prompt Setup:**

  * Role: `system`: “You are a helpful assistant.”
  * Role: `user`: Use `{{$json.userMessage}}`

---

#### 4. **Prepare Response**

* Add field: `reply → {{$json.message.content}}`

---

#### 5. **Send WhatsApp Message (HTTP Request)**

* Method: `POST`

* URL:

  ```
  https://graph.facebook.com/v19.0/{PHONE_NUMBER_ID}/messages
  ```

* **Headers:**

  * `Authorization`: `Bearer <your-access-token>`
  * `Content-Type`: `application/json`

* **Body (JSON):**

  ```json
  {
    "messaging_product": "whatsapp",
    "to": "{{$json.userPhone.replace('+','')}}",
    "type": "template",
    "template": {
      "name": "visaad",
      "language": {
        "code": "en"
      },
      "components": [
        {
          "type": "body",
          "parameters": [
            {
              "type": "text",
              "text": "{{$json.reply}}"
            }
          ]
        }
      ]
    }
  }
  ```

> ⚠️ Use only **pre-approved templates** if the conversation is **outside the 24-hour user-initiated window**.

---

#### 6. **MySQL (Insert Query)**

* Query:

  ```sql
  INSERT INTO whatsapp_logs (user_phone, user_message, bot_reply)
  VALUES ({{$json.userPhone}}, {{$json.userMessage}}, {{$json.reply}});
  ```

---

#### 7. **Convert to File → Write to Disk**

* Format a text output:

  ```json
  {
    "text": "User: {{$json.userMessage}}\nBot: {{$json.reply}}\nPhone: {{$json.userPhone}}\n---\n"
  }
  ```

* Save as `.txt` file in:

  ```
  /files/messages-log.txt
  ```

---

#### 8. **Respond to Webhook**

* Return success response like:

  ```json
  {
    "status": "sent",
    "to": "{{$json.userPhone}}"
  }
  ```

---

### ✅ Tips

* To test locally, use tools like `curl` or Postman to send the webhook payload.
* To send **normal text messages**, the user must message within 24 hours.
* If you're outside the window, use **template messages only**.
* Always validate your WhatsApp template format. E.g., if it expects `IMAGE`, text won’t work.

---

### 📁 Save This Agent

Export the workflow as JSON and save to:

```
ai-agent-workshop/ai-agent-workshop/instructions/workflows/whatsapp-gpt-agent.json
```


---

📌 **Copyright © BotCampus AI. All rights reserved.**


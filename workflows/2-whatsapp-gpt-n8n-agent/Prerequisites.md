
---

### 💰 What Is Free vs Paid

| Component              | Type     | Notes                                                           |
| ---------------------- | -------- | --------------------------------------------------------------- |
| **n8n (Self-hosted)**  | ✅ Free   | Installed locally, no cost                                      |
| **OpenAI API Key**     | 🔒 Paid  | Charged per usage (GPT-3.5 is cheaper than GPT-4)               |
| **WhatsApp Cloud API** | ✅ Free\* | Meta allows free tier for limited messages per month, then paid |
| **MySQL**              | ✅ Free   | Open-source; install locally                                    |
| **Writing to File**    | ✅ Free   | Uses your local disk, no cost                                   |

> ⚠️ \*WhatsApp gives 1,000 free conversations/month per phone number. After that, charges apply.

---

### 🛠️ What Needs to Be Installed

| Tool/Service                | Installation Needed?    | Purpose                               |
| --------------------------- | ----------------------- | ------------------------------------- |
| **n8n**                     | ✅ Yes (Local)           | Core automation tool to run workflows |
| **MySQL**                   | ✅ Yes (Local)           | To log data (messages, phone numbers) |
| **Text Editor or IDE**      | ✅ Yes                   | To edit the project and upload flows  |
| **OpenAI API Key**          | 🔑 Needed (Not install) | To call GPT model responses           |
| **Meta WhatsApp Cloud API** | 🔑 Needed (Cloud setup) | For sending WhatsApp messages         |

---

### 🧠 What Each Component Does

| Component              | Role                                                |
| ---------------------- | --------------------------------------------------- |
| **n8n**                | Hosts and connects all nodes to automate the flow   |
| **Webhook Node**       | Receives the message (trigger)                      |
| **OpenAI Node**        | Sends input to GPT and receives a reply             |
| **Prepare Response**   | Formats the response message                        |
| **HTTP Request Node**  | Sends a message to WhatsApp via template            |
| **MySQL Node**         | Stores message history for records                  |
| **Write to Disk Node** | Saves messages to a `.txt` file for backup or audit |

---
## 🔄 Flow Summary

1. Webhook receives WhatsApp message
2. Edit Fields node captures relevant data (message, phone)
3. GPT node generates a response
4. Prepare Response formats the message
5. Message is sent via WhatsApp using a **pre-approved template**
6. Message + phone are logged in MySQL
7. Backup is written to a `.txt` file

---

## 📝 Important Notes

- You **must use a pre-approved template** if the user's last message is **older than 24 hours**.
- The WhatsApp `template` must **match the parameter types** (e.g., text vs image).
- This flow **does not use `ngrok`**, as we are running locally with public API keys.

---
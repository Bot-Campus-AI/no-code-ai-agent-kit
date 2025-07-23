# 🛠️ How to Build the Resume Screener Workflow in n8n

This guide walks you through building the Resume Screener AI Workflow step-by-step in **n8n**, explaining each node and its role in the process.

---
“On import, reconfigure the OpenAI and Google Sheets credentials.”

## 🎯 Goal

To create an automated pipeline that:

1. Accepts one or more resumes via HTTP.
2. Extracts text from each file.
3. Sends the content to OpenAI for evaluation.
4. Parses the result.
5. Appends the evaluation to a Google Sheet.

---

## 🧩 Required Nodes & What They Do

### 1. **Webhook Node**

**Purpose**: Acts as the entry point. Allows file uploads from external apps like Postman or web forms.

* Method: POST
* Path: `/webhook-test/resume-screener`
* Accepts `multipart/form-data`
* Key name: `data` (used to send files)
* ⚠️ Supports multiple files only if your webhook is set up to accept binary arrays.

---

### 2. **Split In Batches (Optional)**

**Purpose**: Loops through each file if multiple resumes are uploaded.

* Input: Binary list
* Output: Single file per loop

---

### 3. **Read Binary File**

**Purpose**: Extracts plain text from each PDF resume.

* Mode: Text
* Encoding: UTF-8

---

### 4. **OpenAI Node (ChatGPT)**

**Purpose**: Analyzes resume content and generates structured output.

* Model: gpt-3.5-turbo
* Input: Resume text
* Prompt: Ask for name, skills, tags, experience, and AI Trainer fit with reason.
* Output: JSON

---

### 5. **Set Node - Extract Fields**

**Purpose**: Parses and reformats JSON reply from OpenAI into variables.

```json
{
  "name": JSON.parse($json["message"]["content"]).name,
  "is_fit_for_ai_trainer": JSON.parse($json["message"]["content"]).is_fit_for_ai_trainer,
  "reason": JSON.parse($json["message"]["content"]).reason,
  "skills": JSON.parse($json["message"]["content"]).skills,
  "tags": JSON.parse($json["message"]["content"]).tags,
  "timestamp": $now.toFormat("ccc, dd LLL yyyy")
}
```

* Note: Timestamp is prettified using Luxon

---

### 6. **Google Sheets Node**

**Purpose**: Writes each evaluated candidate row to Google Sheets.

* Action: Append Row
* Sheet ID: (Paste your sheet ID)
* Fields mapped: name, is\_fit\_for\_ai\_trainer, reason, skills, tags, timestamp

---

## ⚙️ Optional Enhancements

* **Add JSON Validator node** to check OpenAI response.
* **Switch Node** to handle errors in parsing.
* **Send Message Node** (e.g., WhatsApp or email) to notify recruiter.

---

## ✅ Why So Many Nodes?

Each node is modular for:

* Debugging
* Reusability
* Clear input-output separation
* Better control over API payloads and conditions

---

## 🧪 Testing

* Use Postman with `form-data`
* Key: `data`
* Value: One or more PDF resumes
* Run the workflow
* Check Google Sheet for rows

---

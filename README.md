
---

# 🤖 No-Code AI Agent Kit – BotCampus AI

Welcome to the official repository for the **No-Code AI Agent Kit** built by **BotCampus AI**.

This repo contains everything you need to build and run your own **AI-powered automation bots** using `n8n`, OpenAI, Google Sheets, and WhatsApp — no coding required.

---

## 📂 Repository Structure

```

no-code-ai-agent-kit/
│
├── README.md                    # You're here
├── LICENSE                      # Optional license (MIT recommended)
│
├── workflows/                   # Plug-and-play n8n agents
│   ├── whatsapp-gpt-agent.json     # Smart support bot for WhatsApp
│   ├── daily-report-agent.json     # (Coming soon)
│   ├── resume-screener.json        # Resume screening + Google Sheets
│   └── your-first-ai-agent.json    # Starter chatbot agent
│
├── instructions/                # Setup & usage guides
│   ├── install-n8n-mac.md
│   ├── how-to-import-flows.md
│   └── simulate-webhook.md
│
├── templates/                   # Prompt templates and fallbacks
│   ├── agent-prompts.md
│   └── fallback-responses.md
│
├── assets/                      # Visuals like diagrams and screenshots
│   └── flow-diagrams.png
│
└── .gitignore

```

---

## 🚀 What You’ll Build

1. **🟢 WhatsApp GPT Agent**  
   Smart bot that replies to WhatsApp queries via OpenAI.

2. **📄 Resume Screener Agent**  
   Upload multiple resumes (PDF), extract key skills, evaluate fit for AI Trainer role, and log to Google Sheets.

3. **🟣 Your First AI Agent**  
   A beginner-friendly chatbot powered by OpenAI and customizable in seconds.

---

## 📥 How to Use

1. Install `n8n` locally  
   👉 `instructions/install-n8n-mac.md`

2. Open `http://localhost:5678` (default n8n instance)

3. Import any `.json` file from `workflows/`

4. Test the agent via webhook or Postman  
   👉 `instructions/simulate-webhook.md`

5. Customize prompts in `templates/`

---

## 🧠 Resume Screener Workflow Details

- Accepts multiple resume files via a webhook  
- Uses GPT-3.5 to extract:
  - Candidate name
  - Years of experience
  - Skills
  - Fit for AI Trainer role (Yes/No + Reason)
  - Tags for categorization
- Appends all data to a connected **Google Sheet**

👉 See: `instructions/simulate-webhook.md`

---

## 👥 Who Should Use This Repo?

- Aspiring AI builders  
- Automation enthusiasts  
- Sales/support teams wanting smarter workflows  
- Participants of **BotCampus AI Workshops**

---

## 🙌 Credits

Created by **Abdullah Khan**  
Founder & CEO – [BotCampus AI](https://www.botcampus.ai)

📩 For support or workshops: `support@botcampus.ai`

---

**Your AI agents are just one workflow away.** 

---

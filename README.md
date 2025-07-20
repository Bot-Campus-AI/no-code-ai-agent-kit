
# 🤖 AI Agent Workshop – BotCampus AI

Welcome to the official repository for the **AI Agent Workshop** powered by **BotCampus AI**.

This repo contains everything you need to build, test, and deploy your own **AI-powered automation bots** using `n8n`, OpenAI, and WhatsApp Cloud API — all in a no-code/low-code environment.

---

## 📂 Repository Structure

```

ai-agent-workshop/
│
├── README.md                    # You're here
├── LICENSE                      # (Optional) License file (MIT recommended)
│
├── workflows/                   # Ready-to-import n8n flows
│   ├── support-agent.json       # WhatsApp + GPT Support Bot
│   ├── daily-report-agent.json  # Google Sheets + GPT Summary Bot
│   └── lead-qualifier.json      # FB Lead + GPT Classifier Bot
│
├── assets/                      # Visuals like screenshots or diagrams
│   └── flow-diagrams.png
│
├── instructions/                # Step-by-step setup & usage guides
│   ├── install-n8n-mac.md
│   ├── how-to-import-flows.md
│   └── simulate-webhook.md
│
├── templates/                   # Prompt templates & fallback messages
│   ├── agent-prompts.md
│   └── fallback-responses.md
│
└── .gitignore                   # Ignores unnecessary system files

```

---

## ⚙️ What You’ll Build

During this workshop, you’ll create 3 working AI agents:

1. **Support Agent Bot**  
   Auto-respond to WhatsApp queries using OpenAI

2. **Daily Report Bot**  
   Summarize Google Sheets data and send via WhatsApp

3. **Lead Qualifier Bot**  
   Classify Facebook leads and trigger customized emails

---

## 🚀 Getting Started

1. 📖 Read the setup guide:  
   [`instructions/install-n8n-mac.md`](instructions/install-n8n-mac.md)

2. 📦 Import a bot from the `workflows/` folder into your local n8n instance

3. 🧪 Test with Postman or simulated webhook calls  
   See: [`instructions/simulate-webhook.md`](instructions/simulate-webhook.md)

4. ✏️ Customize prompts in `templates/`

---

## 👥 Who Should Use This Repo?

- Tech enthusiasts exploring AI automation
- Marketers or product folks who want to build bots without coding
- Developers experimenting with OpenAI + workflows
- Workshop participants from BotCampus AI sessions

---

## 🙌 Credits

Created by **Abdullah Khan**  
Founder & CEO – [BotCampus AI](https://www.botcampus.ai)

For support, questions, or to join the next live session, contact:  
📩 `support@botcampus.ai`

---

**Let’s build your first AI Agent — together.**

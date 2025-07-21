
---

### 🛠️ How to Import Flows in n8n

Use the following steps to import a workflow JSON file into your local n8n environment:

---

#### ✅ Step-by-Step

1. **Open your local n8n instance**
   URL: `http://localhost:5678`

2. **Click “➕ Create Workflow”** (top-left corner)

3. **Click the ☰ menu (top-left)**
   Then select `Import from File`

4. **Choose the `.json` file to import**
   Example: `whatsapp-gpt-agent.json`

5. **Review and update credentials**
   Reconnect integrations like OpenAI, MySQL, etc., as credentials are not saved in `.json` exports.

6. **Click “Activate”** (top-right)
   This will enable webhook listeners or scheduled triggers.

---

#### 💡 Tips

* Rename the workflow after import for clarity.
* You can also import from clipboard via `Workflows > Import from Clipboard`.
* Avoid including secrets in the JSON before sharing it publicly.

---

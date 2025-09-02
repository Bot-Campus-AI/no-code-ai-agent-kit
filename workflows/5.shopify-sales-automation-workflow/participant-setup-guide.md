
---

## 🛠 Participant Setup Guide

**Goal:** Have all API keys, tokens, and OAuth setups ready **before** the workshop so the automation runs smoothly.

---

### **1. Gmail OAuth Setup (Free)**

* Go to **[Google Cloud Console](https://console.cloud.google.com/)**
* Create a new project → Enable **Gmail API**.
* Go to **OAuth Consent Screen** → Choose **External** → Add your email.
* Go to **Credentials** → Create **OAuth Client ID** → App type: Web.
* Add redirect URI from n8n/make.
* Save **Client ID** and **Client Secret**.

---

### **2. Trello API Key & Token (Free)**

* Go to **[Trello API Key page](https://trello.com/app-key)**.
* Copy **API Key**.
* Scroll down and click **Token** → Authorize → Copy token.
* Keep both safe.

---

### **3. Shopify API Key, Password, Shop URL (Free Dev Store)**

* Sign up for a **[Shopify Partner Account](https://partners.shopify.com/)**.
* Create a **Development Store** (no cost).
* Go to **Apps** → **Develop Apps** → Create app → Get **API Key** and **API Secret**.
* For Store URL, copy from your dev store dashboard.

---

### **4. Zoho OAuth Setup (Free Tier)**

* Go to **[Zoho API Console](https://api-console.zoho.com/)**.
* Create a new client → Type: Server-based.
* Add your redirect URI.
* Save **Client ID** and **Client Secret**.
* Authorize Zoho CRM (free plan works for testing).

---

### **5. Mailchimp API Key (Free Tier)**

* Sign up for a **[Mailchimp Free Account](https://mailchimp.com/)**.
* Go to **Profile → Extras → API Keys**.
* Generate a new API key → Copy.

---

### **6. Harvest Account ID & Token (Free Trial)**

* Go to **[Harvest](https://www.getharvest.com/)** → Start free trial (30 days).
* From dashboard → **My Profile → API** → Copy **Account ID** and **Personal Access Token**.

---

✅ **Tip:** Have all keys ready in a **Notepad** or **Google Doc** so you can copy-paste during the workshop.

---


To get your **Shopify API Key**, you’ll need to create a private app (or a custom app in newer Shopify versions) from the admin. Here’s the step-by-step:

---

### **1. Go to Apps in Shopify Admin**

* In your Shopify admin sidebar, scroll down and click **"Apps"**.
* At the bottom of the Apps page, click **"App and sales channel settings"**.

---

### **2. Create a Custom App**

* Click **"Develop apps"** in the top-right (if it’s your first time, you may need to enable app development).
* Click **"Create an app"**.
* Give it a name (e.g., `n8n-integration`) and choose your account as the app developer.

---

### **3. Configure API Access**

* Go to **"Configuration"** inside the new app.
* Under **Admin API integration**, click **"Configure"**.
* Select the necessary permissions your n8n workflow will need (for your workflow, you likely need `read_orders`, `write_orders`, `read_customers`, and possibly `write_customers`).

---

### **4. Install the App**

* Once permissions are set, click **"Install app"**.

---

### **5. Get API Credentials**

* After installation, go to the **"API credentials"** tab.
* You’ll see:

  * **Admin API access token** (this is your password/token — copy it securely)
  * **API key**
  * **API secret key**
* Also note your **Shop domain** (e.g., `dev-n8n.myshopify.com`).

---

### **6. Add to n8n**

In your n8n **Shopify credentials**:

* **API Key** → from Shopify
* **Password/Access Token** → Admin API access token
* **Shop URL** → e.g., `https://dev-n8n.myshopify.com`

---

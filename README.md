# 🤖 OrderlyBot AI — WhatsApp Restaurant Order Agent

> An intelligent, fully automated WhatsApp order-receiving agent built with **n8n**, powered by **Google Gemini AI**, and connected to **Google Sheets** — designed for restaurants that want to handle orders, menus, and customer queries 24/7 without lifting a finger.

---

## 📽️ Demo Video

A full walkthrough demo of OrderlyBot in action is included in this repository:

```
📁 OrderlyBot Demo Video/n8n project.mp4
```

Watch it to see the bot handling a real order end-to-end — from greeting to delivery confirmation.

---

## 📌 What Is OrderlyBot AI?

**OrderlyBot AI** is a no-code/low-code automation workflow built on [n8n](https://n8n.io) that turns your WhatsApp Business number into a smart, conversational order-taking agent for restaurants.

It is configured for **Al Rozi Food & Cafe** but is fully customizable for any restaurant.

The bot can:
- 💬 Chat with customers on **WhatsApp** in a natural, human-like tone
- 🍽️ Fetch and present the **live menu** from Google Sheets
- 📋 Take, confirm, and **log orders** to a Google Sheet in real time
- ❓ Answer **FAQs** (location, hours, payment, etc.) from a dedicated sheet
- 🚚 Calculate **delivery charges & ETAs** automatically
- 💳 Handle **payment methods** (Cash, Card, Online)
- 📦 Track order lifecycle: `Order Placed → Delivered → Paid`
- 🌐 Respond in the **customer's language** (e.g., English or Urdu)

---

## 🧠 Tech Stack

| Component | Technology |
|---|---|
| Automation Platform | [n8n](https://n8n.io) |
| AI / LLM | Google Gemini (via LangChain) |
| Messaging Channel | WhatsApp Business API |
| Data Storage | Google Sheets (3 sheets) |
| Memory | n8n Window Buffer Memory |

---

## 🗂️ Google Sheets Architecture

The bot uses **one Google Sheets file** with **three tabs/sheets**:

### 1. 📄 `MENU` Sheet
Contains all food items, categories, sizes, availability status, and prices.
The AI fetches this live to tell customers what's available and what isn't.

**Suggested columns:**
```
| Category | Item Name | Size | Price (Rs.) | Available (Yes/No) |
```

### 2. 📋 `ORDERS` Sheet
Every confirmed order is automatically appended here as a new row. The bot updates the **same row** as the order progresses — no duplicates.

**Columns used:**
```
| Customer Name | Phone | Address | Food Item | Quantity/Size | Status | Order Date | Total Price | Payment |
```

**Status values:** `Order Placed` → `Delivered`
**Payment values:** `COD` / `Card` / `Paid via EasyPaisa` / etc.

### 3. ❓ `FAQ's` Sheet
A list of frequently asked questions and their answers (location, timings, contact, policies, etc.). The AI answers **strictly** from this sheet — it never guesses.

**Suggested columns:**
```
| Question | Answer |
```

---

## ⚙️ How It Works — Flow Diagram

```
Customer Message (WhatsApp)
        │
        ▼
  WhatsApp Trigger (n8n)
        │
        ▼
  AI Agent (Google Gemini)
   ├── 🍽️  Get MENU        → Reads Sheet 1 (MENU)
   ├── ❓  Get FAQ's       → Reads Sheet 1 (FAQ's tab)
   └── 📋  Post Order      → Writes to Sheet 1 (ORDERS tab)
        │
        ▼
  Send Reply (WhatsApp)
        │
        ▼
  Customer receives response
```

---

## 🚀 How to Set This Up on Your Machine

Follow these steps to run OrderlyBot AI on your own n8n instance.

### ✅ Prerequisites

- [n8n](https://docs.n8n.io/getting-started/installation/) installed (self-hosted or cloud)
- A **WhatsApp Business API** account (via Meta for Developers)
- A **Google Cloud** account with Sheets API enabled
- A **Google Gemini API** key

---

### Step 1 — Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/OrderlyBot-AI.git
cd OrderlyBot-AI
```

---

### Step 2 — Import the Workflow into n8n

1. Open your n8n instance in the browser
2. Go to **Workflows** → click **Import**
3. Upload the file: `WhatsApp_OrderlyBot_AI.json`
4. The full workflow will appear with all nodes pre-configured

---

### Step 3 — Set Up Google Sheets

1. Create a new Google Spreadsheet
2. Add **three tabs** and name them exactly:
   - `MENU`
   - `ORDERS`
   - `FAQ's`
3. Populate the `MENU` tab with your restaurant's food items (category, name, size, price, availability)
4. Populate the `FAQ's` tab with common questions and answers
5. Leave `ORDERS` empty — the bot will fill it automatically
6. Copy the **Spreadsheet ID** from the URL:
   ```
   https://docs.google.com/spreadsheets/d/YOUR_SPREADSHEET_ID/edit
   ```

---

### Step 4 — Connect Google Sheets in n8n

1. In n8n, open the workflow
2. Click on the **"Get MENU"** node → set credentials → select your Google account → paste your Spreadsheet ID → select the `MENU` tab
3. Repeat for **"Get FAQ's"** node → select the `FAQ's` tab
4. Repeat for **"Post Order"** node → select the `ORDERS` tab
5. Authorize via **OAuth2** when prompted

---

### Step 5 — Connect Google Gemini

1. Click the **"Google Gemini Chat Model"** node
2. Add a new credential → paste your **Gemini API Key** (get it from [Google AI Studio](https://aistudio.google.com/))
3. Save

---

### Step 6 — Set Up WhatsApp Business API

1. Go to [Meta for Developers](https://developers.facebook.com/) → create an App → add **WhatsApp** product
2. Get your:
   - **Phone Number ID**
   - **WhatsApp Access Token**
   - **Webhook Verify Token**
3. In n8n, click **"WhatsApp Trigger"** node → add credentials with your access token
4. Click **"Send Message"** node → add WhatsApp API credentials → paste your **Phone Number ID**
5. Copy the **n8n webhook URL** from the WhatsApp Trigger node
6. In Meta Developer Console → WhatsApp → Configuration → paste the webhook URL and verify token
7. Subscribe to the **`messages`** webhook event

---

### Step 7 — Customize the AI Prompt (Optional)

The AI Agent node contains a detailed system prompt pre-configured for **Al Rozi Food & Cafe**. To adapt it to your restaurant:

1. Click the **"AI Agent"** node
2. Edit the **System Message** field:
   - Change the restaurant name
   - Update delivery charges and ETAs for your area
   - Update payment account numbers
   - Update cancellation contact number
3. Save the node

---

### Step 8 — Activate & Test

1. Click **"Activate"** (toggle) on the workflow
2. Send a WhatsApp message to your business number
3. The bot should respond within seconds 🎉

---

## 🤖 Bot Capabilities At a Glance

| Feature | Details |
|---|---|
| 🌅 Smart Greetings | Time-based (morning / afternoon / evening) |
| 🌐 Multilingual | Responds in English or Urdu based on customer |
| 🍽️ Live Menu Lookup | Fetches real-time availability from Google Sheet |
| 🛒 Upselling | Gently suggests add-ons, combos, desserts |
| 📦 Order Logging | Auto-appends to ORDERS sheet with timestamp |
| 💳 Payment Handling | COD, Card, EasyPaisa, SadaPay, NayaPay, Meezan Bank |
| 🚚 Delivery ETA | Calculated by distance (1km–5km range) |
| ❓ FAQ Answering | Strictly from the FAQ sheet — no hallucinations |
| 🔁 Order Lifecycle | Tracks Placed → Delivered → Paid in one row |
| ❌ Cancellation Handling | Redirects customer to owner's contact number |
| 🧠 Conversation Memory | Remembers last 10 messages per customer session |

---

## 📁 Repository Structure

```
OrderlyBot-AI/
│
├── WhatsApp_OrderlyBot_AI.json     # n8n workflow file (import this)
├── OrderlyBot Demo Video/
│   └── n8n project.mp4             # Full demo video of the bot in action
└── README.md                       # You are here
```

---

## 🛡️ Important Notes

- **Never share** your `WhatsApp_OrderlyBot_AI.json` publicly with real credentials inside it. Always remove or reset credentials before pushing to GitHub — n8n credential references are stored by ID only and won't expose secrets.
- The bot uses **Window Buffer Memory** with a context of 10 messages per customer session, keyed by WhatsApp ID — so each customer gets their own isolated conversation.
- The `ORDERS` sheet is append-only for new orders. Status and payment updates happen **in-place on the same row** — no duplicate rows are ever created.

---

## 👨‍💻 Author

Built with ❤️ using n8n, Google Gemini, and WhatsApp Business API.

Feel free to fork, customize, and deploy for your own restaurant or client projects.

---

## 📄 License

This project is open for personal and commercial use. Attribution appreciated but not required.

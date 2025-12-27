# 🚀 BEGINNER'S QUICKSTART GUIDE
## Daily Chronicle Newsletter - Step by Step

---

## 📁 FILES YOU NEED (In Order)

```
newsletter-v3/
├── 1️⃣ QUICKSTART_BEGINNER.md      ← YOU ARE HERE (Read this first!)
├── 2️⃣ n8n-workflows/
│   ├── 01-subscription-handler.json    ← Load 1st in N8N
│   ├── 02-content-aggregator.json      ← Load 2nd in N8N
│   ├── 03-newsletter-sender.json       ← Load 3rd in N8N
│   └── 04-unsubscribe-handler.json     ← Load 4th in N8N
└── 3️⃣ html-templates/
    └── subscription-page.html          ← Deploy to web hosting
```

**IGNORE these files** (advanced versions for later):
- ❌ 02-content-aggregator-with-agent.json
- ❌ 02-content-aggregator-anthropic-agent.json
- ❌ SETUP_GUIDE.md (detailed reference, not needed now)

---

## ⏱️ TOTAL TIME: ~45 minutes

---

# STEP 1: Create Free Accounts (15 mins)

### 1.1 Google Account
- Go to: https://accounts.google.com
- Create account or use existing
- You need this for Google Sheets & Gmail

### 1.2 N8N Cloud Account  
- Go to: https://n8n.io
- Click "Get Started Free"
- Sign up with email
- Free plan = 500 executions/month

### 1.3 YouTube API Key
- Go to: https://console.cloud.google.com
- Create new project (name it "Newsletter")
- Search "YouTube Data API v3" → Enable it
- Go to Credentials → Create Credentials → API Key
- Copy and save the API key

### 1.4 OpenRouter API Key
- Go to: https://openrouter.ai
- Sign up (free)
- Go to Keys → Create Key
- Copy and save the API key

---

# STEP 2: Create Google Sheet Database (10 mins)

### 2.1 Create New Spreadsheet
- Go to: https://sheets.google.com
- Click "+ Blank" to create new spreadsheet
- Name it: "Newsletter Database"

### 2.2 Create 5 Sheets (Tabs)
Click the "+" at bottom to add sheets. Create these 5:

**Sheet 1: "Subscribers"** (rename Sheet1)
Add these headers in Row 1:
```
A1: subscriber_id
B1: email
C1: name
D1: topics
E1: subscribed_date
F1: status
G1: last_email_sent
H1: unsubscribe_token
I1: unsubscribed_date
```

**Sheet 2: "Topics"** (click + to add)
Add headers and data:
```
Row 1: topic_id | topic_name | keywords | active
Row 2: T001 | Technology | tech,AI,software,programming | TRUE
Row 3: T002 | Science | science,research,discovery | TRUE
Row 4: T003 | Business | business,finance,economy,startup | TRUE
Row 5: T004 | Health | health,medical,wellness,fitness | TRUE
Row 6: T005 | Sports | sports,football,cricket,basketball | TRUE
Row 7: T006 | Entertainment | movies,music,celebrity,entertainment | TRUE
```

**Sheet 3: "Content_Cache"** (click + to add)
Add these headers in Row 1:
```
A1: topic_id
B1: topic_name
C1: fetch_date
D1: hook
E1: lead
F1: stories
G1: sources
H1: content
I1: content_ids
```

**Sheet 4: "Content_Sent"** (click + to add)
Add these headers in Row 1:
```
A1: content_id
B1: sent_date
C1: topic
```

**Sheet 5: "Newsletter_Log"** (click + to add)
Add these headers in Row 1:
```
A1: email
B1: topic
C1: hook
D1: sent_at
```

### 2.3 Copy Your Spreadsheet ID
- Look at the URL: `https://docs.google.com/spreadsheets/d/XXXXXXXXX/edit`
- Copy the XXXXXXXXX part (your Spreadsheet ID)
- Save it somewhere - you'll need it!

---

# STEP 3: Setup N8N Credentials (10 mins)

### 3.1 Login to N8N
- Go to your N8N instance (cloud or self-hosted)
- Click on the menu icon (☰)

### 3.2 Create Google Sheets Credential
1. Go to: **Credentials** (left sidebar)
2. Click: **+ Add Credential**
3. Search: "Google Sheets"
4. Select: "Google Sheets OAuth2 API"
5. Click: **Sign in with Google**
6. Select your Google account
7. Allow permissions
8. Click: **Save**

### 3.3 Create Gmail Credential
1. Click: **+ Add Credential**
2. Search: "Gmail"
3. Select: "Gmail OAuth2 API"
4. Click: **Sign in with Google**
5. Select same Google account
6. Allow permissions
7. Click: **Save**

---

# STEP 4: Import Workflows into N8N (10 mins)

### 4.1 Import Workflow 1: Subscription Handler
1. In N8N, click: **+ Add Workflow**
2. Click: **⋮** (three dots menu) → **Import from File**
3. Select: `01-subscription-handler.json`
4. Click the **Google Sheets node** ("Add to Sheet")
5. Click **Credential** dropdown → Select your Google Sheets credential
6. Click **Document** → Select "Newsletter Database"
7. Click **Sheet** → Select "Subscribers"
8. Click the **Gmail node** ("Welcome Email")
9. Click **Credential** dropdown → Select your Gmail credential
10. Click: **Save**
11. Click: **Activate** (toggle at top right)
12. Click the **Webhook node** → Copy the webhook URL (save it!)

### 4.2 Import Workflow 2: Content Aggregator
1. Click: **+ Add Workflow**
2. Import: `02-content-aggregator.json`
3. Update ALL Google Sheets nodes:
   - Click each one → Select credential → Select spreadsheet → Select correct sheet
4. Find the **YouTube** node:
   - Replace `YOUR_YOUTUBE_API_KEY` with your actual key
5. Find the **AI** node (HTTP Request to OpenRouter):
   - Replace `YOUR_OPENROUTER_KEY` with your actual key
6. Click: **Save**
7. Click: **Activate**

### 4.3 Import Workflow 3: Newsletter Sender
1. Click: **+ Add Workflow**
2. Import: `03-newsletter-sender.json`
3. Update ALL Google Sheets nodes (same as above)
4. Update Gmail node with your credential
5. In the **Build HTML** code node, replace:
   - `YOUR_UNSUBSCRIBE_URL` with your unsubscribe webhook (from step 4.4)
6. Click: **Save**
7. Click: **Activate**

### 4.4 Import Workflow 4: Unsubscribe Handler
1. Click: **+ Add Workflow**
2. Import: `04-unsubscribe-handler.json`
3. Update Google Sheets nodes with credential + spreadsheet
4. Click: **Save**
5. Click: **Activate**
6. Copy the webhook URL (you need this for step 4.3)

---

# STEP 5: Deploy Subscription Page (5 mins)

### Option A: Use GitHub Pages (Easiest)
1. Go to: https://github.com → Sign up/Login
2. Create new repository: "newsletter-signup"
3. Upload `subscription-page.html` and rename to `index.html`
4. Go to: Settings → Pages → Enable GitHub Pages
5. Your page will be at: `https://yourusername.github.io/newsletter-signup`

### Option B: Use Netlify (Also Easy)
1. Go to: https://netlify.com → Sign up
2. Drag & drop the `subscription-page.html` file
3. Get your free URL

### 5.1 Update the Webhook URL in Page
1. Open `subscription-page.html` in a text editor
2. Find line ~95: `fetch('YOUR_N8N_WEBHOOK_URL'`
3. Replace with your subscription webhook URL from Step 4.1
4. Re-upload the file

---

# STEP 6: Test Everything (5 mins)

### 6.1 Test Subscription
1. Open your subscription page
2. Fill in: Name, Email, Select a Topic
3. Click Subscribe
4. Check your email for welcome message
5. Check Google Sheet "Subscribers" tab for new entry ✓

### 6.2 Test Content Fetching (Manual)
1. Open workflow: "02-Content-Aggregator"
2. Click: **Execute Workflow** (play button)
3. Wait 30-60 seconds
4. Check Google Sheet "Content_Cache" for new content ✓

### 6.3 Test Newsletter Sending (Manual)
1. Open workflow: "03-Newsletter-Sender"
2. Click: **Execute Workflow**
3. Check your email for newsletter ✓

---

# ✅ DONE! 

Your newsletter system is now set up!

**What happens automatically:**
- 6:00 AM daily → Content is fetched from 5 sources
- 8:00 AM daily → Newsletters are sent to subscribers

**Your URLs to save:**
- Subscription Page: (your GitHub/Netlify URL)
- Subscribe Webhook: (from N8N workflow 1)
- Unsubscribe Webhook: (from N8N workflow 4)

---

# 🔧 TROUBLESHOOTING

### "Could not find property" on import
- Make sure you're using N8N version 1.122.4+
- Try creating the workflow manually using the JSON as reference

### No content being fetched
- Check Topics sheet has "TRUE" in active column
- Check YouTube API key is correct
- Run workflow manually and check error logs

### Emails not sending
- Check Gmail credential is connected
- Check spam folder
- Gmail limit is 500/day

### Webhook not working
- Make sure workflow is ACTIVATED (green toggle)
- Check the webhook URL is correct (no extra spaces)

---

# 📞 NEED HELP?

1. Check N8N execution logs for errors
2. Test each workflow individually first
3. Make sure all credentials are connected
4. Verify API keys are correct


---

# 🎨 OPTION B: USE LOVABLE.DEV FOR UI (Alternative to Step 5)

Instead of using the static HTML file, you can use **Lovable.dev** to generate a beautiful React-based subscription page.

## What is Lovable?
Lovable.dev is an AI tool that generates full React applications from text prompts. It's great for creating professional UIs without coding.

---

## STEP 5B: Create UI with Lovable (10 mins)

### 5B.1 Create Lovable Account
1. Go to: https://lovable.dev
2. Sign up for free account
3. Click: **New Project**

### 5B.2 Use the Provided Prompt
1. Open the file: `lovable-prompts/subscription-page-prompt.md`
2. Copy the ENTIRE content of that file
3. Paste it into Lovable's chat
4. Wait for Lovable to generate the UI

### 5B.3 Update the Webhook URL in Lovable
1. In the generated code, find the `fetch()` function
2. Look for: `'YOUR_N8N_WEBHOOK_URL'`
3. Replace with your actual N8N subscription webhook URL (from Step 4.1)

Example:
```javascript
// Change this:
fetch('YOUR_N8N_WEBHOOK_URL', {

// To this:
fetch('https://your-n8n.app/webhook/subscribe', {
```

### 5B.4 Deploy from Lovable
1. Click: **Deploy** button in Lovable
2. Choose: "Deploy to Lovable hosting" (free)
3. Get your live URL
4. Share this URL for people to subscribe!

---

## 🔗 CONNECTING LOVABLE UI TO N8N

### How it works:

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   LOVABLE UI    │         │    N8N WEBHOOK  │         │  GOOGLE SHEETS  │
│                 │  POST   │                 │  SAVE   │                 │
│  User fills     │────────►│  01-subscription│────────►│  Subscribers    │
│  form & clicks  │ {name,  │  -handler       │         │  sheet          │
│  Subscribe      │  email, │                 │         │                 │
│                 │  topics}│                 │         │                 │
└─────────────────┘         └────────┬────────┘         └─────────────────┘
                                     │
                                     │ SEND
                                     ▼
                            ┌─────────────────┐
                            │     GMAIL       │
                            │  Welcome Email  │
                            └─────────────────┘
```

### The Data Flow:
1. User fills form in Lovable UI
2. Lovable sends POST request to N8N webhook
3. N8N validates and saves to Google Sheets
4. N8N sends welcome email via Gmail
5. N8N returns success/error to Lovable
6. Lovable shows success/error message to user

---

## 📝 LOVABLE PROMPT CUSTOMIZATION

The prompt in `lovable-prompts/subscription-page-prompt.md` creates:
- ✅ Professional newspaper-style header
- ✅ 3 feature cards
- ✅ Name & email input fields
- ✅ Topic selection checkboxes (6 topics)
- ✅ Custom topic input
- ✅ Loading states
- ✅ Success/error messages
- ✅ Mobile responsive design

### To Customize:
You can modify the prompt to:
- Change colors (edit the hex codes)
- Add/remove topics
- Change the newsletter name
- Add more form fields
- Change the styling

Just edit the prompt file and re-paste into Lovable!

---

## 🆚 HTML FILE vs LOVABLE UI

| Feature | HTML File | Lovable UI |
|---------|-----------|------------|
| Setup Time | 5 mins | 10 mins |
| Customization | Edit HTML manually | Edit via AI chat |
| Hosting | GitHub Pages/Netlify | Lovable hosting |
| Updates | Re-upload file | Edit & re-deploy |
| Look | Good | More polished |
| Best For | Quick setup | Professional look |

**Recommendation:** Start with HTML file for quick testing, switch to Lovable for production.


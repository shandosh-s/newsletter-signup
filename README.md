# Daily Chronicle - AI-Powered Newsletter System

**An automated, AI-powered newsletter micro-SaaS that aggregates content from multiple sources and delivers beautiful, page-flip e-newspapers to subscribers daily.**

**Tech Stack:** N8N | Google Sheets | OpenRouter AI | Gmail | GitHub Pages

---

## Features

### Multi-Source Content Aggregation
- Reddit (Hot posts and discussions)
- Google News (Latest headlines)
- Yahoo News (News coverage)
- YouTube (Video content)
- Hacker News (Tech discussions)

### AI-Powered Curation
- Intelligent content summarization
- Engaging headline generation
- Story prioritization and selection

### Beautiful E-Newspaper Format
- Realistic page-flip reading experience
- Professional newspaper typography
- Source attribution with logos
- Download as PDF option

### Fully Automated Pipeline
- Daily content fetching (6:00 AM)
- Newsletter generation and delivery (8:00 AM)
- Zero manual intervention required

### Duplicate Prevention
- Tracks all sent content
- 7-day lookback window
- Never sends the same article twice

### Complete Subscription Management
- User-friendly signup form
- Topic-based preferences
- Welcome emails
- One-click unsubscribe

---

## System Architecture

```
                         DAILY CHRONICLE SYSTEM
    
    +----------------+     +----------------+     +----------------+
    |  SUBSCRIPTION  |     |    CONTENT     |     |   NEWSLETTER   |
    |     FLOW       |     |  AGGREGATION   |     |    DELIVERY    |
    +-------+--------+     +-------+--------+     +-------+--------+
            |                      |                      |
            v                      v                      v
    +----------------+     +----------------+     +----------------+
    |    GitHub      |     |      N8N       |     |     Gmail      |
    |   Pages UI     |     |   Workflows    |     |     SMTP       |
    +-------+--------+     +-------+--------+     +----------------+
            |                      |
            |        POST          |
            +--------------------->|
                                   |
                                   v
                         +------------------+
                         |  Google Sheets   |
                         |   (Database)     |
                         |                  |
                         |  - Subscribers   |
                         |  - Topics        |
                         |  - Content_Cache |
                         |  - Content_Sent  |
                         |  - Newsletter_Log|
                         +--------+---------+
                                  |
                                  v
              +-------------------+-------------------+
              |           CONTENT SOURCES            |
              |                                      |
              |  Reddit  |  Google News  |  Yahoo    |
              |  YouTube |  Hacker News             |
              +-------------------+-------------------+
                                  |
                                  v
                         +------------------+
                         |   OpenRouter     |
                         |  (AI Summary)    |
                         +------------------+
```

---

## Project Structure

```
daily-chronicle/
|
|-- README.md                             # This file
|-- QUICKSTART_BEGINNER.md                # Step-by-step setup guide
|
|-- n8n-workflows/
|   |-- 01-subscription-handler.json      # Handles new subscriptions
|   |-- 02-content-aggregator.json        # Fetches and processes content
|   |-- 03-newsletter-sender.json         # Generates and sends newsletters
|   |-- 04-unsubscribe-handler.json       # Handles unsubscribe requests
|
|-- html-templates/
|   |-- subscription-page.html            # Subscription form (static HTML)
|
|-- lovable-prompts/
    |-- subscription-page-prompt.md       # Prompt for Lovable.dev UI generation
```

---

## Tech Stack

| Component  | Technology           | Purpose                          |
|------------|----------------------|----------------------------------|
| Automation | N8N                  | Workflow orchestration           |
| Database   | Google Sheets        | Store subscribers, content, logs |
| AI         | OpenRouter Mistral-7B| Content summarization            |
| Email      | Gmail SMTP           | Send newsletters                 |
| Frontend   | HTML/CSS/JS          | Subscription page                |
| Hosting    | GitHub Pages         | Host subscription form           |
| Video API  | YouTube Data API     | Fetch video content              |

---

## Prerequisites

Before you begin, ensure you have:

1. Google Account (for Sheets and Gmail)
2. N8N Account (cloud or self-hosted)
3. YouTube Data API Key
4. OpenRouter API Key (free tier available)
5. GitHub Account (for hosting)

---

## Quick Start

### Step 1: Clone or Download

```
git clone https://github.com/yourusername/daily-chronicle.git
cd daily-chronicle
```

### Step 2: Create Google Sheets Database

Create a new Google Spreadsheet with 5 sheets:

**Sheet 1: Subscribers**
Columns: subscriber_id, email, name, topics, subscribed_date, status, last_email_sent, unsubscribe_token, unsubscribed_date

**Sheet 2: Topics**
Columns: topic_id, topic_name, keywords, active

**Sheet 3: Content_Cache**
Columns: topic_id, topic_name, fetch_date, hook, lead, stories, sources, content, content_ids

**Sheet 4: Content_Sent**
Columns: content_id, sent_date, topic

**Sheet 5: Newsletter_Log**
Columns: email, topic, hook, sent_at

### Step 3: Add Default Topics

In the Topics sheet, add these rows:

| topic_id | topic_name    | keywords                          | active |
|----------|---------------|-----------------------------------|--------|
| T001     | Technology    | tech,AI,software,programming      | TRUE   |
| T002     | Science       | science,research,discovery        | TRUE   |
| T003     | Business      | business,finance,economy,startup  | TRUE   |
| T004     | Health        | health,medical,wellness,fitness   | TRUE   |
| T005     | Sports        | sports,football,cricket,basketball| TRUE   |
| T006     | Entertainment | movies,music,celebrity            | TRUE   |

### Step 4: Setup N8N Credentials

1. Google Sheets OAuth2 - Connect your Google account
2. Gmail OAuth2 - Connect for sending emails

### Step 5: Import N8N Workflows

Import in this order:
1. 01-subscription-handler.json
2. 02-content-aggregator.json
3. 03-newsletter-sender.json
4. 04-unsubscribe-handler.json

### Step 6: Configure Workflows

For each workflow:
1. Connect Google Sheets credential
2. Select your spreadsheet
3. Select correct sheet names
4. Add API keys where needed

### Step 7: Deploy Subscription Page

1. Update webhook URL in subscription-page.html
2. Upload to GitHub Pages
3. Share your subscription URL!

---

## Configuration

### API Keys Required

| Service           | Where to Get            | Cost                    |
|-------------------|-------------------------|-------------------------|
| YouTube Data API  | Google Cloud Console    | Free (10,000 units/day) |
| OpenRouter        | openrouter.ai           | Free tier available     |

### Workflow Schedule

| Workflow           | Default Time | Purpose              |
|--------------------|--------------|----------------------|
| Content Aggregator | 6:00 AM      | Fetch daily content  |
| Newsletter Sender  | 8:00 AM      | Send to subscribers  |

To change schedule, edit the Schedule Trigger node in each workflow.

---

## Newsletter Preview

The generated e-newspaper includes:

| Page   | Content                      |
|--------|------------------------------|
| Cover  | Topic, date, main headline   |
| Page 2 | Lead story with image        |
| Page 3 | Secondary stories (4 articles)|
| Page 4 | More highlights (4 articles) |
| Page 5 | Video section (YouTube)      |
| Page 6 | Sources and credits          |
| Page 7 | Thank you message            |
| Back   | Unsubscribe link             |

**Features:**
- Realistic page-flip animation (Turn.js)
- Print/Download as PDF
- Mobile responsive
- Source attribution with logos

---

## Troubleshooting

### Common Issues

| Problem                   | Solution                                      |
|---------------------------|-----------------------------------------------|
| Import error in N8N       | Ensure N8N version is 1.122.4 or higher       |
| Webhook not working       | Check workflow is ACTIVATED (green toggle)    |
| No data in Google Sheets  | Verify credentials and sheet names            |
| Emails not sending        | Check Gmail OAuth, verify spam folder         |
| No content fetched        | Verify Topics sheet has active = TRUE         |
| Duplicate content         | Check Content_Sent sheet is being populated   |

### Debug Steps

1. Check N8N Execution Logs
   - Go to Executions tab
   - Look for failed runs
   - Check error messages

2. Test Webhook Manually
   - Open browser console (F12)
   - Submit form
   - Check Network tab for response

3. Verify Google Sheets Connection
   - Open workflow
   - Click Google Sheets node
   - Test with Execute Node button

---

## Free Tier Limits

| Service       | Limit                              |
|---------------|------------------------------------|
| N8N Cloud     | 500 executions/month               |
| YouTube API   | 10,000 units/day (about 100 searches)|
| Gmail         | 500 emails/day                     |
| OpenRouter    | Varies by model                    |
| GitHub Pages  | Unlimited                          |

---

## Roadmap

- Add more content sources (LinkedIn, Twitter/X)
- Multiple newsletter templates
- Analytics dashboard
- A/B testing for headlines
- Multi-language support
- Custom sending schedules per user

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch: git checkout -b feature/AmazingFeature
3. Commit your changes: git commit -m 'Add some AmazingFeature'
4. Push to the branch: git push origin feature/AmazingFeature
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## Acknowledgments

- N8N (n8n.io) - Workflow automation
- Turn.js - Page flip effect
- OpenRouter (openrouter.ai) - AI API gateway
- Google Fonts - Typography (Playfair Display)

---

## Support

If you encounter any issues:

1. Check the Troubleshooting section above
2. Review the QUICKSTART_BEGINNER.md guide
3. Open an issue on GitHub

---

Made with love for news enthusiasts

Star this repo if you found it helpful!

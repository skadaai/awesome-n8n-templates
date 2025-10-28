# AI Lead Generation Workflow - Setup Guide

## Overview
This n8n workflow automatically scrapes leads from Outscraper (Google Maps), qualifies them using Gemini AI, generates personalized outreach messages, and saves everything to Google Sheets.

## What This Workflow Does

1. **Scrapes Leads** - Uses Outscraper API to find businesses based on your search query
2. **Extracts Data** - Pulls company name, phone, email, website, address, rating, reviews, etc.
3. **AI Qualification** - Gemini analyzes each lead and assigns a qualification score (0-100)
4. **Generates Insights** - AI identifies pain points, key insights, and decision maker titles
5. **Personalizes Outreach** - Creates a custom message for each lead
6. **Saves to Google Sheets** - All data organized and ready to use

## Setup Instructions

### Step 1: Get Your API Keys

#### Outscraper API
1. Go to https://outscraper.com/
2. Sign up for an account (free tier available)
3. Navigate to Profile → API → Create API Key
4. Copy your API key

#### Gemini API
1. Go to https://aistudio.google.com/app/apikey
2. Click "Get API Key" or "Create API Key"
3. Copy your API key (starts with "AIza...")

### Step 2: Create Your Google Sheet

1. Create a new Google Sheet
2. Name it "Lead Database" (or whatever you prefer)
3. The workflow will automatically create headers when it runs
4. Copy the Sheet ID from the URL (it's the long string between /d/ and /edit)
   - Example: `https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit`

### Step 3: Import the Workflow to n8n

1. Open your n8n instance
2. Click "Add Workflow" or the "+" button
3. Click the three dots menu (⋮) → "Import from File"
4. Select the `lead_generation_workflow.json` file
5. The workflow will load with all nodes

### Step 4: Configure Credentials

#### Outscraper API Credential
1. In n8n, go to Settings → Credentials
2. Click "Add Credential" → Search for "HTTP Header Auth"
3. Name it "Outscraper API Key"
4. Set:
   - **Name**: `X-API-KEY`
   - **Value**: Your Outscraper API key
5. Save

#### Gemini API Credential
1. In n8n, go to Settings → Credentials
2. Click "Add Credential" → Search for "Query Auth"
3. Name it "Gemini API Key"
4. Set:
   - **Name**: `key`
   - **Value**: Your Gemini API key
5. Save

#### Google Sheets Credential
1. In the workflow, click on the "Google Sheets" node
2. Click "Create New Credential"
3. Follow the OAuth2 flow to connect your Google account
4. Authorize n8n to access your Google Sheets

### Step 5: Configure the Workflow

#### Update the Outscraper Node
1. Click on "Outscraper API" node
2. In the "query" parameter, change the search query to your target:
   - Example: `"real estate agents in Los Angeles"`
   - Example: `"restaurants in New York"`
   - Example: `"law firms in Chicago"`
3. Adjust the "limit" parameter (max 20 for free tier, more for paid)

#### Update the Google Sheets Node
1. Click on "Google Sheets" node
2. Select your credential
3. Click "Document ID" → Select your sheet
4. Click "Sheet Name" → Select "Sheet1" (or your sheet name)

#### Customize the AI Prompts (Optional)
1. Click on "Gemini - Qualify Lead" node to customize qualification criteria
2. Click on "Gemini - Personalize Message" node to adjust outreach tone/style

### Step 6: Test the Workflow

1. Click "Execute Workflow" button (play icon)
2. Watch each node execute step by step
3. Check your Google Sheet for results
4. Review the qualification scores and personalized messages

## Workflow Behavior

### Scheduling
- Currently set to run every Monday at 9 AM
- To change: Click "Schedule Trigger" node → Modify "Cron Expression"
- Common schedules:
  - Daily at 9 AM: `0 9 * * *`
  - Every 6 hours: `0 */6 * * *`
  - Weekdays at 8 AM: `0 8 * * 1-5`

### Lead Qualification Scoring
- **70-100**: Hot Lead (high priority, good fit)
- **50-69**: Warm Lead (moderate priority)
- **0-49**: Cold Lead (low priority or poor fit)

### Output Columns in Google Sheets
1. Timestamp
2. Company Name
3. Phone
4. Email
5. Website
6. Address
7. Category
8. Rating
9. Reviews Count
10. Qualification Score
11. Decision Maker (suggested role)
12. Key Insights
13. Pain Points
14. Personalized Message
15. Status (Hot/Warm/Cold Lead)

## Customization Ideas

### Change the Search Query
Modify the Outscraper API node to target different niches:
- B2B: "SaaS companies", "consulting firms", "agencies"
- B2C: "coffee shops", "fitness centers", "salons"
- Location-based: Add city names for local targeting

### Adjust AI Behavior
- Edit the Gemini prompts to focus on different qualification criteria
- Change the personalized message tone (formal, casual, urgent)
- Add more analysis points (budget estimation, tech stack, etc.)

### Add More Features
- **Email Sending**: Add a Gmail node to automatically send the personalized messages
- **Slack Notifications**: Alert your team when hot leads are found
- **CRM Integration**: Replace Google Sheets with HubSpot, Salesforce, or Pipedrive
- **Duplicate Detection**: Add a filter to skip companies you've already contacted

## Troubleshooting

### "Credential not found" error
- Make sure you've created the credentials in Settings → Credentials
- Verify the credential names match exactly

### Outscraper returns no results
- Check your API key is valid and has credits
- Try a broader search query
- Verify the location/query exists on Google Maps

### Gemini API errors
- Ensure your API key is correct
- Check you haven't exceeded rate limits (60 requests/minute)
- Verify the model name is correct: `gemini-2.0-flash-exp`

### Google Sheets not updating
- Confirm you've authorized the OAuth connection
- Check the Sheet ID is correct
- Verify the sheet isn't protected or restricted

## Cost Estimation

**For 100 leads per day:**
- Outscraper: ~$5-10/month (depending on plan)
- Gemini API: Nearly free (2 calls per lead × ~1,000 tokens = ~$0.05 total)
- n8n: Free (self-hosted) or starts at $20/month (cloud)

**Total**: ~$25-40/month for automated lead generation

---

Need help? Feel free to reach out with any questions!

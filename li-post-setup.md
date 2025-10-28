# LinkedIn Content Automation Workflow - Setup Guide

## Overview
This n8n workflow automatically discovers trending articles from RSS feeds, analyzes them with Gemini AI, generates engaging LinkedIn posts, and organizes everything in a Google Sheets content calendar for easy scheduling and tracking.

## What This Workflow Does

1. **Monitors RSS Feeds** - Pulls latest articles from industry sources (TechCrunch, The Verge, etc.)
2. **Filters Recent Content** - Only processes articles from the last 24 hours
3. **AI Content Analysis** - Gemini evaluates each article for relevance and engagement potential
4. **Smart Filtering** - Only keeps articles with 60+ relevance score
5. **Generates LinkedIn Posts** - Creates professional, engaging posts optimized for LinkedIn
6. **Suggests Optimal Timing** - AI recommends the best date/time to post
7. **Content Calendar** - Everything organized in Google Sheets with performance tracking

## Setup Instructions

### Step 1: Get Your Gemini API Key

1. Go to https://aistudio.google.com/app/apikey
2. Click "Get API Key" or "Create API Key"
3. Copy your API key (starts with "AIza...")

### Step 2: Create Your Google Sheets Content Calendar

1. Create a new Google Sheet
2. Name it "LinkedIn Content Calendar" (or your preference)
3. The workflow will automatically create all necessary columns:
   - Post ID, Status, Scheduled Date/Time
   - Post Content, Article Title, Article Link
   - Relevance Score, Engagement Potential
   - Target Audience, Key Takeaway
   - Performance Metrics (Likes, Comments, Shares)
   - Notes
4. Copy the Sheet ID from the URL
   - Example: `https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit`

### Step 3: Import the Workflow

1. Open your n8n instance
2. Click "Add Workflow" or the "+" button
3. Click the three dots menu (⋮) → "Import from File"
4. Select `linkedin_content_automation.json`
5. The workflow will load with all nodes

### Step 4: Configure Credentials

#### Gemini API Credential
1. In n8n, go to Settings → Credentials
2. Click "Add Credential" → Search for "Query Auth"
3. Name it "Gemini API Key"
4. Set:
   - **Name**: `key`
   - **Value**: Your Gemini API key
5. Save

#### Google Sheets Credential
1. In the workflow, click on "Add to Content Calendar" node
2. Click "Create New Credential"
3. Follow OAuth2 flow to connect your Google account
4. Authorize n8n to access your Google Sheets

### Step 5: Configure RSS Feeds

The workflow comes with 2 RSS feeds pre-configured:
- **TechCrunch**: Technology and startup news
- **The Verge**: Tech, science, and culture

#### To Add More RSS Feeds:
1. Click the "+" button between "Schedule Trigger" and "Merge RSS Feeds"
2. Search for "RSS Feed Read" node
3. Add the RSS feed URL
4. Name it descriptively (e.g., "RSS Feed - Wired")
5. Connect it to the "Merge RSS Feeds" node

#### Recommended RSS Feeds for B2B/Tech:

**AI & Technology:**
- TechCrunch: `https://feeds.feedburner.com/TechCrunch/`
- The Verge: `https://www.theverge.com/rss/index.xml`
- Wired: `https://www.wired.com/feed/rss`
- VentureBeat AI: `https://venturebeat.com/category/ai/feed/`
- MIT Technology Review: `https://www.technologyreview.com/feed/`

**Business & Marketing:**
- Harvard Business Review: `https://feeds.hbr.org/harvardbusiness`
- Fast Company: `https://www.fastcompany.com/latest/rss`
- Marketing Land: `https://marketingland.com/feed`
- CMSWire: `https://www.cmswire.com/rss/`

**Industry Specific:**
- SaaS: `https://www.saastr.com/feed/`
- eCommerce: `https://www.practicalecommerce.com/feed`
- Finance: `https://www.finextra.com/rss/headlines.aspx`

### Step 6: Update Google Sheets Connection

1. Click on "Add to Content Calendar" node
2. Select your Google Sheets credential
3. Click "Document ID" → Select your calendar sheet
4. Click "Sheet Name" → Select "Sheet1" (or your sheet name)

### Step 7: Customize AI Prompts (Optional)

#### Analysis Prompt (Node: "Gemini - Analyze Article")
Edit to match your agency's focus:
- Change industry focus (e.g., healthcare, finance, real estate)
- Adjust scoring criteria
- Modify target audience definitions

#### Post Creation Prompt (Node: "Gemini - Create LinkedIn Post")
Customize your brand voice:
- Tone: Professional, casual, authoritative, friendly
- Length preferences
- Hashtag strategy
- CTA style

### Step 8: Test the Workflow

1. Click "Execute Workflow" button (play icon)
2. Watch the nodes execute step by step
3. Check your Google Sheet for new posts
4. Review the generated content and scores

## Workflow Behavior

### Scheduling
- **Default**: Runs daily at 8 AM
- **To change**: Click "Schedule Trigger" → Modify "Cron Expression"
- **Common schedules**:
  - Twice daily: `0 8,16 * * *`
  - Every 6 hours: `0 */6 * * *`
  - Weekdays only: `0 8 * * 1-5`

### Content Filtering
Articles must meet BOTH criteria to be added to calendar:
- **Relevance Score**: 60+ (relevant to B2B tech/AI)
- **Engagement Potential**: 60+ (likely to generate interaction)

You can adjust these thresholds in the "Filter Recommended Only" node.

### Post Status Flow
1. **Draft** - Generated and ready for review
2. **Scheduled** - Approved and ready to post (manually update in Sheet)
3. **Posted** - Published to LinkedIn (manually update + add timestamp)
4. **Archived** - Past posts for reference

## Using the Content Calendar

### Daily Workflow:
1. **Morning**: Check new posts added overnight
2. **Review**: Read each draft post, edit if needed
3. **Approve**: Change status from "Draft" to "Scheduled"
4. **Post**: Copy content to LinkedIn at suggested time
5. **Track**: Update "Posted At" timestamp and performance metrics

### Google Sheets Columns Explained:

| Column | Purpose |
|--------|---------|
| **Post ID** | Unique identifier for tracking |
| **Status** | Draft / Scheduled / Posted / Archived |
| **Scheduled Date** | AI-suggested posting date |
| **Scheduled Time** | AI-suggested posting time |
| **Post Content** | Ready-to-copy LinkedIn post text |
| **Article Title** | Source article headline |
| **Article Link** | Link to original article |
| **Source** | Which RSS feed (TechCrunch, etc.) |
| **Relevance Score** | 0-100, how relevant to your audience |
| **Engagement Potential** | 0-100, predicted engagement |
| **Target Audience** | Who should read this |
| **Key Takeaway** | Main insight from article |
| **Created At** | When workflow generated this |
| **Posted At** | When you actually posted (manual entry) |
| **Likes** | Performance tracking (manual entry) |
| **Comments** | Performance tracking (manual entry) |
| **Shares** | Performance tracking (manual entry) |
| **Notes** | Any additional context |

### Best Practices:

1. **Review Before Posting**: Always read and customize AI-generated posts
2. **Add Personal Touch**: Insert your own insights or experiences
3. **Track Performance**: Update metrics 24-48 hours after posting
4. **Optimize Over Time**: Notice which topics/formats perform best
5. **Maintain Consistency**: Post 3-5 times per week for best results

## Advanced Customization

### Add Multiple Industries
Duplicate the analysis node and create industry-specific filters:
- Tech posts (Monday, Wednesday, Friday)
- Business strategy posts (Tuesday, Thursday)
- Case studies (weekends)

### A/B Test Post Formats
Create variations:
- Different hooks
- Various lengths
- With/without questions
- Different hashtag strategies

### Automated Performance Tracking
Add a second workflow that:
1. Reads the Google Sheet
2. Uses LinkedIn API to fetch post metrics
3. Updates performance columns automatically

### Email Digest
Add Gmail node to send daily summary:
- "5 new posts ready for review"
- Top-scoring articles
- Suggested posting schedule for the week

## Cost Estimation

**For 30 posts per month:**
- Gemini API: ~$0.50/month (3 calls per post × 300 tokens)
- RSS Feeds: Free
- n8n: Free (self-hosted) or $20/month (cloud)
- Google Sheets: Free

**Total**: ~$20-21/month for fully automated LinkedIn content

## Troubleshooting

### No articles appearing in calendar
- Check RSS feed URLs are working (test in browser)
- Verify relevance score threshold (try lowering to 40)
- Confirm date filter is set correctly (last 24 hours)
- Check Gemini API quota

### Generated posts are too generic
- Edit the "Gemini - Create LinkedIn Post" prompt
- Add more specific instructions about your brand voice
- Include examples of your best-performing posts
- Adjust target audience definition

### Wrong posting times suggested
- Edit "Gemini - Suggest Schedule" prompt
- Specify your audience's timezone
- Add constraints (e.g., "never weekends")
- Provide historical performance data

### Google Sheets not updating
- Verify OAuth connection is active
- Check Sheet ID is correct
- Ensure sheet isn't protected
- Confirm column mapping is correct

## Content Strategy Tips

### Topic Diversity
Use different RSS feeds to cover:
- Industry news (30%)
- How-to/educational (40%)
- Opinion/thought leadership (20%)
- Company updates (10%)

### Engagement Tactics
- Ask open-ended questions
- Share contrarian viewpoints
- Use data/statistics
- Tell stories
- Reference current events

### Hashtag Strategy
The AI will suggest hashtags, but consider:
- Mix broad (#AI, #Technology) with niche (#B2BMarketing)
- Use 3-5 hashtags maximum
- Track which hashtags drive engagement
- Create a branded hashtag

## Next Steps

### Week 1: Setup & Testing
- Import workflow and configure credentials
- Add 3-5 relevant RSS feeds
- Generate 20 sample posts
- Review and refine AI prompts

### Week 2: Content Production
- Start posting 3-5 times per week
- Track performance metrics
- Identify top-performing topics
- Adjust filtering criteria

### Week 3: Optimization
- Analyze what content resonates
- Fine-tune AI prompts based on results
- Experiment with posting times
- Build a content backlog

### Week 4: Scale
- Add more RSS feeds
- Create content variations
- Build workflow #2 (Document Processing)
- Package as a service for clients

## Packaging This as a Service

**Offer to clients:**
- "LinkedIn Content Automation Package"
- Price: $500-1500/month
- Include: Setup, custom RSS feeds, brand voice tuning, first month management
- Upsells: Performance reporting, direct LinkedIn posting, multi-platform expansion

# AI Customer Support Automation - Setup Guide

## Overview
This n8n workflow transforms your Gmail into an intelligent customer support system. It automatically categorizes tickets, searches your knowledge base, generates AI responses, and either auto-sends simple replies or creates drafts for complex issues.

## What This Workflow Does

1. **Monitors Gmail** - Checks inbox every minute for new support emails
2. **Filters Valid Emails** - Ignores auto-replies and system messages
3. **Smart Categorization** - AI analyzes and categorizes each ticket (9 categories, 4 priority levels)
4. **Knowledge Base Search** - Finds relevant docs in Google Drive
5. **AI Response Generation** - Creates helpful, personalized responses
6. **Smart Routing** - Auto-sends simple answers, drafts complex ones
7. **Ticket Tracking** - Logs everything to Google Sheets with full details
8. **Sentiment Analysis** - Tracks customer mood and escalates angry customers

## Setup Instructions

### Step 1: Get Your Gemini API Key

1. Go to https://aistudio.google.com/app/apikey
2. Click "Get API Key" or "Create API Key"
3. Copy your API key (starts with "AIza...")

### Step 2: Create Your Google Sheets Ticket Tracker

1. Create a new Google Sheet
2. Name it "Support Tickets" (or your preference)
3. The workflow will automatically create all columns:
   - Ticket ID, Status, Received At
   - Customer Name, Customer Email, Subject
   - Category, Priority, Sentiment, Complexity
   - Summary, Key Points, Suggested Action
   - Response Text, Confidence Score
   - KB Docs Used, Auto Sent, Resolved At, Notes
4. Copy the Sheet ID from the URL

### Step 3: Set Up Knowledge Base in Google Drive

#### Create Support Documentation Folder:
1. In Google Drive, create a folder called "Support Knowledge Base"
2. Add documents with common answers:
   - **FAQ.doc** - Frequently asked questions
   - **Getting Started Guide.doc** - Onboarding instructions
   - **Technical Troubleshooting.doc** - Common technical issues
   - **Billing & Refunds.doc** - Payment policies
   - **Feature Documentation.doc** - How features work

#### Best Practices for Knowledge Base Docs:
- Use clear section headers
- Write in Q&A format when possible
- Include step-by-step instructions
- Keep language simple and conversational
- Update regularly based on common questions

**Example FAQ.doc Structure:**
```
FREQUENTLY ASKED QUESTIONS

Q: How do I reset my password?
A: Click "Forgot Password" on the login page, enter your email, and follow the instructions sent to your inbox.

Q: What payment methods do you accept?
A: We accept all major credit cards (Visa, Mastercard, Amex) and PayPal.

Q: How long is my free trial?
A: All new accounts get a 14-day free trial with full access to all features.
```

### Step 4: Import the Workflow

1. Open your n8n instance
2. Click "Add Workflow" or the "+" button
3. Click the three dots menu (⋮) → "Import from File"
4. Select `customer_support_automation.json`
5. The workflow will load with all nodes

### Step 5: Configure Credentials

#### Gemini API Credential
1. In n8n, go to Settings → Credentials
2. Click "Add Credential" → Search for "Query Auth"
3. Name it "Gemini API Key"
4. Set:
   - **Name**: `key`
   - **Value**: Your Gemini API key
5. Save

#### Gmail Credential
1. In the workflow, click on "Gmail Trigger" node
2. Click "Create New Credential"
3. Follow OAuth2 flow to connect your Gmail account
4. Authorize n8n to read and send emails
5. **Important**: This must be the support email inbox you want to monitor

#### Google Drive Credential
1. Click on "Search Knowledge Base" node
2. Click "Create New Credential"
3. Follow OAuth2 flow to connect your Google Drive
4. Authorize access to read files

#### Google Sheets Credential
1. Click on "Log to Support Tracker" node
2. Select or create Google Sheets credential
3. Select your Support Tickets sheet
4. Select Sheet1 (or your sheet name)

### Step 6: Configure the Workflow

#### Update Gmail Trigger (Optional)
- Default: Checks every minute
- To change: Click "Gmail Trigger" → Modify "Poll Times"
- Options: Every minute, every 5 minutes, every 10 minutes

#### Update Knowledge Base Search
1. Click "Search Knowledge Base" node
2. Update the query to match your folder structure:
   - Current: Searches for files with "support", "help", "faq", "documentation"
   - Add your specific folder name if needed

#### Customize Auto-Send Logic (Optional)
The workflow auto-sends when:
- Complexity = "Simple" AND
- AI Confidence > 75% AND
- Priority ≠ "Urgent"

To change, edit the "Parse Response & Decide Action" node JavaScript.

### Step 7: Test the Workflow

1. Activate the workflow (toggle switch to ON)
2. Send a test email to your support Gmail:
   - Subject: "How do I reset my password?"
   - Body: "Hi, I forgot my password and need help logging in."
3. Wait 1-2 minutes
4. Check:
   - Your Google Sheet for the logged ticket
   - Your Gmail for the auto-sent reply (if simple) OR
   - Your Gmail Drafts for the draft response (if complex)

## How It Works

### Email Categorization

**9 Categories:**
1. Technical Issue
2. Billing Question
3. Feature Request
4. General Inquiry
5. Complaint
6. Account Issue
7. Product Question
8. Bug Report
9. Other

**4 Priority Levels:**
- **Urgent**: System down, data loss, security, angry customer
- **High**: Feature broken, billing error, affects multiple customers
- **Medium**: Single customer issue, feature question
- **Low**: General question, feature request, feedback

**3 Complexity Levels:**
- **Simple**: Can be answered from knowledge base, no technical steps
- **Moderate**: Needs investigation or multi-step solution
- **Complex**: Needs specialist, custom solution, or development

**4 Sentiment Levels:**
- Very Negative (escalate immediately)
- Negative (prioritize response)
- Neutral (standard handling)
- Positive (quick acknowledgment)

### Auto-Send vs. Draft Logic

**Auto-Send Criteria** (all must be true):
1. Complexity = Simple
2. AI Confidence Score > 75%
3. Priority ≠ Urgent

**Draft for Review** (any of these):
1. Complexity = Moderate or Complex
2. AI Confidence Score ≤ 75%
3. Priority = Urgent
4. Sentiment = Very Negative

### Knowledge Base Integration

The workflow:
1. Searches Google Drive for relevant docs based on ticket category
2. Downloads up to 3 most relevant documents
3. Extracts text content (up to 3,000 chars per doc)
4. Feeds content to AI for context
5. AI generates response using this knowledge
6. Logs which docs were used in the ticket tracker

## Google Sheets Columns Explained

| Column | Purpose |
|--------|---------|
| **Ticket ID** | Unique identifier (TICKET_YYYYMMDD_ID) |
| **Status** | Auto-Responded or Pending Review |
| **Received At** | Timestamp when email arrived |
| **Customer Name** | Extracted from email sender |
| **Customer Email** | Sender's email address |
| **Subject** | Original email subject |
| **Category** | AI-assigned category (9 options) |
| **Priority** | Low / Medium / High / Urgent |
| **Sentiment** | Customer's emotional tone |
| **Complexity** | Simple / Moderate / Complex |
| **Summary** | One-sentence issue summary |
| **Key Points** | Main points from customer email |
| **Suggested Action** | AI recommendation for resolution |
| **Response Text** | Generated response (sent or drafted) |
| **Confidence Score** | AI's confidence (0-100) in response |
| **KB Docs Used** | Which knowledge base docs were referenced |
| **Auto Sent** | TRUE if auto-sent, FALSE if drafted |
| **Resolved At** | Manually update when ticket closed |
| **Notes** | Add internal team notes |

## Daily Support Workflow

### Morning Routine (5 minutes):
1. Open Support Tickets Google Sheet
2. Filter for **Status = "Pending Review"**
3. Review drafted responses
4. Edit if needed, then send from Gmail Drafts
5. Update **Status** to "Resolved" and add **Resolved At** timestamp

### Throughout the Day:
- Simple tickets auto-respond instantly
- Complex tickets appear in Drafts folder
- Monitor Sheet for Urgent/High priority tickets
- Check for Very Negative sentiment tickets

### Weekly Review:
- Analyze which categories are most common
- Review low-confidence responses for quality
- Update knowledge base docs based on gaps
- Track average resolution time by category

## Advanced Customization

### Add Email Templates by Category

Edit the "Gemini - Generate Response" prompt to include category-specific instructions:

```
If category is "Billing Question":
- Always mention our 30-day refund policy
- Include link to billing portal: https://yoursite.com/billing
- Offer to schedule a call if amount > $500

If category is "Technical Issue":
- Ask for browser, OS, and error messages
- Provide troubleshooting steps
- Offer to escalate to engineering if needed
```

### Add Slack Notifications for Urgent Tickets

Add a Slack node after "Parse Response & Decide Action":
1. Filter for Priority = "Urgent"
2. Send message to #support-urgent channel
3. Include customer name, issue summary, and ticket ID

### Auto-Escalation for Angry Customers

Add an IF node after sentiment analysis:
1. If sentiment = "Very Negative"
2. Send Slack alert to support manager
3. Create high-priority Asana task
4. Auto-CC manager on response email

### Multi-Language Support

Add a language detection node before response generation:
1. Detect customer's language
2. Pass to Gemini with instruction: "Respond in [detected language]"
3. Works for Spanish, French, German, etc.

### Integration with CRM

Replace or supplement Google Sheets with:
- **HubSpot**: Log tickets as support conversations
- **Salesforce**: Create cases automatically
- **Zendesk**: Sync tickets bidirectionally
- **Freshdesk**: Create and update tickets

## Cost Estimation

**For 500 tickets per month:**
- Gemini API: ~$2-5/month (2-3 calls per ticket)
- Gmail: Free
- Google Drive: Free (or existing workspace)
- Google Sheets: Free
- n8n: Free (self-hosted) or $20/month (cloud)

**Total**: ~$22-30/month for automated support

**Savings**: 
- Manual support: 15-20 hours/week (worth $2,000-4,000/month)
- ROI: ~7,000-15,000% in the first month

## Troubleshooting

### Emails not being processed
- Check Gmail Trigger is activated
- Verify OAuth connection is valid (re-authenticate if needed)
- Check filter criteria in "Filter Valid Support Emails"
- Look for emails with "noreply" or "auto-reply" (these are filtered out)

### AI responses are generic or wrong
- Update knowledge base docs with more detailed info
- Edit the response generation prompt for more specificity
- Increase knowledge base search results (edit Drive node)
- Lower auto-send confidence threshold temporarily

### Too many drafts, not enough auto-sends
- Lower confidence threshold in "Parse Response & Decide Action" node
- Review what's being marked as "Moderate" complexity
- Improve knowledge base documentation
- Adjust auto-send criteria

### Knowledge base not being used
- Verify Google Drive search query finds your docs
- Check file permissions (ensure n8n can access)
- Review search terms in "Search Knowledge Base" node
- Add more relevant keywords to your docs

### Duplicate responses sent
- Check if Gmail Trigger polling is too frequent
- Add message ID tracking to prevent duplicates
- Review Gmail label filters

## Best Practices

### Knowledge Base Maintenance
- Review unanswered questions weekly
- Add new docs for recurring issues
- Keep docs under 5 pages each (better for AI processing)
- Use simple language and bullet points
- Include examples and screenshots

### Response Quality
- Review auto-sent emails daily for first 2 weeks
- Create a "Response Quality Checklist"
- A/B test different prompt approaches
- Collect customer feedback on responses

### Team Workflow
- Assign ownership for each ticket category
- Set SLA targets (e.g., respond within 2 hours)
- Weekly team review of complex tickets
- Monthly analysis of common issues

### Continuous Improvement
- Track metrics: response time, resolution rate, customer satisfaction
- Identify knowledge gaps from low-confidence tickets
- Refine AI prompts based on feedback
- Expand knowledge base monthly

## Packaging This as a Service

**Offer to clients:**
- "AI Customer Support Automation Package"
- Price: $1,000-3,000/month
- Include: Setup, knowledge base creation, Gmail integration, 30-day optimization period
- Upsells: Slack integration, CRM sync, multi-language support, custom categorization

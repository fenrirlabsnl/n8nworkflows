# B2B Lead Enrichment: Attio CRM Template

An AI-powered B2B lead enrichment workflow for n8n that automatically enriches company and contact records in Attio CRM using Apollo, LinkedIn, and news data.

## Overview

This workflow takes a company domain and performs comprehensive enrichment:

1. **Company Enrichment** - Fetches company data from Apollo (name, industry, size, socials)
2. **News Analysis** - Retrieves and summarizes recent news articles about the company
3. **LinkedIn Enrichment** - Scrapes company LinkedIn page and recent posts
4. **Leadership Discovery** - Uses AI to identify leadership team members
5. **Contact Enrichment** - Enriches individual contacts with email and LinkedIn data
6. **AI Synthesis** - Produces conversation-ready dossier with positioning, talking points, and leadership insights

### Key Features

- Multi-source data aggregation (Apollo, LinkedIn, Tavily)
- AI-powered leadership team discovery with web search
- LLM-as-Judge verification layer for accuracy
- Automatic Attio CRM record creation/update
- Graceful error handling for missing data

## Prerequisites

Before setting up this workflow, you'll need:

- **n8n** - Self-hosted or cloud instance
- **Apollo.io** - API access for company/contact enrichment
- **Scrape Creators** - API for LinkedIn scraping (n8n community node)
- **Tavily** - API for web content extraction
- **OpenAI** - API key with GPT-4o access
- **Attio** - CRM workspace with API access

### Required Community Nodes

Install these community nodes in n8n before importing:

- `n8n-nodes-scrape-creators` - LinkedIn scraping
- `@tavily/n8n-nodes-tavily` - Web extraction
- `n8n-nodes-attio` - Attio CRM integration

## Quick Start

1. Install required community nodes in n8n
2. Import `workflow.json` into n8n
3. Set up your credentials in n8n
4. Replace placeholder values with your credential IDs
5. Activate the workflow
6. Trigger manually or integrate with your lead source

## Required Credentials

### Apollo.io API Setup

1. Log in to [Apollo.io](https://app.apollo.io)
2. Go to Settings > Integrations > API
3. Generate a new API key
4. In n8n, create an "Header Auth" credential:
   - Name: `X-Api-Key`
   - Value: Your Apollo API key

### Scrape Creators API Setup

1. Sign up at [Scrape Creators](https://scrapecreators.com)
2. Get your API key from the dashboard
3. Add as a Scrape Creators credential in n8n

### Tavily API Setup

1. Sign up at [Tavily](https://tavily.com)
2. Get your API key from the dashboard
3. Add as a Tavily credential in n8n

### OpenAI API Setup

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Create a new API key
3. Add as an OpenAI credential in n8n

### Attio API Setup

1. Log in to [Attio](https://app.attio.com)
2. Go to Settings > Developers > API Keys
3. Create a new API key with read/write access to:
   - Companies object
   - People object
4. Add as an Attio credential in n8n

## Configuration Values

After importing the workflow, search and replace these placeholders:

### Credential IDs

| Placeholder | Description | How to Find |
|-------------|-------------|-------------|
| `{{APOLLO_API_CREDENTIAL_ID}}` | Apollo HTTP Header Auth credential ID | n8n > Credentials > Apollo API > URL contains the ID |
| `{{SCRAPE_CREATORS_CREDENTIAL_ID}}` | Scrape Creators API credential ID | n8n > Credentials > Scrape Creators > URL contains the ID |
| `{{TAVILY_API_CREDENTIAL_ID}}` | Tavily API credential ID | n8n > Credentials > Tavily > URL contains the ID |
| `{{OPENAI_API_CREDENTIAL_ID}}` | OpenAI API credential ID | n8n > Credentials > OpenAI > URL contains the ID |
| `{{ATTIO_API_CREDENTIAL_ID}}` | Attio API credential ID | n8n > Credentials > Attio > URL contains the ID |

**Tip**: To find a credential ID in n8n:
1. Go to Credentials in your n8n instance
2. Click on the credential
3. The ID is in the URL: `https://your-n8n.com/credentials/ABC123` where `ABC123` is the ID

## Attio Custom Fields

This workflow expects certain custom fields on your Attio Companies object:

| Field Name | Type | Description |
|------------|------|-------------|
| `positioning` | Text | AI-generated company positioning summary |
| `conversation_points` | Text | Key talking points for sales conversations |
| `leadership_conversations` | Text | Summary of leadership team communications |
| `enrichment_status` | Select | Status of enrichment (complete/pending) |

Create these fields in Attio before running the workflow, or modify the final Code node to match your schema.

## Workflow Architecture

```
Input: Company Domain
        │
        ▼
[Apollo: Enrich Company] ──► [Company Data Exists?]
                                      │
                                     Yes
                                      │
        ┌─────────────────────────────┼─────────────────────────────┐
        │                             │                             │
        ▼                             ▼                             ▼
[Apollo: Latest News]    [Leadership Search Agent]    [LinkedIn Company Page]
        │                             │                             │
        ▼                             ▼                             ▼
[Tavily: Extract]              [Apollo: People]             [Split Posts]
        │                             │                             │
        ▼                             ▼                             ▼
[Apollo Summary Agent]    [LinkedIn Leadership Summary]    [LinkedIn Summary Agent]
        │                             │                             │
        └─────────────────────────────┼─────────────────────────────┘
                                      │
                                      ▼
                              [Merge All Research]
                                      │
                                      ▼
                              [LLM Critic - Verify]
                                      │
                                      ▼
                        [Final AI Agent - Synthesize]
                                      │
                                      ▼
                          [Update Attio Company]
```

## Usage

### Manual Trigger

The workflow starts with a manual trigger. To customize input:

1. Replace the manual trigger with your preferred trigger (webhook, schedule, etc.)
2. Ensure the trigger passes a `domain` field to the first HTTP node

### Example Input

```json
{
  "domain": "example.com"
}
```

### Expected Output

The workflow enriches Attio with:

**Company Record:**
- Name, description, LinkedIn URL
- AI-generated positioning summary
- Key conversation points
- Leadership conversation insights

**People Records:**
- Leadership team members
- Job titles
- Email addresses (when available)
- LinkedIn profiles

## Customization

### Adjusting AI Models

The workflow uses GPT-4o throughout. To change models:
1. Find the "OpenAI Chat Model" nodes
2. Update the `model` parameter to your preferred model

### Modifying Enrichment Sources

- **Add more news sources**: Extend the Tavily extraction step
- **Different scraping service**: Replace Scrape Creators nodes
- **Alternative CRM**: Modify the Attio nodes to your CRM's API

### Adjusting Leadership Search

The Leadership Search Agent uses web search to find current executives. Modify the system prompt in the "Leadership Search Agent" node to adjust:
- Roles to search for (CEO, CTO, VP Sales, etc.)
- Recency requirements
- Company validation rules

## Troubleshooting

### Apollo Returns No Data

- Verify your API key has sufficient credits
- Check if the domain exists in Apollo's database
- Try with a well-known company domain first

### LinkedIn Scraping Fails

- Verify Scrape Creators API key is valid
- Check if the LinkedIn URL is correctly formatted (https://)
- The company may have a private LinkedIn page

### Attio Update Fails

- Ensure custom fields exist with exact names
- Verify API key has write permissions
- Check that the domain matching is finding the correct record

### AI Agents Timeout

- Large companies with many news articles may take longer
- Consider reducing the "Last 5 Articles" limit
- Check OpenAI API rate limits

## License

MIT License - Feel free to use and modify this workflow for your needs.

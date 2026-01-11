# Second Brain: n8n Workflow System

An AI-powered personal knowledge management system built with n8n. Capture thoughts via Slack, automatically classify them with GPT-4, and store everything in Notion.

## What It Does

This system consists of three workflows that work together:

| Workflow | Purpose | Schedule |
|----------|---------|----------|
| **Entry & Classification** | Captures Slack messages, uses AI to classify into categories (People, Projects, Ideas, Admin), stores in Notion | Real-time (on message) |
| **Daily Digest** | Sends a morning summary of priorities, follow-ups, and due tasks | Daily at 8am |
| **Weekly Review** | Comprehensive analysis of captures, progress, and patterns | Weekly at 4pm |

## Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- Slack workspace
- Notion workspace
- OpenAI API key (GPT-4 access)

## Setup Instructions

### 1. Slack App Setup

You need to create a Slack App to allow n8n to read messages and send responses.

1. Follow the [n8n Slack credentials guide](https://docs.n8n.io/integrations/builtin/credentials/slack/)
2. Create a new Slack App at [api.slack.com/apps](https://api.slack.com/apps)
3. Add these **Bot Token OAuth Scopes**:
   - `channels:history` - Read messages in public channels
   - `channels:read` - View basic channel info
   - `chat:write` - Send messages
   - `users:read` - View user info
   - `im:write` - Send direct messages (for digests)
4. Install the app to your workspace
5. Copy the **Bot User OAuth Token** (starts with `xoxb-`)
6. Add as a Slack credential in n8n

### 2. Notion Integration Setup

You need to create a Notion integration and grant it access to your databases.

1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click **New integration**
3. Give it a name (e.g., "Second Brain n8n")
4. Select your workspace
5. Copy the **Internal Integration Token** (starts with `secret_`)
6. Add as a Notion credential in n8n

**Important: Enable the integration on every database**

For each of your 5 databases (Ideas, People, Projects, Admin, Inbox Log):
1. Open the database in Notion
2. Click **...** (menu) in the top right
3. Go to **Connections**
4. Find and select your integration
5. Click **Confirm**

Without this step, n8n cannot read or write to your databases.

### 3. OpenAI Setup

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Create a new API key
3. Add as an OpenAI credential in n8n

### 4. Import Workflows

1. Import each JSON file into n8n
2. Connect your credentials to each node
3. Update placeholder values (see below)

## Placeholder Values

After importing, search and replace these placeholders in each workflow:

### Slack Placeholders

| Placeholder | Description | How to Find |
|-------------|-------------|-------------|
| `{{YOUR_SLACK_CHANNEL_ID}}` | Channel to monitor for captures | Right-click channel > Copy link > Extract ID (e.g., `C01ABC23DEF`) |
| `{{YOUR_SLACK_USER_ID}}` | Your Slack user ID (for DMs) | Profile > ... > Copy member ID |
| `{{YOUR_SLACK_USERNAME}}` | Display name | Your Slack username |
| `{{YOUR_CHANNEL_NAME}}` | Channel display name | The name of your inbox channel |

### Notion Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{{NOTION_IDEAS_DB_ID}}` | Ideas database ID |
| `{{NOTION_IDEAS_URL}}` | Ideas database URL |
| `{{NOTION_PEOPLE_DB_ID}}` | People database ID |
| `{{NOTION_PEOPLE_URL}}` | People database URL |
| `{{NOTION_PROJECTS_DB_ID}}` | Projects database ID |
| `{{NOTION_PROJECTS_URL}}` | Projects database URL |
| `{{NOTION_ADMIN_DB_ID}}` | Admin/Tasks database ID |
| `{{NOTION_ADMIN_URL}}` | Admin database URL |
| `{{NOTION_INBOX_LOG_DB_ID}}` | Inbox Log database ID |
| `{{NOTION_INBOX_LOG_URL}}` | Inbox Log database URL |

**Finding Notion Database IDs:**
1. Open the database in Notion
2. Click **Share** > **Copy link**
3. The ID is the 32-character string in the URL:
   ```
   https://www.notion.so/myworkspace/2e3f6d1ea16380d9b464f0e97cde2833?v=...
                                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                     This is your database ID
   ```

## Required Notion Database Schemas

Each database needs specific properties. See the workflow JSON files for exact property names, or refer to the templates folder for detailed schemas.

## Usage

Once configured:

1. **Capture thoughts**: Send messages to your Slack inbox channel
2. **Receive confirmations**: Bot replies with classification result
3. **Correct mistakes**: Reply with `fix: [category]` to reclassify
4. **Morning briefing**: Receive daily digest at 8am
5. **Weekly reflection**: Receive weekly review summary

## License

MIT

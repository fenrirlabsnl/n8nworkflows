# n8nworkflows

A collection of n8n workflows for everyone to enjoy.

## Workflows

### [Second Brain](NateSecondBrain/)

An AI-powered personal knowledge management system that captures thoughts via Slack and automatically classifies them into Notion databases using GPT-4. The system includes three workflows: Entry & Classification (real-time message capture and AI categorization into People, Projects, Ideas, or Admin), Daily Digest (morning summary of priorities and due tasks), and Weekly Review (comprehensive analysis of captures and patterns). Includes a manual correction flow via Slack threads and confidence-based routing for uncertain classifications.

### [B2B Lead Enrichment: Attio CRM](AttioB2BLeadEnrichment/)

A comprehensive B2B lead enrichment pipeline that takes a company domain and produces a sales-ready dossier in Attio CRM. The workflow aggregates data from Apollo.io (company info, contacts, news), LinkedIn (company page and posts via Scrape Creators), and Tavily (article extraction), then uses a multi-agent AI system with GPT-4o to synthesize positioning summaries, conversation talking points, and leadership insights. Features AI-powered leadership team discovery with web search and an LLM-as-Judge verification layer to fact-check outputs before writing to CRM.

### [WhatsApp Booking Flow](WhatsAppBookingFlow/)

An automated appointment booking system via WhatsApp Flows with real-time calendar availability. Users can book consultations directly within WhatsApp through an interactive multi-step flow that collects service type, customer details, and preferred time slots. Features AI-powered intent classification (GPT-4o) to detect booking requests from regular messages, end-to-end encrypted WhatsApp Flow handling, Google Calendar integration for availability checks, and Airtable CRM sync for customer and booking records.

## Getting Started

Each workflow folder contains:
- Workflow JSON files - Import directly into n8n
- `README.md` - Setup instructions, prerequisites, and configuration guide

Browse the folders above to find workflows for your use case.

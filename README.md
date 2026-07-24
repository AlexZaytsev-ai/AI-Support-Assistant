# AI Support Assistant

AI-powered customer support automation workflow built with n8n.

## Overview

This workflow automatically analyzes incoming customer requests, classifies them using AI, stores every request in Google Sheets, and routes notifications to the appropriate department.

## Features

- Receive customer requests from Telegram
- AI classification using OpenAI
- Structured JSON output
- Automatic department routing
- Priority detection
- Order number extraction
- Google Sheets logging
- Telegram notifications
- Manual review for unknown requests

## Tech Stack

- n8n
- OpenAI
- Telegram Bot API
- Google Sheets

## Workflow

```
Telegram
      │
      ▼
Normalize Request
      │
      ▼
AI Classification
      │
      ▼
Prepare Response
      ├──────────────► Google Sheets
      │
      ▼
Department Router
      ├── Support
      ├── Sales
      ├── Finance
      └── Manual Review
```

## Repository Structure

```
workflow/
└── AI Support Assistant — TechStore.json
```

## Author

Alexander Zaytsev

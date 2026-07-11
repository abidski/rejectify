# rejectify

A self-hosted job application tracker that automatically classifies and extracts data from job application related emails using  LLM.

## Why

Maually inputing your job applications when applying takes alot of time and gets messy when there are many of applications. This solution provides for a cleaner approach.

## Features

- **Automated email ingestion** — parses through email inbox using IMAP
- **LLM classification** — uses Groq LLM's to classify and extract email (wether the email is a rejection or an offer...)
- **PostgreSQL persistence** — uses PostgreSQL to store data and info regarding applications

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | FastAPI |
| ORM | SQLAlchemy 2.0 |
| Database | PostgreSQL |
| Email classification | [Groq](https://console.groq.com/home?utm_source=website&utm_medium=outbound_link&utm_campaign=dev_console_click) llama-3.3-70b-versatile |
| Email ingestion | IMAP |
| Hosting | Self-hosted (Linux) |

## Architecture

```
IMAP inbox
    │
    ▼
Email poller/extraction ──> LLM classifier (llama-3.3-70b-versatile)
    │                    │
    │                    ▼
    │           Email type (confirmation / rejection / interview / other)
    │           Application infor (company, role, date)
    │
    ▼
PostgreSQL ──> To be built [FastAPI ──> Client]
```

Uses IMAP to access inbox and uses [Groq](https://console.groq.com/home?utm_source=website&utm_medium=outbound_link&utm_campaign=dev_console_click) to extract emails and parse data regarding applications before writing data to Postgres.  Use [Groq](https://console.groq.com/home?utm_source=website&utm_medium=outbound_link&utm_campaign=dev_console_click) to gain acces to top of the line LLM's to classify emails.

## How it works

### Prerequisites

- Python 3.11+
- PostgreSQL 15+
- Obtain a free Groq API key
- Enable IMAP accesability via [Gmail](https://developers.google.com/workspace/gmail/imap/imap-smtp) with an App Password

### Installation

Get acces to email inbox by enabaling IMAP via [Gmail](https://developers.google.com/workspace/gmail/imap/imap-smtp) and obtain an app password.

```bash
git clone https://github.com/<your-username>/rejectify.git
cd rejectify
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Configuration

Create a `.env` file:

```env
EMAIL_ADDRESS=...
EMAIL_PASSWORD=...
SERVER=imap.gmail.com
GROQ_API_KEY=...
DBNAME=...
DBUSER=...
DBPASSWORD=...
DBHOST=...
DBPORT=...

```

### Run

Make sure DB is running before-hand.
```bash
python main.py
```

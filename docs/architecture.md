# Platform Overview

Ledgyx is a **no-code / low-code operational platform** that lets you build APIs, automate workflows, manage data, and deploy AI agents — all from a single admin panel.

## How it works

At the core of Ledgyx is the idea that every HTTP endpoint, data operation, and automation is powered by a **named event** — a SQL-based handler you write once and connect to anything: a REST endpoint, a webhook, a subscription trigger, or an AI agent.

```
You define →  Events (SQL handlers)
              Entities (data models)
              Templates (HTML pages)
              
Platform handles → HTTP routing, data storage, AI execution, webhooks
```

## Core concepts

### Configuration
Every resource in Ledgyx belongs to a **configuration** — a tenant workspace. You can have multiple configurations (e.g. one per client or environment) and switch between them instantly. Everything you see in the admin panel — events, entities, API endpoints, agents — belongs to the currently active configuration.

### Events
An Event is a named SQL handler with up to 4 methods (GET, POST, PUT, DELETE). When an HTTP request hits your API, Ledgyx routes it to the matching event and executes the SQL. Events can also call other services using built-in CALL providers:

| Provider | What it does |
|---|---|
| `AGENT` | Runs an AI agent asynchronously |
| `AI_CHAT_COMPLETION` | Calls an LLM directly and gets a structured response |
| `TG_SEND_MESSAGE` | Sends a Telegram message |
| `TG_GET_FILE` | Downloads a file from Telegram and processes it with AI |
| `WEBHOOK` | Calls an external HTTP service |
| `SSE_SEND` | Pushes a real-time update to the browser |

### Entities
Entities are your data model — the objects your platform stores and manages. Think of them like typed database tables with a schema you control. You define entity types (Dictionary, Enum, Const, etc.) and their fields with strong types (String, Number, Boolean, Date, JSON, and more).

### Templates
HTML pages rendered by the platform use Mustache templates. You can build entire websites — landing pages, dashboards, e-commerce stores — where each URL route maps to a template and an event that provides the data.

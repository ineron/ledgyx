# Configurations & Multi-tenancy

## What is a configuration?

A **configuration** is your isolated workspace in Ledgyx. Everything you create — events, entities, API endpoints, templates, sitemap routes, webhooks, subscriptions, and agent graphs — belongs to exactly one configuration.

You can create multiple configurations for:
- Different projects or products
- Different clients (agency/SaaS use case)
- Staging vs. production environments

## Switching configurations

The active configuration is shown in the header at all times. Click the badge to go to Settings and switch.

When you switch configurations, all data in the admin panel refreshes to show the new workspace. Query caches are cleared automatically.

## Your base domain

Each configuration has a **base domain** automatically assigned:

```
{config-id}.ledgyx.com
```

This domain serves your Mustache-rendered HTML pages (see [Sitemap](pages/sitemap.md) and [Templates](pages/templates.md)).

You can also add **custom domains** (aliases) under Settings → DNS/Domain. The platform handles SSL certificate issuance automatically once you add the required DNS TXT record.

## API access

Your configuration's REST API is available at:

```
https://app.ledgyx.com/rest/v2/{api-key}/{group}/{endpoint}
```

The `{api-key}` is found in **Settings → API Keys**. Each key can be scoped and revoked independently.

## Settings tabs overview

| Tab | What it controls |
|---|---|
| **Configurations** | Create, activate, and manage workspaces |
| **API Keys** | UUID tokens that external callers use to access your API |
| **Security** | Access control groups |
| **DNS / Domain** | Base domain, custom aliases, SSL verification |
| **AI Credentials** | API keys for OpenAI, Anthropic, Mistral, and other LLM providers |
| **AI Settings** | Prompt templates and JSON output schemas for LLM calls |
| **SSE Tokens** | Server-Sent Events authentication tokens |
| **Telegram** | Bot accounts for notifications and file delivery |
| **Password** | Change your login password |
| **Addons** | Stripe subscription plans |

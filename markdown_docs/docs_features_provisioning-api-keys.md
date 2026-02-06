Search

`/`

Ask AI

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)

* Overview

  + [Quickstart](/docs/quickstart)
  + [FAQ](/docs/faq)
  + [Principles](/docs/overview/principles)
  + [Models](/docs/overview/models)
  + [Enterprise](https://openrouter.ai/enterprise)
* Features

  + [Privacy and Logging](/docs/features/privacy-and-logging)
  + [Zero Data Retention (ZDR)](/docs/features/zdr)
  + [Model Routing](/docs/features/model-routing)
  + [Provider Routing](/docs/features/provider-routing)
  + [Exacto Variant](/docs/features/exacto-variant)
  + [Latency and Performance](/docs/features/latency-and-performance)
  + [Presets](/docs/features/presets)
  + [Prompt Caching](/docs/features/prompt-caching)
  + [Structured Outputs](/docs/features/structured-outputs)
  + [Tool Calling](/docs/features/tool-calling)
  + Multimodal
  + [Message Transforms](/docs/features/message-transforms)
  + [Uptime Optimization](/docs/features/uptime-optimization)
  + [Web Search](/docs/features/web-search)
  + [Zero Completion Insurance](/docs/features/zero-completion-insurance)
  + [Provisioning API Keys](/docs/features/provisioning-api-keys)
  + [App Attribution](/docs/app-attribution)
* API Reference

  + [Overview](/docs/api-reference/overview)
  + [Streaming](/docs/api-reference/streaming)
  + [Embeddings](/docs/api-reference/embeddings)
  + [Limits](/docs/api-reference/limits)
  + [Authentication](/docs/api-reference/authentication)
  + [Parameters](/docs/api-reference/parameters)
  + [Errors](/docs/api-reference/errors)
  + Responses API
  + beta.responses
  + Analytics
  + Credits
  + Embeddings
  + Generations
  + Models
  + Endpoints
  + Parameters
  + Providers
  + API Keys
  + O Auth
  + Chat
  + Completions
* SDK Reference (BETA)

  + [Python SDK](/docs/sdks/python)
  + [TypeScript SDK](/docs/sdks/typescript)
* Use Cases

  + [BYOK](/docs/use-cases/byok)
  + [Crypto API](/docs/use-cases/crypto-api)
  + [OAuth PKCE](/docs/use-cases/oauth-pkce)
  + [MCP Servers](/docs/use-cases/mcp-servers)
  + [Organization Management](/docs/use-cases/organization-management)
  + [For Providers](/docs/use-cases/for-providers)
  + [Reasoning Tokens](/docs/use-cases/reasoning-tokens)
  + [Usage Accounting](/docs/use-cases/usage-accounting)
  + [User Tracking](/docs/use-cases/user-tracking)
* Community

  + [Frameworks and Integrations Overview](/docs/community/frameworks-and-integrations-overview)
  + [Effect AI SDK](/docs/community/effect-ai-sdk)
  + [Arize](/docs/community/arize)
  + [LangChain](/docs/community/lang-chain)
  + [LiveKit](/docs/community/live-kit)
  + [Langfuse](/docs/community/langfuse)
  + [Mastra](/docs/community/mastra)
  + [OpenAI SDK](/docs/community/open-ai-sdk)
  + [PydanticAI](/docs/community/pydantic-ai)
  + [Vercel AI SDK](/docs/community/vercel-ai-sdk)
  + [Xcode](/docs/community/xcode)
  + [Zapier](/docs/community/zapier)
  + [Discord](https://discord.gg/openrouter)

Light

On this page

* [Creating a Provisioning API Key](#creating-a-provisioning-api-key)
* [Use Cases](#use-cases)
* [Example Usage](#example-usage)
* [Response Format](#response-format)

[Features](/docs/features/privacy-and-logging)

# Provisioning API Keys

Copy page

Manage API keys programmatically

OpenRouter provides endpoints to programmatically manage your API keys, enabling key creation and management for applications that need to distribute or rotate keys automatically.

## Creating a Provisioning API Key

To use the key management API, you first need to create a Provisioning API key:

1. Go to the [Provisioning API Keys page](https://openrouter.ai/settings/provisioning-keys)
2. Click “Create New Key”
3. Complete the key creation process

Provisioning keys cannot be used to make API calls to OpenRouter’s completion endpoints - they are exclusively for key management operations.

## Use Cases

Common scenarios for programmatic key management include:

* **SaaS Applications**: Automatically create unique API keys for each customer instance
* **Key Rotation**: Regularly rotate API keys for security compliance
* **Usage Monitoring**: Track key usage and automatically disable keys that exceed limits (with optional daily/weekly/monthly limit resets)

## Example Usage

All key management endpoints are under `/api/v1/keys` and require a Provisioning API key in the Authorization header.

TypeScript SDKPythonTypeScript (fetch)

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: 'your-provisioning-key', // Use your Provisioning API key |
| 5 | }); |
| 6 |  |
| 7 | // List the most recent 100 API keys |
| 8 | const keys = await openRouter.apiKeys.list(); |
| 9 |  |
| 10 | // You can paginate using the offset parameter |
| 11 | const keysPage2 = await openRouter.apiKeys.list({ offset: 100 }); |
| 12 |  |
| 13 | // Create a new API key |
| 14 | const newKey = await openRouter.apiKeys.create({ |
| 15 | name: 'Customer Instance Key', |
| 16 | limit: 1000, // Optional credit limit |
| 17 | }); |
| 18 |  |
| 19 | // Get a specific key |
| 20 | const keyHash = '<YOUR_KEY_HASH>'; |
| 21 | const key = await openRouter.apiKeys.get(keyHash); |
| 22 |  |
| 23 | // Update a key |
| 24 | const updatedKey = await openRouter.apiKeys.update(keyHash, { |
| 25 | name: 'Updated Key Name', |
| 26 | disabled: true, // Optional: Disable the key |
| 27 | includeByokInLimit: false, // Optional: control BYOK usage in limit |
| 28 | limitReset: 'daily', // Optional: reset limit every day at midnight UTC |
| 29 | }); |
| 30 |  |
| 31 | // Delete a key |
| 32 | await openRouter.apiKeys.delete(keyHash); |
```

## Response Format

API responses return JSON objects containing key information:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "data": [ |
| 3 | { |
| 4 | "created_at": "2025-02-19T20:52:27.363244+00:00", |
| 5 | "updated_at": "2025-02-19T21:24:11.708154+00:00", |
| 6 | "hash": "<YOUR_KEY_HASH>", |
| 7 | "label": "sk-or-v1-abc...123", |
| 8 | "name": "Customer Key", |
| 9 | "disabled": false, |
| 10 | "limit": 10, |
| 11 | "limit_remaining": 10, |
| 12 | "limit_reset": null, |
| 13 | "include_byok_in_limit": false, |
| 14 | "usage": 0, |
| 15 | "usage_daily": 0, |
| 16 | "usage_weekly": 0, |
| 17 | "usage_monthly": 0, |
| 18 | "byok_usage": 0, |
| 19 | "byok_usage_daily": 0, |
| 20 | "byok_usage_weekly": 0, |
| 21 | "byok_usage_monthly": 0 |
| 22 | } |
| 23 | ] |
| 24 | } |
```

When creating a new key, the response will include the key string itself. Read more in the [API reference](/docs/api-reference/api-keys/create-api-key).

Was this page helpful?

YesNo

[Previous](/docs/features/zero-completion-insurance)[#### App Attribution

Get your app featured in OpenRouter rankings and analytics

Next](/docs/app-attribution)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
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

* [Rate Limits and Credits Remaining](#rate-limits-and-credits-remaining)

[API Reference](/docs/api-reference/overview)

# Limits

Copy page

Rate Limits

#####

Making additional accounts or API keys will not affect your rate limits, as we
govern capacity globally. We do however have different rate limits for
different models, so you can share the load that way if you do run into
issues.

## Rate Limits and Credits Remaining

To check the rate limit or credits left on an API key, make a GET request to `https://openrouter.ai/api/v1/key`.

TypeScript SDKPythonTypeScript (Raw API)

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | const keyInfo = await openRouter.apiKeys.getCurrent(); |
| 8 | console.log(keyInfo); |
```

If you submit a valid API key, you should get a response of the form:

TypeScript

```
|  |  |
| --- | --- |
| 1 | type Key = { |
| 2 | data: { |
| 3 | label: string; |
| 4 | limit: number | null; // Credit limit for the key, or null if unlimited |
| 5 | limit_reset: string | null; // Type of limit reset for the key, or null if never resets |
| 6 | limit_remaining: number | null; // Remaining credits for the key, or null if unlimited |
| 7 | include_byok_in_limit: boolean;  // Whether to include external BYOK usage in the credit limit |
| 8 |  |
| 9 | usage: number; // Number of credits used (all time) |
| 10 | usage_daily: number; // Number of credits used (current UTC day) |
| 11 | usage_weekly: number; // ... (current UTC week, starting Monday) |
| 12 | usage_monthly: number; // ... (current UTC month) |
| 13 |  |
| 14 | byok_usage: number; // Same for external BYOK usage |
| 15 | byok_usage_daily: number; |
| 16 | byok_usage_weekly: number; |
| 17 | byok_usage_monthly: number; |
| 18 |  |
| 19 | is_free_tier: boolean; // Whether the user has paid for credits before |
| 20 | // rate_limit: { ... } // A deprecated object in the response, safe to ignore |
| 21 | }; |
| 22 | }; |
```

There are a few rate limits that apply to certain types of requests, regardless of account status:

1. Free usage limits: If you’re using a free model variant (with an ID ending in `:free`), you can make up to 20 requests per minute. The following per-day limits apply:

* If you have purchased less than 10 credits, you’re limited to 50 `:free` model requests per day.
* If you purchase at least 10 credits, your daily limit is increased to 1000 `:free` model requests per day.

2. **DDoS protection**: Cloudflare’s DDoS protection will block requests that dramatically exceed reasonable usage.

If your account has a negative credit balance, you may see `402` errors, including for free models. Adding credits to put your balance above zero allows you to use those models again.

Was this page helpful?

YesNo

[Previous](/docs/api-reference/embeddings)[#### Authentication

API Authentication

Next](/docs/api-reference/authentication)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
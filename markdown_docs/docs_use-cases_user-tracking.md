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

* [What is User Tracking?](#what-is-user-tracking)
* [How It Works](#how-it-works)
* [Benefits](#benefits)
* [Improved Caching Performance](#improved-caching-performance)
* [Enhanced Reporting and Analytics](#enhanced-reporting-and-analytics)
* [Implementation Example](#implementation-example)
* [Best Practices](#best-practices)
* [Choose Stable Identifiers](#choose-stable-identifiers)
* [Consider Privacy](#consider-privacy)
* [Be Consistent](#be-consistent)

[Use Cases](/docs/use-cases/byok)

# User Tracking

Copy page

The OpenRouter API supports **User Tracking** through the optional `user` parameter, allowing you to track your own user IDs and improve your application’s performance and reporting capabilities.

## What is User Tracking?

User tracking enables you to specify an arbitrary string identifier for your end-users in API requests. This optional metadata helps OpenRouter understand your sub-users, leading to several benefits:

1. **Improved Caching**: OpenRouter can make caches sticky to your individual users, improving load-balancing and throughput
2. **Enhanced Reporting**: View detailed analytics and activity feeds broken down by your user IDs

## How It Works

Simply include a `user` parameter in your API requests with any string identifier that represents your end-user. This could be a user ID, email hash, session identifier, or any other stable identifier you use in your application.

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4o", |
| 3 | "messages": [ |
| 4 | {"role": "user", "content": "Hello, how are you?"} |
| 5 | ], |
| 6 | "user": "user_12345" |
| 7 | } |
```

## Benefits

### Improved Caching Performance

When you consistently use the same user identifier for a specific user, OpenRouter can optimize caching to be “sticky” to that user. This means:

* A given user of your application (assuming you are using caching) will always get routed to the same provider and the cache will stay warm
* But separate users can be spread over different providers, improving load-balancing and throughput

### Enhanced Reporting and Analytics

The user parameter is available in the /activity page, in the exports from that page, and in the /generations API.

* **Activity Feed**: View requests broken down by user ID in your OpenRouter dashboard
* **Usage Analytics**: Understand which users are making the most requests
* **Export Data**: Get detailed exports that include user-level breakdowns

## Implementation Example

TypeScript SDKPython (OpenAI SDK)TypeScript (OpenAI SDK)

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | const response = await openRouter.chat.send({ |
| 8 | model: '{{MODEL}}', |
| 9 | messages: [ |
| 10 | { |
| 11 | role: 'user', |
| 12 | content: "What's the weather like today?", |
| 13 | }, |
| 14 | ], |
| 15 | user: 'user_12345', // Your user identifier |
| 16 | stream: false, |
| 17 | }); |
| 18 |  |
| 19 | console.log(response.choices[0].message.content); |
```

## Best Practices

### Choose Stable Identifiers

Use consistent, stable identifiers for the same user across requests:

* **Good**: `user_12345`, `customer_abc123`, `account_xyz789`
* **Avoid**: Random strings that change between requests

### Consider Privacy

When using user identifiers, consider privacy implications:

* Use internal user IDs rather than exposing personal information
* Avoid including personally identifiable information in user identifiers
* Consider using anonymized identifiers for better privacy protection

### Be Consistent

Use the same user identifier format throughout your application:

```
|  |  |
| --- | --- |
| 1 | # Consistent format |
| 2 | user_id = f"app_{internal_user_id}" |
```

Was this page helpful?

YesNo

[Previous](/docs/use-cases/usage-accounting)[#### Frameworks and Integrations Overview

Using OpenRouter with Popular Frameworks and Integrations

Next](/docs/community/frameworks-and-integrations-overview)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
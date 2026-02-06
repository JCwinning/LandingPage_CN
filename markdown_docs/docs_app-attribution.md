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

* [Benefits of App Attribution](#benefits-of-app-attribution)
* [Attribution Headers](#attribution-headers)
* [HTTP-Referer](#http-referer)
* [X-Title](#x-title)
* [Implementation Examples](#implementation-examples)
* [Where Your App Appears](#where-your-app-appears)
* [App Rankings](#app-rankings)
* [Model Apps Tabs](#model-apps-tabs)
* [Individual App Analytics](#individual-app-analytics)
* [Best Practices](#best-practices)
* [URL Requirements](#url-requirements)
* [Title Guidelines](#title-guidelines)
* [Privacy Considerations](#privacy-considerations)
* [Related Documentation](#related-documentation)

[Features](/docs/features/privacy-and-logging)

# App Attribution

Copy page

Get your app featured in OpenRouter rankings and analytics

App attribution allows developers to associate their API usage with their application, enabling visibility in OpenRouter’s public rankings and detailed analytics. By including simple headers in your requests, your app can appear in our leaderboards and gain insights into your model usage patterns.

## Benefits of App Attribution

When you properly attribute your app usage, you gain access to:

* **Public App Rankings**: Your app appears in OpenRouter’s [public rankings](https://openrouter.ai/rankings) with daily, weekly, and monthly leaderboards
* **Model Apps Tabs**: Your app is featured on individual model pages showing which apps use each model most
* **Detailed Analytics**: Access comprehensive analytics showing your app’s model usage over time, token consumption, and usage patterns
* **Professional Visibility**: Showcase your app to the OpenRouter developer community

## Attribution Headers

OpenRouter tracks app attribution through two optional HTTP headers:

### HTTP-Referer

The `HTTP-Referer` header identifies your app’s URL and is used as the primary identifier for rankings.

### X-Title

The `X-Title` header sets or modifies your app’s display name in rankings and analytics.

#####

Both headers are optional, but including them enables all attribution features. Apps using localhost URLs must include a title to be tracked.

## Implementation Examples

TypeScript SDKPython (OpenAI SDK)TypeScript (OpenAI SDK)Python (Direct API)TypeScript (fetch)cURL

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '<OPENROUTER_API_KEY>', |
| 5 | defaultHeaders: { |
| 6 | 'HTTP-Referer': 'https://myapp.com', // Your app's URL |
| 7 | 'X-Title': 'My AI Assistant', // Your app's display name |
| 8 | }, |
| 9 | }); |
| 10 |  |
| 11 | const completion = await openRouter.chat.send({ |
| 12 | model: 'openai/gpt-4o', |
| 13 | messages: [ |
| 14 | { |
| 15 | role: 'user', |
| 16 | content: 'Hello, world!', |
| 17 | }, |
| 18 | ], |
| 19 | stream: false, |
| 20 | }); |
| 21 |  |
| 22 | console.log(completion.choices[0].message); |
```

## Where Your App Appears

### App Rankings

Your attributed app will appear in OpenRouter’s main rankings page at [openrouter.ai/rankings](https://openrouter.ai/rankings). The rankings show:

* **Top Apps**: Largest public apps by token usage
* **Time Periods**: Daily, weekly, and monthly views
* **Usage Metrics**: Total token consumption across all models

### Model Apps Tabs

On individual model pages (e.g., [GPT-4o](https://openrouter.ai/models/openai/gpt-4o)), your app will be featured in the “Apps” tab showing:

* **Top Apps**: Apps using that specific model most
* **Weekly Rankings**: Updated weekly based on usage
* **Usage Context**: How your app compares to others using the same model

### Individual App Analytics

Once your app is tracked, you can access detailed analytics at `openrouter.ai/apps?url=<your-app-url>` including:

* **Model Usage Over Time**: Charts showing which models your app uses
* **Token Consumption**: Detailed breakdown of prompt and completion tokens
* **Usage Patterns**: Historical data to understand your app’s AI usage trends

## Best Practices

### URL Requirements

* Use your app’s primary domain (e.g., `https://myapp.com`)
* Avoid using subdomains unless they represent distinct apps
* For localhost development, always include a title header

### Title Guidelines

* Keep titles concise and descriptive
* Use your app’s actual name as users know it
* Avoid generic names like “AI App” or “Chatbot”

### Privacy Considerations

* Only public apps, meaning those that send headers, are included in rankings
* Attribution headers don’t expose sensitive information about your requests

## Related Documentation

* [Quickstart Guide](/docs/quickstart) - Basic setup with attribution headers
* [API Reference](/docs/api-reference/overview) - Complete header documentation
* [Usage Accounting](/docs/use-cases/usage-accounting) - Understanding your API usage

Was this page helpful?

YesNo

[Previous](/docs/features/provisioning-api-keys)[#### API Reference

An overview of OpenRouter's API

Next](/docs/api-reference/overview)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
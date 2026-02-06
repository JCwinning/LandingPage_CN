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

* [Using the OpenRouter SDK (Beta)](#using-the-openrouter-sdk-beta)
* [Using the OpenRouter API directly](#using-the-openrouter-api-directly)
* [Using the OpenAI SDK](#using-the-openai-sdk)
* [Using third-party SDKs](#using-third-party-sdks)

[Overview](/docs/quickstart)

# Quickstart

Copy page

Get started with OpenRouter

OpenRouter provides a unified API that gives you access to hundreds of AI models through a single endpoint, while automatically handling fallbacks and selecting the most cost-effective options. Get started with just a few lines of code using your preferred SDK or framework.

#####

Looking for information about free models and rate limits? Please see the [FAQ](/docs/faq#how-are-rate-limits-calculated)

In the examples below, the OpenRouter-specific headers are optional. Setting them allows your app to appear on the OpenRouter leaderboards. For detailed information about app attribution, see our [App Attribution guide](/docs/app-attribution).

## Using the OpenRouter SDK (Beta)

First, install the SDK:

npmyarnpnpm

```
|  |  |
| --- | --- |
| $ | npm install @openrouter/sdk |
```

Then use it in your code:

TypeScript SDK

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '<OPENROUTER_API_KEY>', |
| 5 | defaultHeaders: { |
| 6 | 'HTTP-Referer': '<YOUR_SITE_URL>', // Optional. Site URL for rankings on openrouter.ai. |
| 7 | 'X-Title': '<YOUR_SITE_NAME>', // Optional. Site title for rankings on openrouter.ai. |
| 8 | }, |
| 9 | }); |
| 10 |  |
| 11 | const completion = await openRouter.chat.send({ |
| 12 | model: 'openai/gpt-4o', |
| 13 | messages: [ |
| 14 | { |
| 15 | role: 'user', |
| 16 | content: 'What is the meaning of life?', |
| 17 | }, |
| 18 | ], |
| 19 | stream: false, |
| 20 | }); |
| 21 |  |
| 22 | console.log(completion.choices[0].message.content); |
```

## Using the OpenRouter API directly

#####

You can use the interactive [Request Builder](/request-builder) to generate OpenRouter API requests in the language of your choice.

PythonTypeScript (fetch)Shell

```
|  |  |
| --- | --- |
| 1 | import requests |
| 2 | import json |
| 3 |  |
| 4 | response = requests.post( |
| 5 | url="https://openrouter.ai/api/v1/chat/completions", |
| 6 | headers={ |
| 7 | "Authorization": "Bearer <OPENROUTER_API_KEY>", |
| 8 | "HTTP-Referer": "<YOUR_SITE_URL>", # Optional. Site URL for rankings on openrouter.ai. |
| 9 | "X-Title": "<YOUR_SITE_NAME>", # Optional. Site title for rankings on openrouter.ai. |
| 10 | }, |
| 11 | data=json.dumps({ |
| 12 | "model": "openai/gpt-4o", # Optional |
| 13 | "messages": [ |
| 14 | { |
| 15 | "role": "user", |
| 16 | "content": "What is the meaning of life?" |
| 17 | } |
| 18 | ] |
| 19 | }) |
| 20 | ) |
```

## Using the OpenAI SDK

TypescriptPython

```
|  |  |
| --- | --- |
| 1 | import OpenAI from 'openai'; |
| 2 |  |
| 3 | const openai = new OpenAI({ |
| 4 | baseURL: 'https://openrouter.ai/api/v1', |
| 5 | apiKey: '<OPENROUTER_API_KEY>', |
| 6 | defaultHeaders: { |
| 7 | 'HTTP-Referer': '<YOUR_SITE_URL>', // Optional. Site URL for rankings on openrouter.ai. |
| 8 | 'X-Title': '<YOUR_SITE_NAME>', // Optional. Site title for rankings on openrouter.ai. |
| 9 | }, |
| 10 | }); |
| 11 |  |
| 12 | async function main() { |
| 13 | const completion = await openai.chat.completions.create({ |
| 14 | model: 'openai/gpt-4o', |
| 15 | messages: [ |
| 16 | { |
| 17 | role: 'user', |
| 18 | content: 'What is the meaning of life?', |
| 19 | }, |
| 20 | ], |
| 21 | }); |
| 22 |  |
| 23 | console.log(completion.choices[0].message); |
| 24 | } |
| 25 |  |
| 26 | main(); |
```

The API also supports [streaming](/docs/api-reference/streaming).

## Using third-party SDKs

For information about using third-party SDKs and frameworks with OpenRouter, please [see our frameworks documentation.](/docs/community/frameworks-and-integrations-overview)

Was this page helpful?

YesNo

[#### Frequently Asked Questions

Common questions about OpenRouter

Next](/docs/faq)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
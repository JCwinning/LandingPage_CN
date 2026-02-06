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

* [Why use the OpenRouter SDK?](#why-use-the-openrouter-sdk)
* [Auto-generated from API specifications](#auto-generated-from-api-specifications)
* [Type-safe by default](#type-safe-by-default)
* [Installation](#installation)
* [Quick start](#quick-start)

[SDK Reference (BETA)](/docs/sdks/python)

# Python SDK

Copy page

Official OpenRouter Python SDK documentation

The OpenRouter Python SDK is a type-safe toolkit for building AI applications with access to 300+ language models through a unified API.

## Why use the OpenRouter SDK?

Integrating AI models into applications involves handling different provider APIs, managing model-specific requirements, and avoiding common implementation mistakes. The OpenRouter SDK standardizes these integrations and protects you from footguns.

```
|  |  |
| --- | --- |
| 1 | from openrouter import OpenRouter |
| 2 | import os |
| 3 |  |
| 4 | with OpenRouter( |
| 5 | api_key=os.getenv("OPENROUTER_API_KEY") |
| 6 | ) as client: |
| 7 | response = client.chat.send( |
| 8 | model="minimax/minimax-m2", |
| 9 | messages=[ |
| 10 | {"role": "user", "content": "Explain quantum computing"} |
| 11 | ] |
| 12 | ) |
```

The SDK provides three core benefits:

### Auto-generated from API specifications

The SDK is automatically generated from OpenRouter’s OpenAPI specs and updated with every API change. New models, parameters, and features appear in your IDE autocomplete immediately. No manual updates. No version drift.

```
|  |  |
| --- | --- |
| 1 | # When new models launch, they're available instantly |
| 2 | response = client.chat.send( |
| 3 | model="minimax/minimax-m2" |
| 4 | ) |
```

### Type-safe by default

Every parameter, response field, and configuration option is fully typed with Python type hints and validated with Pydantic. Invalid configurations are caught at runtime with clear error messages.

```
|  |  |
| --- | --- |
| 1 | response = client.chat.send( |
| 2 | model="minimax/minimax-m2", |
| 3 | messages=[ |
| 4 | {"role": "user", "content": "Hello"} |
| 5 | # ← Pydantic validates message structure |
| 6 | ], |
| 7 | temperature=0.7,  # ← Type-checked and validated |
| 8 | stream=True       # ← Response type changes based on this |
| 9 | ) |
```

**Actionable error messages:**

```
|  |  |
| --- | --- |
| 1 | # Instead of generic errors, get specific guidance: |
| 2 | # "Model 'openai/o1-preview' requires at least 2 messages. |
| 3 | #  You provided 1 message. Add a system or user message." |
```

**Type-safe streaming:**

```
|  |  |
| --- | --- |
| 1 | stream = client.chat.send( |
| 2 | model="minimax/minimax-m2", |
| 3 | messages=[{"role": "user", "content": "Write a story"}], |
| 4 | stream=True |
| 5 | ) |
| 6 |  |
| 7 | for event in stream: |
| 8 | # Full type information for streaming responses |
| 9 | content = event.choices[0].delta.content if event.choices else None |
```

**Async support:**

```
|  |  |
| --- | --- |
| 1 | import asyncio |
| 2 |  |
| 3 | async def main(): |
| 4 | async with OpenRouter( |
| 5 | api_key=os.getenv("OPENROUTER_API_KEY") |
| 6 | ) as client: |
| 7 | response = await client.chat.send_async( |
| 8 | model="minimax/minimax-m2", |
| 9 | messages=[{"role": "user", "content": "Hello"}] |
| 10 | ) |
| 11 | print(response.choices[0].message.content) |
| 12 |  |
| 13 | asyncio.run(main()) |
```

## Installation

```
|  |  |
| --- | --- |
| $ | # Using uv (recommended) |
| > | uv add openrouter |
| > |  |
| > | # Using pip |
| > | pip install openrouter |
| > |  |
| > | # Using poetry |
| > | poetry add openrouter |
```

**Requirements:** Python 3.9 or higher

Get your API key from [openrouter.ai/settings/keys](https://openrouter.ai/settings/keys).

## Quick start

```
|  |  |
| --- | --- |
| 1 | from openrouter import OpenRouter |
| 2 | import os |
| 3 |  |
| 4 | with OpenRouter( |
| 5 | api_key=os.getenv("OPENROUTER_API_KEY") |
| 6 | ) as client: |
| 7 | response = client.chat.send( |
| 8 | model="minimax/minimax-m2", |
| 9 | messages=[ |
| 10 | {"role": "user", "content": "Hello!"} |
| 11 | ] |
| 12 | ) |
| 13 |  |
| 14 | print(response.choices[0].message.content) |
```

Was this page helpful?

YesNo

[Previous](/docs/api-reference/completions/create-completions)[#### Analytics - Python SDK

Analytics method reference

Next](/docs/sdks/python/analytics)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
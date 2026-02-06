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

* [Using the Exacto Variant](#using-the-exacto-variant)
* [What Is the Exacto Variant?](#what-is-the-exacto-variant)
* [Why Use Exacto?](#why-use-exacto)
* [Why We Built It](#why-we-built-it)
* [Recommended Use Cases](#recommended-use-cases)
* [Supported Models](#supported-models)
* [How We Select Providers](#how-we-select-providers)

[Features](/docs/features/privacy-and-logging)

# Exacto Variant

Copy page

Route requests through OpenRouter-curated providers

Introducing a new set of endpoints, `:exacto`, focused on higher tool‑calling accuracy by routing to a sub‑group of providers with measurably better tool‑use success rates. It uses the same request payloads as any other variant, but filters endpoints so that only vetted providers for the chosen model are considered. To learn more, read our [blog post](https://openrouter.ai/announcements/provider-variance-introducing-exacto).

## Using the Exacto Variant

Add `:exacto` to the end of any supported model slug. The curated allowlist is enforced before provider sorting, fallback, or load balancing — no extra provider preference config is required.

TypeScript SDKTypeScript (OpenAI SDK)cURL

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: process.env.OPENROUTER_API_KEY, |
| 5 | }); |
| 6 |  |
| 7 | const completion = await openRouter.chat.send({ |
| 8 | model: "moonshotai/kimi-k2-0905:exacto", |
| 9 | messages: [ |
| 10 | { |
| 11 | role: "user", |
| 12 | content: "Draft a concise changelog entry for the Exacto launch.", |
| 13 | }, |
| 14 | ], |
| 15 | stream: false, |
| 16 | }); |
| 17 |  |
| 18 | console.log(completion.choices[0].message.content); |
```

#####

You can still supply fallback models with the `models` array. Any model that
carries the `:exacto` suffix will enforce the curated provider list when it is
selected.

## What Is the Exacto Variant?

Exacto is a curated routing variant specifically focused on tool‑calling accuracy. Unlike standard routing, which considers all available providers for a model, Exacto restricts routing to providers that demonstrate higher tool‑use accuracy and normal tool‑use propensity on real workloads.

## Why Use Exacto?

### Why We Built It

Providers running the same model can differ in accuracy due to implementation details in production inference. OpenRouter sees billions of requests monthly, giving us a unique vantage point to observe these differences and minimize surprises for users. Exacto combines benchmark results with real‑world tool‑calling telemetry to select the best‑performing providers.

### Recommended Use Cases

Exacto is optimized for quality‑sensitive, agentic workflows where tool‑calling accuracy and reliability are critical.

## Supported Models

Exacto endpoints are available for:

* Kimi K2 (`moonshotai/kimi-k2-0905:exacto`)
* DeepSeek v3.1 Terminus (`deepseek/deepseek-v3.1-terminus:exacto`)
* GLM 4.6 (`z-ai/glm-4.6:exacto`)
* GPT‑OSS 120B (`openai/gpt-oss-120b:exacto`)
* Qwen3 Coder (`qwen/qwen3-coder:exacto`)

## How We Select Providers

We use three inputs:

* Tool‑calling accuracy from real traffic across billions of calls
* Real‑time provider preferences (pins/ignores) from users making tool calls
* Benchmarking (internal eval suites, Groq OpenBench running LiveMCPBench, official tau2bench, and similar)

You will be routed only to providers that:

1. Are top‑tier on tool‑calling accuracy
2. Fall within a normal range of tool‑calling propensity
3. Are not frequently ignored or blacklisted by users when tools are provided

In our evaluations and open‑source benchmarks (e.g., tau2‑Bench, LiveMCPBench), Exacto shows materially fewer tool‑calling failures and more reliable tool use.

We will continue working with providers not currently in the Exacto pool to help them improve and be included. Exacto targets tool‑calling specifically and is not a broad statement on overall provider quality.

#####

If you have feedback on the Exacto variant, please fill out this form:
<https://openrouter.notion.site/2932fd57c4dc8097ba74ffb6d27f39d1?pvs=105>

Was this page helpful?

YesNo

[Previous](/docs/features/provider-routing)[#### Latency and Performance

Understanding OpenRouter's performance characteristics

Next](/docs/features/latency-and-performance)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
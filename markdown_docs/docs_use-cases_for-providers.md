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

* [For Providers](#for-providers)
* [1. List Models Endpoint](#1-list-models-endpoint)
* [2. Auto Top Up or Invoicing](#2-auto-top-up-or-invoicing)
* [3. Uptime Monitoring & Traffic Routing](#3-uptime-monitoring--traffic-routing)

[Use Cases](/docs/use-cases/byok)

# Provider Integration

Copy page

## For Providers

If you’d like to be a model provider and sell inference on OpenRouter, [fill out our form](https://openrouter.ai/how-to-list) to get started.

To be eligible to provide inference on OpenRouter you must have the following:

### 1. List Models Endpoint

You must implement an endpoint that returns all models that should be served by OpenRouter. At this endpoint, please return a list of all available models on your platform. Below is an example of the response format:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "data": [ |
| 3 | { |
| 4 | // Required |
| 5 | "id": "anthropic/claude-sonnet-4", |
| 6 | "hugging_face_id": "", // required if the model is on Hugging Face |
| 7 | "name": "Anthropic: Claude Sonnet 4", |
| 8 | "created": 1690502400, |
| 9 | "input_modalities": ["text", "image", "file"], |
| 10 | "output_modalities": ["text", "image", "file"], |
| 11 | "quantization": "fp8", |
| 12 | "context_length": 1000000, |
| 13 | "max_output_length": 128000, |
| 14 | "pricing": { |
| 15 | "prompt": "0.000008", // pricing per 1 token |
| 16 | "completion": "0.000024", // pricing per 1 token |
| 17 | "image": "0", // pricing per 1 image |
| 18 | "request": "0", // pricing per 1 request |
| 19 | "input_cache_reads": "0", // pricing per 1 token |
| 20 | "input_cache_writes": "0" // pricing per 1 token |
| 21 | }, |
| 22 | "supported_sampling_parameters": ["temperature", "stop"], |
| 23 | "supported_features": [ |
| 24 | "tools", |
| 25 | "json_mode", |
| 26 | "structured_outputs", |
| 27 | "web_search", |
| 28 | "reasoning" |
| 29 | ], |
| 30 | // Optional |
| 31 | "description": "Anthropic's flagship model...", |
| 32 | "openrouter": { |
| 33 | "slug": "anthropic/claude-sonnet-4" |
| 34 | }, |
| 35 | "datacenters": [ |
| 36 | { |
| 37 | "country_code": "US" // `Iso3166Alpha2Code` |
| 38 | } |
| 39 | ] |
| 40 | } |
| 41 | ] |
| 42 | } |
```

NOTE: `pricing` fields are in string format to avoid floating point precision issues, and must be in USD.

Valid quantization values are: `int4`, `int8`, `fp4`, `fp6`, `fp8`, `fp16`, `bf16`, `fp32`.

Valid sampling parameters are: `temperature`, `top_p`, `top_k`, `repetition_penalty`, `frequency_penalty`, `presence_penalty`, `stop`, `seed`.

Valid features are: `tools`, `json_mode`, `structured_outputs`, `web_search`, `reasoning`.

### 2. Auto Top Up or Invoicing

For OpenRouter to use the provider we must be able to pay for inference automatically. This can be done via auto top up or invoicing.

### 3. Uptime Monitoring & Traffic Routing

OpenRouter automatically monitors provider reliability and adjusts traffic routing based on uptime metrics. Your endpoint’s uptime is calculated as: **successful requests ÷ total requests** (excluding user errors).

**Errors that affect your uptime:**

* Authentication issues (401)
* Payment failures (402)
* Model not found (404)
* All server errors (500+)
* Mid-stream errors
* Successful requests with error finish reasons

**Errors that DON’T affect uptime:**

* Bad requests (400) - user input errors
* Oversized payloads (413) - user input errors
* Rate limiting (429) - tracked separately
* Geographic restrictions (403) - tracked separately

**Traffic routing thresholds:**

* **Minimum data**: 100+ requests required before uptime calculation begins
* **Normal routing**: 95%+ uptime
* **Degraded status**: 80-94% uptime → receives lower priority
* **Down status**: <80% uptime → only used as fallback

This system ensures traffic automatically flows to the most reliable providers while giving temporary issues time to resolve.

Was this page helpful?

YesNo

[Previous](/docs/use-cases/organization-management)[#### Reasoning Tokens

Next](/docs/use-cases/reasoning-tokens)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
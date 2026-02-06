# Query: What are model variants?

# Retrieved 5 chunks from openrouter.duckdb

────────────────────────────────────────────────────────────
## Chunk 1

**Similarity Score:** 0.6169

**Metadata:**
- file_path: /Users/jinchaoduan/Documents/post_project/AI_Blog/posts/RAG/markdown_docs/docs_features_exacto-variant.md
- file_name: docs_features_exacto-variant.md
- file_type: text/markdown
- file_size: 7972
- creation_date: 2025-11-21
- last_modified_date: 2025-11-21

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

────────────────────────────────────────────────────────────
## Chunk 2

**Similarity Score:** 0.6099

**Metadata:**
- file_path: /Users/jinchaoduan/Documents/post_project/AI_Blog/posts/RAG/markdown_docs/docs_overview_models.md
- file_name: docs_overview_models.md
- file_type: text/markdown
- file_size: 9021
- creation_date: 2025-11-21
- last_modified_date: 2025-11-21

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

* [Models API Standard](#models-api-standard)
* [API Response Schema](#api-response-schema)
* [Root Response Object](#root-response-object)
* [Model Object Schema](#model-object-schema)
* [Architecture Object](#architecture-object)
* [Pricing Object](#pricing-object)
* [Top Provider Object](#top-provider-object)
* [Supported Parameters](#supported-parameters)
* [For Providers](#for-providers)

[Overview](/docs/quickstart)

# Models

Copy page

One API for hundreds of models

Explore and browse 400+ models and providers [on our website](/models), or [with our API](/docs/api-reference/models/get-models). You can also subscribe to our [RSS feed](/api/v1/models?use_rss=true) to stay updated on new models.

## Models API Standard

Our [Models API](/docs/api-reference/models/get-models) makes the most important information about all LLMs freely available as soon as we confirm it.

### API Response Schema

The Models API returns a standardized JSON response format that provides comprehensive metadata for each available model. This schema is cached at the edge and designed for reliable integration for production applications.

#### Root Response Object

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "data": [ |
| 3 | /* Array of Model objects */ |
| 4 | ] |
| 5 | } |
```

#### Model Object Schema

Each model in the `data` array contains the following standardized fields:

| Field | Type | Description |
| --- | --- | --- |
| `id` | `string` | Unique model identifier used in API requests (e.g., `"google/gemini-2.5-pro-preview"`) |
| `canonical_slug` | `string` | Permanent slug for the model that never changes |
| `name` | `string` | Human-readable display name for the model |
| `created` | `number` | Unix timestamp of when the model was added to OpenRouter |
| `description` | `string` | Detailed description of the model’s capabilities and characteristics |
| `context_length` | `number` | Maximum context window size in tokens |
| `architecture` | `Architecture` | Object describing the model’s technical capabilities |
| `pricing` | `Pricing` | Lowest price structure for using this model |
| `top_provider` | `TopProvider` | Configuration details for the primary provider |
| `per_request_limits` | Rate limiting information (null if no limits) |  |
| `supported_parameters` | `string[]` | Array of supported API parameters for this model |

#### Architecture Object

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "input_modalities": string[], // Supported input types: ["file", "image", "text"] |
| 3 | "output_modalities": string[], // Supported output types: ["text"] |
| 4 | "tokenizer": string,          // Tokenization method used |
| 5 | "instruct_type": string | null // Instruction format type (null if not applicable) |
| 6 | } |
```

#### Pricing Object

All pricing values are in USD per token/request/unit. A value of `"0"` indicates the feature is free.

────────────────────────────────────────────────────────────
## Chunk 3

**Similarity Score:** 0.5820

**Metadata:**
- file_path: /Users/jinchaoduan/Documents/post_project/AI_Blog/posts/RAG/markdown_docs/docs_guides_overview_models.md
- file_name: docs_guides_overview_models.md
- file_type: text/markdown
- file_size: 7557
- creation_date: 2026-01-06
- last_modified_date: 2026-01-06

Search

`/`

Ask AI

[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)[Docs](/docs/api-reference/overview)

[Docs](/docs/quickstart)[API Reference](/docs/api/reference/overview)[SDK Reference](/docs/sdks/call-model/overview)

[Docs](/docs/quickstart)[API Reference](/docs/api/reference/overview)[SDK Reference](/docs/sdks/call-model/overview)

* Overview

  + [Quickstart](/docs/quickstart)
  + [Principles](/docs/guides/overview/principles)
  + [Models](/docs/guides/overview/models)
  + Multimodal
  + Authentication
  + [FAQ](/docs/faq)
  + [Enterprise](https://openrouter.ai/enterprise)
* Models & Routing

  + [Auto Model Selection](/docs/guides/routing/auto-model-selection)
  + [Model Fallbacks](/docs/guides/routing/model-fallbacks)
  + [Provider Selection](/docs/guides/routing/provider-selection)
  + Model Variants
* Features

  + [Presets](/docs/guides/features/presets)
  + [Tool Calling](/docs/guides/features/tool-calling)
  + Plugins
  + [Structured Outputs](/docs/guides/features/structured-outputs)
  + [Message Transforms](/docs/guides/features/message-transforms)
  + [Model Routing](/docs/guides/features/model-routing)
  + Routers
  + [Zero Completion Insurance](/docs/guides/features/zero-completion-insurance)
  + [ZDR](/docs/guides/features/zdr)
  + [App Attribution](/docs/app-attribution)
  + Broadcast
* + Privacy
  + Best Practices
  + Guides
  + Community

Light

On this page

* [Models API Standard](#models-api-standard)
* [API Response Schema](#api-response-schema)
* [Root Response Object](#root-response-object)
* [Model Object Schema](#model-object-schema)
* [Architecture Object](#architecture-object)
* [Pricing Object](#pricing-object)
* [Top Provider Object](#top-provider-object)
* [Supported Parameters](#supported-parameters)
* [For Providers](#for-providers)

[Overview](/docs/quickstart)

# Models

Copy page

One API for hundreds of models

Explore and browse 400+ models and providers [on our website](/models), or [with our API](/docs/api-reference/models/get-models). You can also subscribe to our [RSS feed](/api/v1/models?use_rss=true) to stay updated on new models.

## Models API Standard

Our [Models API](/docs/api-reference/models/get-models) makes the most important information about all LLMs freely available as soon as we confirm it.

### API Response Schema

The Models API returns a standardized JSON response format that provides comprehensive metadata for each available model. This schema is cached at the edge and designed for reliable integration with production applications.

#### Root Response Object

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "data": [ |
| 3 | /* Array of Model objects */ |
| 4 | ] |
| 5 | } |
```

#### Model Object Schema

Each model in the `data` array contains the following standardized fields:

| Field | Type | Description |
| --- | --- | --- |
| `id` | `string` | Unique model identifier used in API requests (e.g., `"google/gemini-2.5-pro-preview"`) |
| `canonical_slug` | `string` | Permanent slug for the model that never changes |
| `name` | `string` | Human-readable display name for the model |
| `created` | `number` | Unix timestamp of when the model was added to OpenRouter |
| `description` | `string` | Detailed description of the model’s capabilities and characteristics |
| `context_length` | `number` | Maximum context window size in tokens |
| `architecture` | `Architecture` | Object describing the model’s technical capabilities |
| `pricing` | `Pricing` | Lowest price structure for using this model |
| `top_provider` | `TopProvider` | Configuration details for the primary provider |
| `per_request_limits` | Rate limiting information (null if no limits) |  |
| `supported_parameters` | `string[]` | Array of supported API parameters for this model |

#### Architecture Object

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "input_modalities": string[], // Supported input types: ["file", "image", "text"] |
| 3 | "output_modalities": string[], // Supported output types: ["text"] |
| 4 | "tokenizer": string,          // Tokenization method used |
| 5 | "instruct_type": string | null // Instruction format type (null if not applicable) |
| 6 | } |
```

#### Pricing Object

All pricing values are in USD per token/request/unit. A value of `"0"` indicates the feature is free.

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "prompt": string,           // Cost per input token |
| 3 | "completion": string,       // Cost per output token |
| 4 | "request": string,          // Fixed cost per API request |
| 5 | "image": string,           // Cost per image input |
| 6 | "web_search": string,      // Cost per web search operation |
| 7 | "internal_reasoning": string, // Cost for internal reasoning tokens |
| 8 | "input_cache_read": string,   // Cost per cached input token read |
| 9 | "input_cache_write": string   // Cost per cached input token write |
| 10 | } |
```

#### Top Provider Object

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "context_length": number,        // Provider-specific context limit |
| 3 | "max_completion_tokens": number, // Maximum tokens in response |
| 4 | "is_moderated": boolean         // Whether content moderation is applied |
| 5 | } |
```

#### Supported Parameters

The `supported_parameters` array indicates which OpenAI-compatible parameters work with each model:

* `tools` - Function calling capabilities
* `tool_choice` - Tool selection control
* `max_tokens` - Response length limiting
* `temperature` - Randomness control
* `top_p` - Nucleus sampling
* `reasoning` - Internal reasoning mode
* `include_reasoning` - Include reasoning in response
* `structured_outputs` - JSON schema enforcement
* `response_format` - Output format specification
* `stop` - Custom stop sequences
* `frequency_penalty` - Repetition reduction
* `presence_penalty` - Topic diversity
* `seed` - Deterministic outputs

##### Different models tokenize text in different ways

Some models break up text into chunks of multiple characters (GPT, Claude,
Llama, etc), while others tokenize by character (PaLM). This means that token
counts (and therefore costs) will vary between models, even when inputs and
outputs are the same. Costs are displayed and billed according to the
tokenizer for the model in use. You can use the `usage` field in the response
to get the token counts for the input and output.

If there are models or providers you are interested in that OpenRouter doesn’t have, please tell us about them in our [Discord channel](https://openrouter.ai/discord).

## For Providers

If you’re interested in working with OpenRouter, you can learn more on our [providers page](/docs/use-cases/for-providers).

Was this page helpful?

YesNo

[Previous](/docs/guides/overview/principles)[#### Multimodal Capabilities

Send images, PDFs, audio, and video to OpenRouter models

Next](/docs/guides/overview/multimodal/overview)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/5a7e2b0bd58241d151e9e352d7a4f898df12c062576c0ce0184da76c3635c5d3/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/6f95fbca823560084c5593ea2aa4073f00710020e6a78f8a3f54e835d97a8a0b/content/assets/logo-white.svg)](https://openrouter.ai/)

[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)[Docs](/docs/api-reference/overview)

────────────────────────────────────────────────────────────
## Chunk 4

**Similarity Score:** 0.5763

**Metadata:**
- file_path: /Users/jinchaoduan/Documents/post_project/AI_Blog/posts/RAG/markdown_docs/docs_faq_how-are-rate-limits-calculated.md
- file_name: docs_faq_how-are-rate-limits-calculated.md
- file_type: text/markdown
- file_size: 17710
- creation_date: 2026-01-06
- last_modified_date: 2026-01-06

Search

`/`

Ask AI

[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)[Docs](/docs/api-reference/overview)

[Docs](/docs/quickstart)[API Reference](/docs/api/reference/overview)[SDK Reference](/docs/sdks/call-model/overview)

[Docs](/docs/quickstart)[API Reference](/docs/api/reference/overview)[SDK Reference](/docs/sdks/call-model/overview)

* Overview

  + [Quickstart](/docs/quickstart)
  + [Principles](/docs/guides/overview/principles)
  + [Models](/docs/guides/overview/models)
  + Multimodal
  + Authentication
  + [FAQ](/docs/faq)
  + [Enterprise](https://openrouter.ai/enterprise)
* Models & Routing

  + [Auto Model Selection](/docs/guides/routing/auto-model-selection)
  + [Model Fallbacks](/docs/guides/routing/model-fallbacks)
  + [Provider Selection](/docs/guides/routing/provider-selection)
  + Model Variants
* Features

  + [Presets](/docs/guides/features/presets)
  + [Tool Calling](/docs/guides/features/tool-calling)
  + Plugins
  + [Structured Outputs](/docs/guides/features/structured-outputs)
  + [Message Transforms](/docs/guides/features/message-transforms)
  + [Model Routing](/docs/guides/features/model-routing)
  + Routers
  + [Zero Completion Insurance](/docs/guides/features/zero-completion-insurance)
  + [ZDR](/docs/guides/features/zdr)
  + [App Attribution](/docs/app-attribution)
  + Broadcast
* + Privacy
  + Best Practices
  + Guides
  + Community

Light

On this page

* [Getting started](#getting-started)
* [Pricing and Fees](#pricing-and-fees)
* [Models and Providers](#models-and-providers)
* [API Technical Specifications](#api-technical-specifications)
* [Privacy and Data Logging](#privacy-and-data-logging)
* [Credit and Billing Systems](#credit-and-billing-systems)
* [Account Management](#account-management)

[Overview](/docs/quickstart)

# Frequently Asked Questions

Copy page

Common questions about OpenRouter

## Getting started

###### Why should I use OpenRouter?

OpenRouter provides a unified API to access all the major LLM models on the
market. It also allows users to aggregate their billing in one place and
keep track of all of their usage using our analytics.

OpenRouter passes through the pricing of the underlying providers, while pooling their uptime,
so you get the same pricing you’d get from the provider directly, with a
unified API and fallbacks so that you get much better uptime.

[Learn more in our Quickstart guide](/docs/quickstart).

###### How do I get started with OpenRouter?

To get started, create an account and add credits on the
[Credits](https://openrouter.ai/settings/credits) page. Credits are simply
deposits on OpenRouter that you use for LLM inference.
When you use the API or chat interface, we deduct the request cost from your
credits. Each model and provider has a different price per million tokens.

Once you have credits you can either use the chat room, or create API keys
and start using the API. You can read our [quickstart](/docs/quickstart)
guide for code samples and more.

###### How do I get support?

The best way to get technical support is to join our
[Discord](https://discord.gg/openrouter) and ask the community in the #help forum.

For billing and account management questions, please contact us at [[email protected]](/cdn-cgi/l/email-protection#c7b4b2b7b7a8b5b387a8b7a2a9b5a8b2b3a2b5e9a6ae).

###### How do I get billed for my usage on OpenRouter?

For each model we have the pricing displayed per million tokens. There is
usually a different price for prompt and completion tokens. There are also
models that charge per request, for images and for reasoning tokens. All of
these details will be visible on the models page.

When you make a request to OpenRouter, we receive the total number of tokens processed
by the provider. We then calculate the corresponding cost and deduct it from your credits.
You can review your complete usage history in the [Activity tab](https://openrouter.ai/activity).

You can also add the `usage: {include: true}` parameter to your chat request
to get the usage information in the response.

We pass through the pricing of the underlying providers; there is no markup
on inference pricing (however we do charge a [fee](/docs/faq#pricing-and-fees) when purchasing credits).

## Pricing and Fees

###### What are the fees for using OpenRouter?

OpenRouter charges a 5.5% ($0.80 minimum) fee when you purchase credits. We pass through
the pricing of the underlying model providers without any markup, so you pay
the same rate as you would directly with the provider.

Crypto payments are charged a fee of 5%.

###### Is there a fee for using my own provider keys (BYOK)?

Yes, if you choose to use your own provider API keys (Bring Your Own Key -
BYOK), the first 1M BYOK
requests per-month are free, and for all subsequent usage there is a fee
of 5% of what the same
model and provider would normally cost on OpenRouter. This fee is deducted
from your OpenRouter credits. This allows you to manage your rate limits and
costs directly with the provider while still leveraging OpenRouter’s unified
interface.

[Learn more about BYOK](/docs/guides/overview/auth/byok).

## Models and Providers

###### What LLM models does OpenRouter support?

OpenRouter provides access to a wide variety of LLM models, including frontier models from major AI labs.
For a complete list of models you can visit the [models browser](https://openrouter.ai/models) or fetch the list through the [models api](https://openrouter.ai/api/v1/models).

###### How frequently are new models added?

We work on adding models as quickly as we can. We often have partnerships with
the labs releasing models and can release models as soon as they are
available. If there is a model missing that you’d like OpenRouter to support, feel free to message us on
[Discord](https://discord.gg/openrouter).

###### What are model variants?

Variants are suffixes that can be added to the model slug to change its behavior.

Static variants can only be used with specific models and these are listed in our [models api](https://openrouter.ai/api/v1/models).

1. `:free` - The model is always provided for free and has low rate limits. [Learn more](/docs/guides/routing/model-variants/free).
2. `:extended` - The model has longer than usual context length. [Learn more](/docs/guides/routing/model-variants/extended).
3. `:exacto` - The model only uses OpenRouter-curated high-quality endpoints. [Learn more](/docs/guides/routing/model-variants/exacto).
4. `:thinking` - The model supports reasoning by default. [Learn more](/docs/guides/routing/model-variants/thinking).

Dynamic variants can be used on all models and they change the behavior of how the request is routed or used.

1. `:online` - All requests will run a query to extract web results that are attached to the prompt. [Learn more](/docs/guides/routing/model-variants/online).
2. `:nitro` - Providers will be sorted by throughput rather than the default sort, optimizing for faster response times. [Learn more](/docs/features/provider-routing#nitro-shortcut).
3. `:floor` - Providers will be sorted by price rather than the default sort, prioritizing the most cost-effective options. [Learn more](/docs/features/provider-routing#floor-price-shortcut).

###### I am an inference provider, how can I get listed on OpenRouter?

You can read our requirements at the [Providers
page](/docs/use-cases/for-providers). If you would like to contact us, the best
place to reach us is over email.

###### What is the expected latency/response time for different models?

For each model on OpenRouter we show the latency (time to first token) and the token
throughput for all providers. You can use this to estimate how long requests
will take. If you would like to optimize for throughput you can use the
`:nitro` variant to route to the fastest provider.

###### How does model fallback work if a provider is unavailable?

────────────────────────────────────────────────────────────
## Chunk 5

**Similarity Score:** 0.5701

**Metadata:**
- file_path: /Users/jinchaoduan/Documents/post_project/AI_Blog/posts/RAG/markdown_docs/docs_features_model-routing.md
- file_name: docs_features_model-routing.md
- file_type: text/markdown
- file_size: 7024
- creation_date: 2025-11-21
- last_modified_date: 2025-11-21

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

* [Auto Router](#auto-router)
* [The models parameter](#the-models-parameter)
* [Using with OpenAI SDK](#using-with-openai-sdk)

[Features](/docs/features/privacy-and-logging)

# Model Routing

Copy page

Dynamically route requests to models

OpenRouter provides two options for model routing.

## Auto Router

The [Auto Router](https://openrouter.ai/openrouter/auto), a special model ID that you can use to choose between selected high-quality models based on your prompt, powered by [NotDiamond](https://www.notdiamond.ai/).

TypeScript SDKTypeScript (fetch)Python

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '<OPENROUTER_API_KEY>', |
| 5 | }); |
| 6 |  |
| 7 | const completion = await openRouter.chat.send({ |
| 8 | model: 'openrouter/auto', |
| 9 | messages: [ |
| 10 | { |
| 11 | role: 'user', |
| 12 | content: 'What is the meaning of life?', |
| 13 | }, |
| 14 | ], |
| 15 | }); |
| 16 |  |
| 17 | console.log(completion.choices[0].message.content); |
```

The resulting generation will have `model` set to the model that was used.

## The `models` parameter

The `models` parameter lets you automatically try other models if the primary model’s providers are down, rate-limited, or refuse to reply due to content moderation.

TypeScript SDKTypeScript (fetch)Python

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '<OPENROUTER_API_KEY>', |
| 5 | }); |
| 6 |  |
| 7 | const completion = await openRouter.chat.send({ |
| 8 | models: ['anthropic/claude-3.5-sonnet', 'gryphe/mythomax-l2-13b'], |
| 9 | messages: [ |
| 10 | { |
| 11 | role: 'user', |
| 12 | content: 'What is the meaning of life?', |
| 13 | }, |
| 14 | ], |
| 15 | }); |
| 16 |  |
| 17 | console.log(completion.choices[0].message.content); |
```

If the model you selected returns an error, OpenRouter will try to use the fallback model instead. If the fallback model is down or returns an error, OpenRouter will return that error.

By default, any error can trigger the use of a fallback model, including context length validation errors, moderation flags for filtered models, rate-limiting, and downtime.

Requests are priced using the model that was ultimately used, which will be returned in the `model` attribute of the response body.

## Using with OpenAI SDK

To use the `models` array with the OpenAI SDK, include it in the `extra_body` parameter. In the example below, gpt-4o will be tried first, and the `models` array will be tried in order as fallbacks.

PythonTypeScript

```
|  |  |
| --- | --- |
| 1 | from openai import OpenAI |
| 2 |  |
| 3 | openai_client = OpenAI( |
| 4 | base_url="https://openrouter.ai/api/v1", |
| 5 | api_key={{API_KEY_REF}}, |
| 6 | ) |
| 7 |  |
| 8 | completion = openai_client.chat.completions.create( |
| 9 | model="openai/gpt-4o", |
| 10 | extra_body={ |
| 11 | "models": ["anthropic/claude-3.5-sonnet", "gryphe/mythomax-l2-13b"], |
| 12 | }, |
| 13 | messages=[ |
| 14 | { |
| 15 | "role": "user", |
| 16 | "content": "What is the meaning of life?" |
| 17 | } |
| 18 | ] |
| 19 | ) |
| 20 |  |
| 21 | print(completion.choices[0].message.content) |
```

Was this page helpful?


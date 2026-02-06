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

* [Inspecting cache usage](#inspecting-cache-usage)
* [OpenAI](#openai)
* [Grok](#grok)
* [Moonshot AI](#moonshot-ai)
* [Groq](#groq)
* [Anthropic Claude](#anthropic-claude)
* [DeepSeek](#deepseek)
* [Google Gemini](#google-gemini)
* [Implicit Caching](#implicit-caching)
* [Pricing Changes for Cached Requests:](#pricing-changes-for-cached-requests)
* [Supported Models and Limitations:](#supported-models-and-limitations)
* [How Gemini Prompt Caching works on OpenRouter:](#how-gemini-prompt-caching-works-on-openrouter)
* [How to Enable Gemini Prompt Caching:](#how-to-enable-gemini-prompt-caching)
* [Examples:](#examples)
* [System Message Caching Example](#system-message-caching-example)
* [User Message Caching Example](#user-message-caching-example)

[Features](/docs/features/privacy-and-logging)

# Prompt Caching

Copy page

Cache prompt messages

To save on inference costs, you can enable prompt caching on supported providers and models.

Most providers automatically enable prompt caching, but note that some (see Anthropic below) require you to enable it on a per-message basis.

When using caching (whether automatically in supported models, or via the `cache_control` property), OpenRouter will make a best-effort to continue routing to the same provider to make use of the warm cache. In the event that the provider with your cached prompt is not available, OpenRouter will try the next-best provider.

## Inspecting cache usage

To see how much caching saved on each generation, you can:

1. Click the detail button on the [Activity](/activity) page
2. Use the `/api/v1/generation` API, [documented here](/docs/api-reference/overview#querying-cost-and-stats)
3. Use `usage: {include: true}` in your request to get the cache tokens at the end of the response (see [Usage Accounting](/docs/use-cases/usage-accounting) for details)

The `cache_discount` field in the response body will tell you how much the response saved on cache usage. Some providers, like Anthropic, will have a negative discount on cache writes, but a positive discount (which reduces total cost) on cache reads.

## OpenAI

Caching price changes:

* **Cache writes**: no cost
* **Cache reads**: (depending on the model) charged at 0.25x or 0.50x the price of the original input pricing

[Click here to view OpenAI’s cache pricing per model.](https://platform.openai.com/docs/pricing)

Prompt caching with OpenAI is automated and does not require any additional configuration. There is a minimum prompt size of 1024 tokens.

[Click here to read more about OpenAI prompt caching and its limitation.](https://platform.openai.com/docs/guides/prompt-caching)

## Grok

Caching price changes:

* **Cache writes**: no cost
* **Cache reads**: charged at 0.25x the price of the original input pricing

[Click here to view Grok’s cache pricing per model.](https://docs.x.ai/docs/models#models-and-pricing)

Prompt caching with Grok is automated and does not require any additional configuration.

## Moonshot AI

Caching price changes:

* **Cache writes**: no cost
* **Cache reads**: charged at x the price of the original input pricing

Prompt caching with Moonshot AI is automated and does not require any additional configuration.

[Click here to view Moonshot AI’s caching documentation.](https://platform.moonshot.ai/docs/api/caching)

## Groq

Caching price changes:

* **Cache writes**: no cost
* **Cache reads**: charged at x the price of the original input pricing

Prompt caching with Groq is automated and does not require any additional configuration. Currently available on Kimi K2 models.

[Click here to view Groq’s documentation.](https://console.groq.com/docs/prompt-caching)

## Anthropic Claude

Caching price changes:

* **Cache writes**: charged at 1.25x the price of the original input pricing
* **Cache reads**: charged at 0.1x the price of the original input pricing

Prompt caching with Anthropic requires the use of `cache_control` breakpoints. There is a limit of four breakpoints, and the cache will expire within five minutes. Therefore, it is recommended to reserve the cache breakpoints for large bodies of text, such as character cards, CSV data, RAG data, book chapters, etc.

[Click here to read more about Anthropic prompt caching and its limitation.](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)

The `cache_control` breakpoint can only be inserted into the text part of a multipart message.

System message caching example:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "messages": [ |
| 3 | { |
| 4 | "role": "system", |
| 5 | "content": [ |
| 6 | { |
| 7 | "type": "text", |
| 8 | "text": "You are a historian studying the fall of the Roman Empire. You know the following book very well:" |
| 9 | }, |
| 10 | { |
| 11 | "type": "text", |
| 12 | "text": "HUGE TEXT BODY", |
| 13 | "cache_control": { |
| 14 | "type": "ephemeral" |
| 15 | } |
| 16 | } |
| 17 | ] |
| 18 | }, |
| 19 | { |
| 20 | "role": "user", |
| 21 | "content": [ |
| 22 | { |
| 23 | "type": "text", |
| 24 | "text": "What triggered the collapse?" |
| 25 | } |
| 26 | ] |
| 27 | } |
| 28 | ] |
| 29 | } |
```

User message caching example:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "messages": [ |
| 3 | { |
| 4 | "role": "user", |
| 5 | "content": [ |
| 6 | { |
| 7 | "type": "text", |
| 8 | "text": "Given the book below:" |
| 9 | }, |
| 10 | { |
| 11 | "type": "text", |
| 12 | "text": "HUGE TEXT BODY", |
| 13 | "cache_control": { |
| 14 | "type": "ephemeral" |
| 15 | } |
| 16 | }, |
| 17 | { |
| 18 | "type": "text", |
| 19 | "text": "Name all the characters in the above book" |
| 20 | } |
| 21 | ] |
| 22 | } |
| 23 | ] |
| 24 | } |
```

## DeepSeek

Caching price changes:

* **Cache writes**: charged at the same price as the original input pricing
* **Cache reads**: charged at 0.1x the price of the original input pricing

Prompt caching with DeepSeek is automated and does not require any additional configuration.

## Google Gemini

### Implicit Caching

Gemini 2.5 Pro and 2.5 Flash models now support **implicit caching**, providing automatic caching functionality similar to OpenAI’s automatic caching. Implicit caching works seamlessly — no manual setup or additional `cache_control` breakpoints required.

Pricing Changes:

* No cache write or storage costs.
* Cached tokens are charged at 0.25x the original input token cost.

Note that the TTL is on average 3-5 minutes, but will vary. There is a minimum of 1028 tokens for Gemini 2.5 Flash, and 2048 tokens for Gemini 2.5 Pro for requests to be eligible for caching.

[Official announcement from Google](https://developers.googleblog.com/en/gemini-2-5-models-now-support-implicit-caching/)

#####

To maximize implicit cache hits, keep the initial portion of your message
arrays consistent between requests. Push variations (such as user questions or
dynamic context elements) toward the end of your prompt/requests.

### Pricing Changes for Cached Requests:

* **Cache Writes:** Charged at the input token cost plus 5 minutes of cache storage, calculated as follows:

```
|  |
| --- |
| Cache write cost = Input token price + (Cache storage price × (5 minutes / 60 minutes)) |
```

* **Cache Reads:** Charged at 0.25× the original input token cost.

### Supported Models and Limitations:

Only certain Gemini models support caching. Please consult Google’s [Gemini API Pricing Documentation](https://ai.google.dev/gemini-api/docs/pricing) for the most current details.

Cache Writes have a 5 minute Time-to-Live (TTL) that does not update. After 5 minutes, the cache expires and a new cache must be written.

Gemini models have typically have a 4096 token minimum for cache write to occur. Cached tokens count towards the model’s maximum token usage. Gemini 2.5 Pro has a minimum of 2048 tokens, and Gemini 2.5 Flash has a minimum of 1028 tokens.

### How Gemini Prompt Caching works on OpenRouter:

OpenRouter simplifies Gemini cache management, abstracting away complexities:

* You **do not** need to manually create, update, or delete caches.
* You **do not** need to manage cache names or TTL explicitly.

### How to Enable Gemini Prompt Caching:

Gemini caching in OpenRouter requires you to insert `cache_control` breakpoints explicitly within message content, similar to Anthropic. We recommend using caching primarily for large content pieces (such as CSV files, lengthy character cards, retrieval augmented generation (RAG) data, or extensive textual sources).

#####

There is not a limit on the number of `cache_control` breakpoints you can
include in your request. OpenRouter will use only the last breakpoint for
Gemini caching. Including multiple breakpoints is safe and can help maintain
compatibility with Anthropic, but only the final one will be used for Gemini.

### Examples:

#### System Message Caching Example

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "messages": [ |
| 3 | { |
| 4 | "role": "system", |
| 5 | "content": [ |
| 6 | { |
| 7 | "type": "text", |
| 8 | "text": "You are a historian studying the fall of the Roman Empire. Below is an extensive reference book:" |
| 9 | }, |
| 10 | { |
| 11 | "type": "text", |
| 12 | "text": "HUGE TEXT BODY HERE", |
| 13 | "cache_control": { |
| 14 | "type": "ephemeral" |
| 15 | } |
| 16 | } |
| 17 | ] |
| 18 | }, |
| 19 | { |
| 20 | "role": "user", |
| 21 | "content": [ |
| 22 | { |
| 23 | "type": "text", |
| 24 | "text": "What triggered the collapse?" |
| 25 | } |
| 26 | ] |
| 27 | } |
| 28 | ] |
| 29 | } |
```

#### User Message Caching Example

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "messages": [ |
| 3 | { |
| 4 | "role": "user", |
| 5 | "content": [ |
| 6 | { |
| 7 | "type": "text", |
| 8 | "text": "Based on the book text below:" |
| 9 | }, |
| 10 | { |
| 11 | "type": "text", |
| 12 | "text": "HUGE TEXT BODY HERE", |
| 13 | "cache_control": { |
| 14 | "type": "ephemeral" |
| 15 | } |
| 16 | }, |
| 17 | { |
| 18 | "type": "text", |
| 19 | "text": "List all main characters mentioned in the text above." |
| 20 | } |
| 21 | ] |
| 22 | } |
| 23 | ] |
| 24 | } |
```

Was this page helpful?

YesNo

[Previous](/docs/features/presets)[#### Structured Outputs

Return structured data from your models

Next](/docs/features/structured-outputs)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
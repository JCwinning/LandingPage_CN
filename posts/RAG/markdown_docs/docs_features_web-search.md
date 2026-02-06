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

* [Parsing web search results](#parsing-web-search-results)
* [Customizing the Web Plugin](#customizing-the-web-plugin)
* [Engine Selection](#engine-selection)
* [Default Behavior](#default-behavior)
* [Forcing Engine Selection](#forcing-engine-selection)
* [Engine-Specific Pricing](#engine-specific-pricing)
* [Pricing](#pricing)
* [Exa Search Pricing](#exa-search-pricing)
* [Native Search Pricing (Provider Passthrough)](#native-search-pricing-provider-passthrough)
* [Search Context Size Thresholds](#search-context-size-thresholds)
* [Specifying Search Context Size](#specifying-search-context-size)
* [OpenAI Model Pricing](#openai-model-pricing)
* [Perplexity Model Pricing](#perplexity-model-pricing)

[Features](/docs/features/privacy-and-logging)

# Web Search

Copy page

Model-agnostic grounding

You can incorporate relevant web search results for *any* model on OpenRouter by activating and customizing the `web` plugin, or by appending `:online` to the model slug:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4o:online" |
| 3 | } |
```

This is a shortcut for using the `web` plugin, and is exactly equivalent to:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openrouter/auto", |
| 3 | "plugins": [{ "id": "web" }] |
| 4 | } |
```

The web search plugin is powered by native search for Anthropic and OpenAI natively and by [Exa](https://exa.ai/) for other models. For Exa, it uses their [“auto”](https://docs.exa.ai/reference/how-exa-search-works#combining-neural-and-keyword-the-best-of-both-worlds-through-exa-auto-search) method (a combination of keyword search and embeddings-based web search) to find the most relevant results and augment/ground your prompt.

## Parsing web search results

Web search results for all models (including native-only models like Perplexity and OpenAI Online) are available in the API and standardized by OpenRouterto follow the same annotation schema in the [OpenAI Chat Completion Message type](https://platform.openai.com/docs/api-reference/chat/object):

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "message": { |
| 3 | "role": "assistant", |
| 4 | "content": "Here's the latest news I found: ...", |
| 5 | "annotations": [ |
| 6 | { |
| 7 | "type": "url_citation", |
| 8 | "url_citation": { |
| 9 | "url": "https://www.example.com/web-search-result", |
| 10 | "title": "Title of the web search result", |
| 11 | "content": "Content of the web search result", // Added by OpenRouter if available |
| 12 | "start_index": 100, // The index of the first character of the URL citation in the message. |
| 13 | "end_index": 200 // The index of the last character of the URL citation in the message. |
| 14 | } |
| 15 | } |
| 16 | ] |
| 17 | } |
| 18 | } |
```

## Customizing the Web Plugin

The maximum results allowed by the web plugin and the prompt used to attach them to your message stream can be customized:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4o:online", |
| 3 | "plugins": [ |
| 4 | { |
| 5 | "id": "web", |
| 6 | "engine": "exa", // Optional: "native", "exa", or undefined |
| 7 | "max_results": 1, // Defaults to 5 |
| 8 | "search_prompt": "Some relevant web results:" // See default below |
| 9 | } |
| 10 | ] |
| 11 | } |
```

By default, the web plugin uses the following search prompt, using the current date:

```
|  |
| --- |
| A web search was conducted on `date`. Incorporate the following web search results into your response. |
|  |
| IMPORTANT: Cite them using markdown links named using the domain of the source. |
| Example: [nytimes.com](https://nytimes.com/some-page). |
```

## Engine Selection

The web search plugin supports the following options for the `engine` parameter:

* **`native`**: Always uses the model provider’s built-in web search capabilities
* **`exa`**: Uses Exa’s search API for web results
* **`undefined` (not specified)**: Uses native search if available for the provider, otherwise falls back to Exa

### Default Behavior

When the `engine` parameter is not specified:

* **Native search is used by default** for OpenAI and Anthropic models that support it
* **Exa search is used** for all other models or when native search is not supported

When you explicitly specify `"engine": "native"`, it will always attempt to use the provider’s native search, even if the model doesn’t support it (which may result in an error).

### Forcing Engine Selection

You can explicitly specify which engine to use:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4o", |
| 3 | "plugins": [ |
| 4 | { |
| 5 | "id": "web", |
| 6 | "engine": "native" |
| 7 | } |
| 8 | ] |
| 9 | } |
```

Or force Exa search even for models that support native search:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4o", |
| 3 | "plugins": [ |
| 4 | { |
| 5 | "id": "web", |
| 6 | "engine": "exa", |
| 7 | "max_results": 3 |
| 8 | } |
| 9 | ] |
| 10 | } |
```

### Engine-Specific Pricing

* **Native search**: Pricing is passed through directly from the provider (see provider-specific pricing sections below)
* **Exa search**: Uses OpenRouter credits at $4 per 1000 results (default 5 results = $0.02 per request)

## Pricing

### Exa Search Pricing

When using Exa search (either explicitly via `"engine": "exa"` or as fallback), the web plugin uses your OpenRouter credits and charges *$4 per 1000 results*. By default, `max_results` set to 5, this comes out to a maximum of $0.02 per request, in addition to the LLM usage for the search result prompt tokens.

### Native Search Pricing (Provider Passthrough)

Some models have built-in web search. These models charge a fee based on the search context size, which determines how much search data is retrieved and processed for a query.

### Search Context Size Thresholds

Search context can be ‘low’, ‘medium’, or ‘high’ and determines how much search context is retrieved for a query:

* **Low**: Minimal search context, suitable for basic queries
* **Medium**: Moderate search context, good for general queries
* **High**: Extensive search context, ideal for detailed research

### Specifying Search Context Size

You can specify the search context size in your API request using the `web_search_options` parameter:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4.1", |
| 3 | "messages": [ |
| 4 | { |
| 5 | "role": "user", |
| 6 | "content": "What are the latest developments in quantum computing?" |
| 7 | } |
| 8 | ], |
| 9 | "web_search_options": { |
| 10 | "search_context_size": "high" |
| 11 | } |
| 12 | } |
```

### OpenAI Model Pricing

For GPT-4.1, GPT-4o, and GPT-4o search preview Models:

| Search Context Size | Price per 1000 Requests |
| --- | --- |
| Low | $30.00 |
| Medium | $35.00 |
| High | $50.00 |

For GPT-4.1-Mini, GPT-4o-Mini, and GPT-4o-Mini-Search-Preview Models:

| Search Context Size | Price per 1000 Requests |
| --- | --- |
| Low | $25.00 |
| Medium | $27.50 |
| High | $30.00 |

### Perplexity Model Pricing

For Sonar and SonarReasoning:

| Search Context Size | Price per 1000 Requests |
| --- | --- |
| Low | $5.00 |
| Medium | $8.00 |
| High | $12.00 |

For SonarPro and SonarReasoningPro:

| Search Context Size | Price per 1000 Requests |
| --- | --- |
| Low | $6.00 |
| Medium | $10.00 |
| High | $14.00 |

##### Engine Parameter

The pricing above applies when using `"engine": "native"` or when native search is used by default for supported models. When using `"engine": "exa"`, the Exa search pricing ($4 per 1000 results) applies instead.

##### Pricing Documentation

For more detailed information about pricing models, refer to the official documentation:

* [OpenAI Pricing](https://platform.openai.com/docs/pricing#web-search)
* [Anthropic Pricing](https://docs.claude.com/en/docs/agents-and-tools/tool-use/web-search-tool#usage-and-pricing)
* [Perplexity Pricing](https://docs.perplexity.ai/guides/pricing)

Was this page helpful?

YesNo

[Previous](/docs/features/uptime-optimization)[#### Zero Completion Insurance

OpenRouter will not charge you for zero token responses

Next](/docs/features/zero-completion-insurance)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
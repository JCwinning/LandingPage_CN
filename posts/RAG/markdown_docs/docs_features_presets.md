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

* [What are Presets?](#what-are-presets)
* [Quick Start](#quick-start)
* [Benefits](#benefits)
* [Separation of Concerns](#separation-of-concerns)
* [Rapid Iteration](#rapid-iteration)
* [Using Presets](#using-presets)
* [Other Notes](#other-notes)

[Features](/docs/features/privacy-and-logging)

# Presets

Copy page

Manage your LLM configurations

[Presets](/settings/presets) allow you to separate your LLM configuration from your code. Create and manage presets through the OpenRouter web application to control provider routing, model selection, system prompts, and other parameters, then reference them in OpenRouter API requests.

## What are Presets?

Presets are named configurations that encapsulate all the settings needed for a specific use case. For example, you might create:

* An “email-copywriter” preset for generating marketing copy
* An “inbound-classifier” preset for categorizing customer inquiries
* A “code-reviewer” preset for analyzing pull requests

Each preset can manage:

* Provider routing preferences (sort by price, latency, etc.)
* Model selection (specific model or array of models with fallbacks)
* System prompts
* Generation parameters (temperature, top\_p, etc.)
* Provider inclusion/exclusion rules

## Quick Start

1. [Create a preset](/settings/presets). For example, select a model and restrict provider routing to just a few providers.
   ![Creating a new preset](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/preset-example.png "A new preset")
2. Make an API request to the preset:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "@preset/ravenel-bridge", |
| 3 | "messages": [ |
| 4 | { |
| 5 | "role": "user", |
| 6 | "content": "What's your opinion of the Golden Gate Bridge? Isn't it beautiful?" |
| 7 | } |
| 8 | ] |
| 9 | } |
```

## Benefits

### Separation of Concerns

Presets help you maintain a clean separation between your application code and LLM configuration. This makes your code more semantic and easier to maintain.

### Rapid Iteration

Update your LLM configuration without deploying code changes:

* Switch to new model versions
* Adjust system prompts
* Modify parameters
* Change provider preferences

## Using Presets

There are three ways to use presets in your API requests.

1. **Direct Model Reference**

You can reference the preset as if it was a model by sending requests to `@preset/preset-slug`

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "@preset/email-copywriter", |
| 3 | "messages": [ |
| 4 | { |
| 5 | "role": "user", |
| 6 | "content": "Write a marketing email about our new feature" |
| 7 | } |
| 8 | ] |
| 9 | } |
```

2. **Preset Field**

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4", |
| 3 | "preset": "email-copywriter", |
| 4 | "messages": [ |
| 5 | { |
| 6 | "role": "user", |
| 7 | "content": "Write a marketing email about our new feature" |
| 8 | } |
| 9 | ] |
| 10 | } |
```

3. **Combined Model and Preset**

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/gpt-4@preset/email-copywriter", |
| 3 | "messages": [ |
| 4 | { |
| 5 | "role": "user", |
| 6 | "content": "Write a marketing email about our new feature" |
| 7 | } |
| 8 | ] |
| 9 | } |
```

## Other Notes

1. If you’re using an organization account, all members can access organization presets. This is a great way to share best practices across teams.
2. Version history is kept in order to understand changes that were made, and to be able to roll back. However when addressing a preset through the API, the latest version is always used.
3. If you provide parameters in the request, they will be shallow-merged with the options configured in the preset.

Was this page helpful?

YesNo

[Previous](/docs/features/latency-and-performance)[#### Prompt Caching

Cache prompt messages

Next](/docs/features/prompt-caching)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
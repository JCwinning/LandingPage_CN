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

* [Effect AI SDK](#effect-ai-sdk)
* [Effect AI SDK](#effect-ai-sdk-1)

[Community](/docs/community/frameworks-and-integrations-overview)

# Effect AI SDK

Copy page

Integrate OpenRouter using the Effect AI SDK

# Effect AI SDK

> Integrate OpenRouter using the Effect AI SDK. Complete guide for integrating the Effect AI SDK with OpenRouter.

## Effect AI SDK

You can use the [Effect AI SDK](https://www.npmjs.com/package/%40effect/ai) to integrate OpenRouter with your Effect applications. To get started, install the following packages:

* [effect](https://www.npmjs.com/package/effect): the Effect core (if not already installed)
* [@effect/ai](https://www.npmjs.com/package/%40effect/ai): the core Effect AI SDK abstractions
* [@effect/ai-openrouter](https://www.npmjs.com/package/%40effect/ai-openrouter): the Effect AI provider integration for OpenRouter
* [@effect/platform](https://www.npmjs.com/package/%40effect/platform): platform-agnostic abstractions for Effect

```
|  |  |
| --- | --- |
| $ | npm install effect @effect/ai @effect/ai-openrouter @effect/platform |
```

Once that’s done you can use the [LanguageModel](https://effect.website/docs/ai/getting-started/#define-an-interaction-with-a-language-model) module to define interactions with a large language model via OpenRouter.

TypeScript

```
|  |  |
| --- | --- |
| 1 | import { LanguageModel } from "@effect/ai" |
| 2 | import { OpenRouterClient, OpenRouterLanguageModel } from "@effect/ai-openrouter" |
| 3 | import { FetchHttpClient } from "@effect/platform" |
| 4 | import { Config, Effect, Layer, Stream } from "effect" |
| 5 |  |
| 6 | const Gpt4o = OpenRouterLanguageModel.model("openai/gpt-4o") |
| 7 |  |
| 8 | const program = LanguageModel.streamText({ |
| 9 | prompt: [ |
| 10 | { role: "system", content: "You are a comedian with a penchant for groan-inducing puns" }, |
| 11 | { role: "user", content: [{ type: "text", text: "Tell me a dad joke" }] } |
| 12 | ] |
| 13 | }).pipe( |
| 14 | Stream.filter((part) => part.type === "text-delta"), |
| 15 | Stream.runForEach((part) => Effect.sync(() => process.stdout.write(part.delta))), |
| 16 | Effect.provide(Gpt4o) |
| 17 | ) |
| 18 |  |
| 19 | const OpenRouter = OpenRouterClient.layerConfig({ |
| 20 | apiKey: Config.redacted("OPENROUTER_API_KEY") |
| 21 | }).pipe(Layer.provide(FetchHttpClient.layer)) |
| 22 |  |
| 23 | program.pipe( |
| 24 | Effect.provide(OpenRouter), |
| 25 | Effect.runPromise |
| 26 | ) |
```

Was this page helpful?

YesNo

[Previous](/docs/community/frameworks-and-integrations-overview)[#### Arize

Using OpenRouter with Arize

Next](/docs/community/arize)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
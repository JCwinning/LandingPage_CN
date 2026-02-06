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

* [Vercel AI SDK](#vercel-ai-sdk)

[Community](/docs/community/frameworks-and-integrations-overview)

# Vercel AI SDK

Copy page

Using OpenRouter with Vercel AI SDK

## Vercel AI SDK

You can use the [Vercel AI SDK](https://www.npmjs.com/package/ai) to integrate OpenRouter with your Next.js app. To get started, install [@openrouter/ai-sdk-provider](https://github.com/OpenRouterTeam/ai-sdk-provider):

```
|  |  |
| --- | --- |
| $ | npm install @openrouter/ai-sdk-provider |
```

And then you can use [streamText()](https://sdk.vercel.ai/docs/reference/ai-sdk-core/stream-text) API to stream text from OpenRouter.

TypeScript

```
|  |  |
| --- | --- |
| 1 | import { createOpenRouter } from '@openrouter/ai-sdk-provider'; |
| 2 | import { streamText } from 'ai'; |
| 3 | import { z } from 'zod'; |
| 4 |  |
| 5 | export const getLasagnaRecipe = async (modelName: string) => { |
| 6 | const openrouter = createOpenRouter({ |
| 7 | apiKey: '${API_KEY_REF}', |
| 8 | }); |
| 9 |  |
| 10 | const response = streamText({ |
| 11 | model: openrouter(modelName), |
| 12 | prompt: 'Write a vegetarian lasagna recipe for 4 people.', |
| 13 | }); |
| 14 |  |
| 15 | await response.consumeStream(); |
| 16 | return response.text; |
| 17 | }; |
| 18 |  |
| 19 | export const getWeather = async (modelName: string) => { |
| 20 | const openrouter = createOpenRouter({ |
| 21 | apiKey: '${API_KEY_REF}', |
| 22 | }); |
| 23 |  |
| 24 | const response = streamText({ |
| 25 | model: openrouter(modelName), |
| 26 | prompt: 'What is the weather in San Francisco, CA in Fahrenheit?', |
| 27 | tools: { |
| 28 | getCurrentWeather: { |
| 29 | description: 'Get the current weather in a given location', |
| 30 | parameters: z.object({ |
| 31 | location: z |
| 32 | .string() |
| 33 | .describe('The city and state, e.g. San Francisco, CA'), |
| 34 | unit: z.enum(['celsius', 'fahrenheit']).optional(), |
| 35 | }), |
| 36 | execute: async ({ location, unit = 'celsius' }) => { |
| 37 | // Mock response for the weather |
| 38 | const weatherData = { |
| 39 | 'Boston, MA': { |
| 40 | celsius: '15°C', |
| 41 | fahrenheit: '59°F', |
| 42 | }, |
| 43 | 'San Francisco, CA': { |
| 44 | celsius: '18°C', |
| 45 | fahrenheit: '64°F', |
| 46 | }, |
| 47 | }; |
| 48 |  |
| 49 | const weather = weatherData[location]; |
| 50 | if (!weather) { |
| 51 | return `Weather data for ${location} is not available.`; |
| 52 | } |
| 53 |  |
| 54 | return `The current weather in ${location} is ${weather[unit]}.`; |
| 55 | }, |
| 56 | }, |
| 57 | }, |
| 58 | }); |
| 59 |  |
| 60 | await response.consumeStream(); |
| 61 | return response.text; |
| 62 | }; |
```

Was this page helpful?

YesNo

[Previous](/docs/community/pydantic-ai)[#### Xcode

Using OpenRouter with Apple Intelligence in Xcode

Next](/docs/community/xcode)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T15:30:01.083Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T15:30:01.083Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
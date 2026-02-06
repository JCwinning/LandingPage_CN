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

* [How It Works](#how-it-works)
* [Fallback Behavior](#fallback-behavior)
* [Pricing](#pricing)
* [Using with OpenAI SDK](#using-with-openai-sdk)

[Models & Routing](/docs/guides/routing/auto-model-selection)

# Model Fallbacks

Copy page

Automatic failover between models

The `models` parameter lets you automatically try other models if the primary model’s providers are down, rate-limited, or refuse to reply due to content moderation.

## How It Works

Provide an array of model IDs in priority order. If the first model returns an error, OpenRouter will automatically try the next model in the list.

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

## Fallback Behavior

If the model you selected returns an error, OpenRouter will try to use the fallback model instead. If the fallback model is down or returns an error, OpenRouter will return that error.

By default, any error can trigger the use of a fallback model, including:

* Context length validation errors
* Moderation flags for filtered models
* Rate-limiting
* Downtime

## Pricing

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

YesNo

[Previous](/docs/guides/routing/auto-model-selection)[#### Provider Routing

Route requests to the best provider

Next](/docs/guides/routing/provider-selection)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/5a7e2b0bd58241d151e9e352d7a4f898df12c062576c0ce0184da76c3635c5d3/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/6f95fbca823560084c5593ea2aa4073f00710020e6a78f8a3f54e835d97a8a0b/content/assets/logo-white.svg)](https://openrouter.ai/)

[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)[Docs](/docs/api-reference/overview)
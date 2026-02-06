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

* [How to Use](#how-to-use)
* [Response](#response)

[Models & Routing](/docs/guides/routing/auto-model-selection)

# Auto Model Selection

Copy page

Automatically choose the best model for your prompt

The [Auto Router](https://openrouter.ai/openrouter/auto) is a special model ID that automatically chooses between selected high-quality models based on your prompt, powered by [NotDiamond](https://www.notdiamond.ai/).

## How to Use

Simply set your model to `openrouter/auto` and OpenRouter will intelligently select the best model for your specific prompt.

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

## Response

The resulting generation will have `model` set to the model that was actually used for your request, allowing you to track which model was selected.

Was this page helpful?

YesNo

[Previous](/docs/faq)[#### Model Fallbacks

Automatic failover between models

Next](/docs/guides/routing/model-fallbacks)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/5a7e2b0bd58241d151e9e352d7a4f898df12c062576c0ce0184da76c3635c5d3/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/6f95fbca823560084c5593ea2aa4073f00710020e6a78f8a3f54e835d97a8a0b/content/assets/logo-white.svg)](https://openrouter.ai/)

[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)[Docs](/docs/api-reference/overview)
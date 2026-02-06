# RAG Results - OpenRouter Documentation

Generated on: 2025-11-22 01:23:03

---

## Question 1: What are model variants?

**Answer:**

Model variants are suffixes you can append to a model’s slug to modify its behavior. They are static options tied to specific models and include flags like `:free`, `:beta`, `:extended`, `:exacto`, and `:thinking`. Each variant changes aspects such as cost, moderation, context length, endpoint quality, or reasoning support.

**Sources:**

1. docs_faq_how-are-rate-limits-calculated.md
2. docs_faq.md
3. docs_overview_models.md

**Retrieved Context:**

### Context 1 (from docs_faq_how-are-rate-limits-calculated.md)

```
###### How frequently are new models added?

We work on adding models as quickly as we can. We often have partnerships with
the labs releasing models and can release models as soon as they are
available. If there is a model missing that you’d like OpenRouter to support, feel free to message us on
[Discord](https://discord.gg/openrouter).

###### What are model variants?

Variants are suffixes that can be added to the model slug to change its behavior.

Static variants can only be used with speci...
```

### Context 2 (from docs_faq.md)

```
###### How frequently are new models added?

We work on adding models as quickly as we can. We often have partnerships with
the labs releasing models and can release models as soon as they are
available. If there is a model missing that you’d like OpenRouter to support, feel free to message us on
[Discord](https://discord.gg/openrouter).

###### What are model variants?

Variants are suffixes that can be added to the model slug to change its behavior.

Static variants can only be used with speci...
```

### Context 3 (from docs_overview_models.md)

```
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
...
```

---

## Question 2: How do I use streaming with OpenRouter?

**Answer:**

Streaming is enabled by setting `stream: true` in the chat request and then iterating over the returned async iterator. In the TypeScript SDK you can do:

```ts
const stream = await openRouter.chat.send({
  model: '…',
  messages: [{role: 'user', content: 'Your question'}],
  stream: true,
  streamOptions: {includeUsage: true},
});

for await (const chunk of stream) {
  const text = chunk.choices?.[0]?.delta?.content;
  if (text) console.log(text);
  if (chunk.usage) console.log('Usage:', chunk.usage);
}
```

Each chunk arrives as it’s generated, and the final chunk includes usage statistics.

**Sources:**

1. docs_quickstart.md
2. docs_api-reference_streaming.md
3. docs_quickstart.md

**Retrieved Context:**

### Context 1 (from docs_quickstart.md)

```
The API also supports [streaming](/docs/api-reference/streaming).

## Using third-party SDKs

For information about using third-party SDKs and frameworks with OpenRouter, please [see our frameworks documentation.](/docs/community/frameworks-and-integrations-overview)

Was this page helpful?

YesNo

[#### Frequently Asked Questions

Common questions about OpenRouter

Next](/docs/faq)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](...
```

### Context 2 (from docs_api-reference_streaming.md)

```
Here is an example of how to stream a response, and process it:

TypeScript SDKPythonTypeScript (fetch)

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | const question = 'How would you build the tallest building ever?'; |
| 8 |  |
| 9 | const stream = await openRouter.chat.send({ |
| 10 | model: '{{MODEL}}', |
| 11 | messages: [{ role: 'user', content...
```

### Context 3 (from docs_quickstart.md)

```
## Using the OpenRouter SDK (Beta)

First, install the SDK:

npmyarnpnpm

```
|  |  |
| --- | --- |
| $ | npm install @openrouter/sdk |
```

Then use it in your code:

TypeScript SDK

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '<OPENROUTER_API_KEY>', |
| 5 | defaultHeaders: { |
| 6 | 'HTTP-Referer': '<YOUR_SITE_URL>', // Optional. Site URL for rankings on openrouter.ai. |
| 7 | 'X-Title': '<YO...
```

---

## Question 3: What are the pricing options for OpenRouter?

**Answer:**

OpenRouter charges a 5.5 % fee (with a $0.80 minimum) when you buy credits, but it passes through the underlying providers’ model prices with no markup. If you pay with cryptocurrency, a 5 % processing fee applies. Using your own provider keys (BYOK) incurs no additional fee beyond the provider’s own pricing.

**Sources:**

1. docs_faq_how-are-rate-limits-calculated.md
2. docs_faq.md
3. docs_faq.md

**Retrieved Context:**

### Context 1 (from docs_faq_how-are-rate-limits-calculated.md)

```
Was this page helpful?

YesNo

[Previous](/docs/quickstart)[#### Principles

Core principles and values of OpenRouter

Next](/docs/overview/principles)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/as...
```

### Context 2 (from docs_faq.md)

```
Was this page helpful?

YesNo

[Previous](/docs/quickstart)[#### Principles

Core principles and values of OpenRouter

Next](/docs/overview/principles)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/as...
```

### Context 3 (from docs_faq.md)

```
When you make a request to OpenRouter, we receive the total number of tokens processed
by the provider. We then calculate the corresponding cost and deduct it from your credits.
You can review your complete usage history in the [Activity tab](https://openrouter.ai/activity).

You can also add the `usage: {include: true}` parameter to your chat request
to get the usage information in the response.

We pass through the pricing of the underlying providers; there is no markup
on inference pricing (h...
```

---


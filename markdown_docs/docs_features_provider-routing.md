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

* [Price-Based Load Balancing (Default Strategy)](#price-based-load-balancing-default-strategy)
* [Provider Sorting](#provider-sorting)
* [Nitro Shortcut](#nitro-shortcut)
* [Floor Price Shortcut](#floor-price-shortcut)
* [Ordering Specific Providers](#ordering-specific-providers)
* [Example: Specifying providers with fallbacks](#example-specifying-providers-with-fallbacks)
* [Example: Specifying providers with fallbacks disabled](#example-specifying-providers-with-fallbacks-disabled)
* [Targeting Specific Provider Endpoints](#targeting-specific-provider-endpoints)
* [Requiring Providers to Support All Parameters](#requiring-providers-to-support-all-parameters)
* [Example: Excluding providers that don’t support JSON formatting](#example-excluding-providers-that-dont-support-json-formatting)
* [Requiring Providers to Comply with Data Policies](#requiring-providers-to-comply-with-data-policies)
* [Example: Excluding providers that don’t comply with data policies](#example-excluding-providers-that-dont-comply-with-data-policies)
* [Zero Data Retention Enforcement](#zero-data-retention-enforcement)
* [Example: Enforcing ZDR for a specific request](#example-enforcing-zdr-for-a-specific-request)
* [Distillable Text Enforcement](#distillable-text-enforcement)
* [Example: Enforcing distillable text for a specific request](#example-enforcing-distillable-text-for-a-specific-request)
* [Disabling Fallbacks](#disabling-fallbacks)
* [Allowing Only Specific Providers](#allowing-only-specific-providers)
* [Example: Allowing Azure for a request calling GPT-4 Omni](#example-allowing-azure-for-a-request-calling-gpt-4-omni)
* [Ignoring Providers](#ignoring-providers)
* [Example: Ignoring DeepInfra for a request calling Llama 3.3 70b](#example-ignoring-deepinfra-for-a-request-calling-llama-33-70b)
* [Quantization](#quantization)
* [Quantization Levels](#quantization-levels)
* [Example: Requesting FP8 Quantization](#example-requesting-fp8-quantization)
* [Max Price](#max-price)
* [Terms of Service](#terms-of-service)

[Features](/docs/features/privacy-and-logging)

# Provider Routing

Copy page

Route requests to the best provider

OpenRouter routes requests to the best available providers for your model. By default, [requests are load balanced](/docs/features/provider-routing#price-based-load-balancing-default-strategy) across the top providers to maximize uptime.

You can customize how your requests are routed using the `provider` object in the request body for [Chat Completions](/docs/api-reference/chat-completion) and [Completions](/docs/api-reference/completion).

The `provider` object can contain the following fields:

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `order` | string[] | - | List of provider slugs to try in order (e.g. `["anthropic", "openai"]`). [Learn more](/docs/features/provider-routing#ordering-specific-providers) |
| `allow_fallbacks` | boolean | `true` | Whether to allow backup providers when the primary is unavailable. [Learn more](/docs/features/provider-routing#disabling-fallbacks) |
| `require_parameters` | boolean | `false` | Only use providers that support all parameters in your request. [Learn more](/docs/features/provider-routing#requiring-providers-to-support-all-parameters-beta) |
| `data_collection` | ”allow” | “deny" | "allow” | Control whether to use providers that may store data. [Learn more](/docs/features/provider-routing#requiring-providers-to-comply-with-data-policies) |
| `zdr` | boolean | - | Restrict routing to only ZDR (Zero Data Retention) endpoints. [Learn more](/docs/features/provider-routing#zero-data-retention-enforcement) |
| `enforce_distillable_text` | boolean | - | Restrict routing to only models that allow text distillation. [Learn more](/docs/features/provider-routing#distillable-text-enforcement) |
| `only` | string[] | - | List of provider slugs to allow for this request. [Learn more](/docs/features/provider-routing#allowing-only-specific-providers) |
| `ignore` | string[] | - | List of provider slugs to skip for this request. [Learn more](/docs/features/provider-routing#ignoring-providers) |
| `quantizations` | string[] | - | List of quantization levels to filter by (e.g. `["int4", "int8"]`). [Learn more](/docs/features/provider-routing#quantization) |
| `sort` | string | - | Sort providers by price or throughput. (e.g. `"price"` or `"throughput"`). [Learn more](/docs/features/provider-routing#provider-sorting) |
| `max_price` | object | - | The maximum pricing you want to pay for this request. [Learn more](/docs/features/provider-routing#maximum-price) |

##### EU data residency (Enterprise)

OpenRouter supports EU in-region routing for enterprise customers. When enabled, prompts and completions are processed entirely within the EU. Learn more in our [Privacy docs here](/docs/features/privacy-and-logging#enterprise-eu-in-region-routing). To contact our enterprise team, [fill out this form](https://openrouter.ai/enterprise/form).

## Price-Based Load Balancing (Default Strategy)

For each model in your request, OpenRouter’s default behavior is to load balance requests across providers, prioritizing price.

If you are more sensitive to throughput than price, you can use the `sort` field to explicitly prioritize throughput.

#####

When you send a request with `tools` or `tool_choice`, OpenRouter will only
route to providers that support tool use. Similarly, if you set a
`max_tokens`, then OpenRouter will only route to providers that support a
response of that length.

Here is OpenRouter’s default load balancing strategy:

1. Prioritize providers that have not seen significant outages in the last 30 seconds.
2. For the stable providers, look at the lowest-cost candidates and select one weighted by inverse square of the price (example below).
3. Use the remaining providers as fallbacks.

##### A Load Balancing Example

If Provider A costs $1 per million tokens, Provider B costs $2, and Provider C costs $3, and Provider B recently saw a few outages.

* Your request is routed to Provider A. Provider A is 9x more likely to be first routed to Provider A than Provider C because (1/32=1/9)(1 / 3^2 = 1/9)(1/32=1/9) (inverse square of the price).
* If Provider A fails, then Provider C will be tried next.
* If Provider C also fails, Provider B will be tried last.

If you have `sort` or `order` set in your provider preferences, load balancing will be disabled.

## Provider Sorting

As described above, OpenRouter load balances based on price, while taking uptime into account.

If you instead want to *explicitly* prioritize a particular provider attribute, you can include the `sort` field in the `provider` preferences. Load balancing will be disabled, and the router will try providers in order.

The three sort options are:

* `"price"`: prioritize lowest price
* `"throughput"`: prioritize highest throughput
* `"latency"`: prioritize lowest latency

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
| 8 | model: 'meta-llama/llama-3.1-70b-instruct', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | sort: 'throughput', |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

To *always* prioritize low prices, and not apply any load balancing, set `sort` to `"price"`.

To *always* prioritize low latency, and not apply any load balancing, set `sort` to `"latency"`.

## Nitro Shortcut

You can append `:nitro` to any model slug as a shortcut to sort by throughput. This is exactly equivalent to setting `provider.sort` to `"throughput"`.

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
| 8 | model: 'meta-llama/llama-3.1-70b-instruct:nitro', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | stream: false, |
| 11 | }); |
```

## Floor Price Shortcut

You can append `:floor` to any model slug as a shortcut to sort by price. This is exactly equivalent to setting `provider.sort` to `"price"`.

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
| 8 | model: 'meta-llama/llama-3.1-70b-instruct:floor', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | stream: false, |
| 11 | }); |
```

## Ordering Specific Providers

You can set the providers that OpenRouter will prioritize for your request using the `order` field.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `order` | string[] | - | List of provider slugs to try in order (e.g. `["anthropic", "openai"]`). |

The router will prioritize providers in this list, and in this order, for the model you’re using. If you don’t set this field, the router will [load balance](/docs/features/provider-routing#price-based-load-balancing-default-strategy) across the top providers to maximize uptime.

#####

You can use the copy button next to provider names on model pages to get the exact provider slug,
including any variants like “/turbo”. See [Targeting Specific Provider Endpoints](/docs/features/provider-routing#targeting-specific-provider-endpoints) for details.

OpenRouter will try them one at a time and proceed to other providers if none are operational. If you don’t want to allow any other providers, you should [disable fallbacks](/docs/features/provider-routing#disabling-fallbacks) as well.

### Example: Specifying providers with fallbacks

This example skips over OpenAI (which doesn’t host Mixtral), tries Together, and then falls back to the normal list of providers on OpenRouter:

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
| 8 | model: 'mistralai/mixtral-8x7b-instruct', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | order: ['openai', 'together'], |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

### Example: Specifying providers with fallbacks disabled

Here’s an example with `allow_fallbacks` set to `false` that skips over OpenAI (which doesn’t host Mixtral), tries Together, and then fails if Together fails:

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
| 8 | model: 'mistralai/mixtral-8x7b-instruct', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | order: ['openai', 'together'], |
| 12 | allowFallbacks: false, |
| 13 | }, |
| 14 | stream: false, |
| 15 | }); |
```

## Targeting Specific Provider Endpoints

Each provider on OpenRouter may host multiple endpoints for the same model, such as a default endpoint and a specialized “turbo” endpoint. To target a specific endpoint, you can use the copy button next to the provider name on the model detail page to obtain the exact provider slug.

For example, DeepInfra offers DeepSeek R1 through multiple endpoints:

* Default endpoint with slug `deepinfra`
* Turbo endpoint with slug `deepinfra/turbo`

By copying the exact provider slug and using it in your request’s `order` array, you can ensure your request is routed to the specific endpoint you want:

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
| 8 | model: 'deepseek/deepseek-r1', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | order: ['deepinfra/turbo'], |
| 12 | allowFallbacks: false, |
| 13 | }, |
| 14 | stream: false, |
| 15 | }); |
```

This approach is especially useful when you want to consistently use a specific variant of a model from a particular provider.

## Requiring Providers to Support All Parameters

You can restrict requests only to providers that support all parameters in your request using the `require_parameters` field.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `require_parameters` | boolean | `false` | Only use providers that support all parameters in your request. |

With the default routing strategy, providers that don’t support all the [LLM parameters](/docs/api-reference/parameters) specified in your request can still receive the request, but will ignore unknown parameters. When you set `require_parameters` to `true`, the request won’t even be routed to that provider.

### Example: Excluding providers that don’t support JSON formatting

For example, to only use providers that support JSON formatting:

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
| 8 | messages: [{ role: 'user', content: 'Hello' }], |
| 9 | provider: { |
| 10 | requireParameters: true, |
| 11 | }, |
| 12 | responseFormat: { type: 'json_object' }, |
| 13 | stream: false, |
| 14 | }); |
```

## Requiring Providers to Comply with Data Policies

You can restrict requests only to providers that comply with your data policies using the `data_collection` field.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `data_collection` | ”allow” | “deny" | "allow” | Control whether to use providers that may store data. |

* `allow`: (default) allow providers which store user data non-transiently and may train on it
* `deny`: use only providers which do not collect user data

Some model providers may log prompts, so we display them with a **Data Policy** tag on model pages. This is not a definitive source of third party data policies, but represents our best knowledge.

##### Account-Wide Data Policy Filtering

This is also available as an account-wide setting in [your privacy
settings](https://openrouter.ai/settings/privacy). You can disable third party
model providers that store inputs for training.

### Example: Excluding providers that don’t comply with data policies

To exclude providers that don’t comply with your data policies, set `data_collection` to `deny`:

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
| 8 | messages: [{ role: 'user', content: 'Hello' }], |
| 9 | provider: { |
| 10 | dataCollection: 'deny', // or "allow" |
| 11 | }, |
| 12 | stream: false, |
| 13 | }); |
```

## Zero Data Retention Enforcement

You can enforce Zero Data Retention (ZDR) on a per-request basis using the `zdr` parameter, ensuring your request only routes to endpoints that do not retain prompts.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `zdr` | boolean | - | Restrict routing to only ZDR (Zero Data Retention) endpoints. |

When `zdr` is set to `true`, the request will only be routed to endpoints that have a Zero Data Retention policy. When `zdr` is `false` or not provided, it has no effect on routing.

##### Account-Wide ZDR Setting

This is also available as an account-wide setting in [your privacy
settings](https://openrouter.ai/settings/privacy). The per-request `zdr` parameter
operates as an “OR” with your account-wide ZDR setting - if either is enabled, ZDR enforcement will be applied. The request-level parameter can only ensure ZDR is enabled, not override account-wide enforcement.

### Example: Enforcing ZDR for a specific request

To ensure a request only uses ZDR endpoints, set `zdr` to `true`:

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
| 8 | model: 'gpt-4', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | zdr: true, |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

This is useful for customers who don’t want to globally enforce ZDR but need to ensure specific requests only route to ZDR endpoints.

## Distillable Text Enforcement

You can enforce distillable text filtering on a per-request basis using the `enforce_distillable_text` parameter, ensuring your request only routes to models where the author has allowed text distillation.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `enforce_distillable_text` | boolean | - | Restrict routing to only models that allow text distillation. |

When `enforce_distillable_text` is set to `true`, the request will only be routed to models where the author has explicitly enabled text distillation. When `enforce_distillable_text` is `false` or not provided, it has no effect on routing.

This parameter is useful for applications that need to ensure their requests only use models that allow text distillation for training purposes, such as when building datasets for model fine-tuning or distillation workflows.

### Example: Enforcing distillable text for a specific request

To ensure a request only uses models that allow text distillation, set `enforce_distillable_text` to `true`:

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
| 8 | model: 'meta-llama/llama-3.1-70b-instruct', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | enforceDistillableText: true, |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

## Disabling Fallbacks

To guarantee that your request is only served by the top (lowest-cost) provider, you can disable fallbacks.

This is combined with the `order` field from [Ordering Specific Providers](/docs/features/provider-routing#ordering-specific-providers) to restrict the providers that OpenRouter will prioritize to just your chosen list.

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
| 8 | messages: [{ role: 'user', content: 'Hello' }], |
| 9 | provider: { |
| 10 | allowFallbacks: false, |
| 11 | }, |
| 12 | stream: false, |
| 13 | }); |
```

## Allowing Only Specific Providers

You can allow only specific providers for a request by setting the `only` field in the `provider` object.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `only` | string[] | - | List of provider slugs to allow for this request. |

#####

Only allowing some providers may significantly reduce fallback options and
limit request recovery.

##### Account-Wide Allowed Providers

You can allow providers for all account requests by configuring your [preferences](/settings/preferences). This configuration applies to all API requests and chatroom messages.

Note that when you allow providers for a specific request, the list of allowed providers is merged with your account-wide allowed providers.

### Example: Allowing Azure for a request calling GPT-4 Omni

Here’s an example that will only use Azure for a request calling GPT-4 Omni:

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
| 8 | model: 'openai/gpt-4o', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | only: ['azure'], |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

## Ignoring Providers

You can ignore providers for a request by setting the `ignore` field in the `provider` object.

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `ignore` | string[] | - | List of provider slugs to skip for this request. |

#####

Ignoring multiple providers may significantly reduce fallback options and
limit request recovery.

##### Account-Wide Ignored Providers

You can ignore providers for all account requests by configuring your [preferences](/settings/preferences). This configuration applies to all API requests and chatroom messages.

Note that when you ignore providers for a specific request, the list of ignored providers is merged with your account-wide ignored providers.

### Example: Ignoring DeepInfra for a request calling Llama 3.3 70b

Here’s an example that will ignore DeepInfra for a request calling Llama 3.3 70b:

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
| 8 | model: 'meta-llama/llama-3.3-70b-instruct', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | ignore: ['deepinfra'], |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

## Quantization

Quantization reduces model size and computational requirements while aiming to preserve performance. Most LLMs today use FP16 or BF16 for training and inference, cutting memory requirements in half compared to FP32. Some optimizations use FP8 or quantization to reduce size further (e.g., INT8, INT4).

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `quantizations` | string[] | - | List of quantization levels to filter by (e.g. `["int4", "int8"]`). [Learn more](/docs/features/provider-routing#quantization) |

#####

Quantized models may exhibit degraded performance for certain prompts,
depending on the method used.

Providers can support various quantization levels for open-weight models.

### Quantization Levels

By default, requests are load-balanced across all available providers, ordered by price. To filter providers by quantization level, specify the `quantizations` field in the `provider` parameter with the following values:

* `int4`: Integer (4 bit)
* `int8`: Integer (8 bit)
* `fp4`: Floating point (4 bit)
* `fp6`: Floating point (6 bit)
* `fp8`: Floating point (8 bit)
* `fp16`: Floating point (16 bit)
* `bf16`: Brain floating point (16 bit)
* `fp32`: Floating point (32 bit)
* `unknown`: Unknown

### Example: Requesting FP8 Quantization

Here’s an example that will only use providers that support FP8 quantization:

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
| 8 | model: 'meta-llama/llama-3.1-8b-instruct', |
| 9 | messages: [{ role: 'user', content: 'Hello' }], |
| 10 | provider: { |
| 11 | quantizations: ['fp8'], |
| 12 | }, |
| 13 | stream: false, |
| 14 | }); |
```

### Max Price

To filter providers by price, specify the `max_price` field in the `provider` parameter with a JSON object specifying the highest provider pricing you will accept.

For example, the value `{"prompt": 1, "completion": 2}` will route to any provider with a price of `<= $1/m` prompt tokens, and `<= $2/m` completion tokens or less.

Some providers support per request pricing, in which case you can use the `request` attribute of max\_price. Lastly, `image` is also available, which specifies the max price per image you will accept.

Practically, this field is often combined with a provider `sort` to express, for example, “Use the provider with the highest throughput, as long as it doesn’t cost more than `$x/m` tokens.”

## Terms of Service

You can view the terms of service for each provider below. You may not violate the terms of service or policies of third-party providers that power the models on OpenRouter.

Was this page helpful?

YesNo

[Previous](/docs/features/model-routing)[#### Exacto Variant

Route requests through OpenRouter-curated providers

Next](/docs/features/exacto-variant)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
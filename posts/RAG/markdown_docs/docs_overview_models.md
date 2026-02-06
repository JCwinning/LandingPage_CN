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

[Previous](/docs/overview/principles)[#### Privacy, Logging, and Data Collection

Making sure your data is safe

Next](/docs/features/privacy-and-logging)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
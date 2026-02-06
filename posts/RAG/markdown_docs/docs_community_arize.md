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

* [Using Arize](#using-arize)
* [Installation](#installation)
* [Prerequisites](#prerequisites)
* [Why OpenRouter Works with Arize](#why-openrouter-works-with-arize)
* [Configuration](#configuration)
* [Simple LLM Call](#simple-llm-call)
* [What Gets Traced](#what-gets-traced)
* [JavaScript/TypeScript Support](#javascripttypescript-support)
* [Common Issues](#common-issues)
* [Learn More](#learn-more)

[Community](/docs/community/frameworks-and-integrations-overview)

# Arize

Copy page

Using OpenRouter with Arize

## Using Arize

[Arize](https://arize.com/) provides observability and tracing for LLM applications. Since OpenRouter uses the OpenAI API schema, you can utilize Arize’s OpenInference auto-instrumentation with the OpenAI SDK to automatically trace and monitor your OpenRouter API calls.

### Installation

```
|  |  |
| --- | --- |
| $ | pip install openinference-instrumentation-openai openai arize-otel |
```

### Prerequisites

* OpenRouter account and API key
* Arize account with Space ID and API Key

### Why OpenRouter Works with Arize

Arize’s OpenInference auto-instrumentation works with OpenRouter because:

1. **OpenRouter provides a fully OpenAI-API-compatible endpoint** - The `/v1` endpoint mirrors OpenAI’s schema
2. **Reuse official OpenAI SDKs** - Point the OpenAI client’s `base_url` to OpenRouter
3. **Automatic instrumentation** - OpenInference hooks into OpenAI SDK calls seamlessly

### Configuration

Set up your environment variables:

Environment Setup

```
|  |  |
| --- | --- |
| 1 | import os |
| 2 |  |
| 3 | # Set your OpenRouter API key |
| 4 | os.environ["OPENAI_API_KEY"] = "${API_KEY_REF}" |
```

### Simple LLM Call

Initialize Arize and instrument your OpenAI client to automatically trace OpenRouter calls:

Basic Integration

```
|  |  |
| --- | --- |
| 1 | from arize.otel import register |
| 2 | from openinference.instrumentation.openai import OpenAIInstrumentor |
| 3 | import openai |
| 4 |  |
| 5 | # Initialize Arize and register the tracer provider |
| 6 | tracer_provider = register( |
| 7 | space_id="your-space-id", |
| 8 | api_key="your-arize-api-key", |
| 9 | project_name="your-project-name", |
| 10 | ) |
| 11 |  |
| 12 | # Instrument OpenAI SDK |
| 13 | OpenAIInstrumentor().instrument(tracer_provider=tracer_provider) |
| 14 |  |
| 15 | # Configure OpenAI client for OpenRouter |
| 16 | client = openai.OpenAI( |
| 17 | base_url="https://openrouter.ai/api/v1", |
| 18 | api_key="your_openrouter_api_key", |
| 19 | default_headers={ |
| 20 | "HTTP-Referer": "<YOUR_SITE_URL>",  # Optional: Your site URL |
| 21 | "X-Title": "<YOUR_SITE_NAME>",      # Optional: Your site name |
| 22 | } |
| 23 | ) |
| 24 |  |
| 25 | # Make a traced chat completion request |
| 26 | response = client.chat.completions.create( |
| 27 | model="meta-llama/llama-3.1-8b-instruct:free", |
| 28 | messages=[ |
| 29 | {"role": "user", "content": "Write a haiku about observability."} |
| 30 | ], |
| 31 | ) |
| 32 |  |
| 33 | # Print the assistant's reply |
| 34 | print(response.choices[0].message.content) |
```

### What Gets Traced

All OpenRouter model calls are automatically traced and include:

* Request/response data and timing
* Model name and provider information
* Token usage and cost data (when supported)
* Error handling and debugging information

### JavaScript/TypeScript Support

OpenInference also provides instrumentation for the OpenAI JavaScript/TypeScript SDK, which works with OpenRouter. For setup and examples, please refer to the [OpenInference JavaScript examples for OpenAI](https://github.com/Arize-ai/openinference/tree/main/js).

### Common Issues

* **API Key**: Use your OpenRouter API key, not OpenAI’s
* **Model Names**: Use exact model names from [OpenRouter’s model list](https://openrouter.ai/models)
* **Rate Limits**: Check your OpenRouter dashboard for usage limits

### Learn More

* **Arize OpenRouter Integration**: <https://arize.com/docs/ax/integrations/llm-providers/openrouter/openrouter-tracing>
* **OpenRouter Quick Start Guide**: <https://openrouter.ai/docs/quickstart>
* **OpenInference OpenAI Instrumentation**: <https://github.com/Arize-ai/openinference/tree/main/python/instrumentation/openinference-instrumentation-openai>

Was this page helpful?

YesNo

[Previous](/docs/community/effect-ai-sdk)[#### LangChain

Using OpenRouter with LangChain

Next](/docs/community/lang-chain)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
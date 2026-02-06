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

* [Using LiveKit Agents](#using-livekit-agents)
* [Installation](#installation)
* [Authentication](#authentication)
* [Basic Usage](#basic-usage)
* [Advanced Features](#advanced-features)
* [Fallback Models](#fallback-models)
* [Provider Routing](#provider-routing)
* [Web Search Plugin](#web-search-plugin)
* [Analytics Integration](#analytics-integration)
* [Resources](#resources)

[Community](/docs/community/frameworks-and-integrations-overview)

# LiveKit

Copy page

Using OpenRouter with LiveKit Agents

## Using LiveKit Agents

[LiveKit Agents](https://docs.livekit.io/agents/) is an open-source framework for building voice AI agents. The OpenRouter plugin allows you to access 500+ AI models from multiple providers through a unified API, with automatic fallback support and intelligent routing.

### Installation

Install the OpenAI plugin to add OpenRouter support:

```
|  |  |
| --- | --- |
| $ | uv add "livekit-agents[openai]~=1.2" |
```

### Authentication

The OpenRouter plugin requires an [OpenRouter API key](https://openrouter.ai/settings/keys). Set `OPENROUTER_API_KEY` in your `.env` file.

### Basic Usage

Create an OpenRouter LLM using the `with_openrouter` method:

Python

```
|  |  |
| --- | --- |
| 1 | from livekit.plugins import openai |
| 2 |  |
| 3 | session = AgentSession( |
| 4 | llm=openai.LLM.with_openrouter(model="anthropic/claude-sonnet-4.5"), |
| 5 | # ... tts, stt, vad, turn_detection, etc. |
| 6 | ) |
```

### Advanced Features

#### Fallback Models

Configure multiple fallback models to use if the primary model is unavailable:

Python

```
|  |  |
| --- | --- |
| 1 | from livekit.plugins import openai |
| 2 |  |
| 3 | llm = openai.LLM.with_openrouter( |
| 4 | model="openai/gpt-4o", |
| 5 | fallback_models=[ |
| 6 | "anthropic/claude-sonnet-4", |
| 7 | "openai/gpt-5-mini", |
| 8 | ], |
| 9 | ) |
```

#### Provider Routing

Control which providers are used for model inference:

Python

```
|  |  |
| --- | --- |
| 1 | from livekit.plugins import openai |
| 2 |  |
| 3 | llm = openai.LLM.with_openrouter( |
| 4 | model="deepseek/deepseek-chat-v3.1", |
| 5 | provider={ |
| 6 | "order": ["novita/fp8", "gmicloud/fp8", "google-vertex"], |
| 7 | "allow_fallbacks": True, |
| 8 | "sort": "latency", |
| 9 | }, |
| 10 | ) |
```

#### Web Search Plugin

Enable OpenRouter’s web search capabilities:

Python

```
|  |  |
| --- | --- |
| 1 | from livekit.plugins import openai |
| 2 |  |
| 3 | llm = openai.LLM.with_openrouter( |
| 4 | model="google/gemini-2.5-flash-preview-09-2025", |
| 5 | plugins=[ |
| 6 | openai.OpenRouterWebPlugin( |
| 7 | max_results=5, |
| 8 | search_prompt="Search for relevant information", |
| 9 | ) |
| 10 | ], |
| 11 | ) |
```

#### Analytics Integration

Include site and app information for OpenRouter analytics:

Python

```
|  |  |
| --- | --- |
| 1 | from livekit.plugins import openai |
| 2 |  |
| 3 | llm = openai.LLM.with_openrouter( |
| 4 | model="openrouter/auto", |
| 5 | site_url="https://myapp.com", |
| 6 | app_name="My Voice Agent", |
| 7 | ) |
```

### Resources

* [LiveKit OpenRouter Plugin Documentation](https://docs.livekit.io/agents/models/llm/plugins/openrouter/)
* [LiveKit Agents GitHub](https://github.com/livekit/agents)
* [OpenRouter Models](https://openrouter.ai/models)

Was this page helpful?

YesNo

[Previous](/docs/community/lang-chain)[#### Langfuse

Using OpenRouter with Langfuse

Next](/docs/community/langfuse)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
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

* [Using Xcode with Apple Intelligence](#using-xcode-with-apple-intelligence)
* [Prerequisites](#prerequisites)
* [Setup Instructions](#setup-instructions)
* [Step 1: Access Intelligence Settings](#step-1-access-intelligence-settings)
* [Step 2: Configure OpenRouter Provider](#step-2-configure-openrouter-provider)
* [Step 3: Browse Available Models](#step-3-browse-available-models)
* [Step 4: Start Using AI in Xcode](#step-4-start-using-ai-in-xcode)
* [Using Apple Intelligence Features](#using-apple-intelligence-features)
* [Learn More](#learn-more)

[Community](/docs/community/frameworks-and-integrations-overview)

# Xcode

Copy page

Using OpenRouter with Apple Intelligence in Xcode

## Using Xcode with Apple Intelligence

[Apple Intelligence](https://developer.apple.com/apple-intelligence/) in Xcode 26 provides built-in AI assistance for coding. By integrating OpenRouter, you can access hundreds of AI models directly in your Xcode development environment, going far beyond the default ChatGPT integration.

This integration allows you to use models from Anthropic, Google, Meta, and many other providers without leaving your development environment.

### Prerequisites

#####

Apple Intelligence on Xcode is currently in Beta and requires:

* **macOS Tahoe 26.0 Beta** or later
* **[Xcode 26 beta 4](https://developer.apple.com/download/applications/)** or later

### Setup Instructions

#### Step 1: Access Intelligence Settings

Navigate to **Settings > Intelligence > Add a Model Provider** in your macOS system preferences.

![Xcode Intelligence Settings](file:53a72695-7072-4880-8936-eb1ad5c0688f)

#### Step 2: Configure OpenRouter Provider

In the “Add a Model Provider” dialog, enter the following details:

* **URL**: `https://openrouter.ai/api`
  + **Important**: Do not add `/v1` at the end of the endpoint like you typically would for direct API calls
* **API Key Header**: `api_key`
* **API Key**: Your OpenRouter API key (starts with `sk-or-v1-`)
* **Description**: `OpenRouter` (or any name you prefer)

Click **Add** to save the configuration.

![OpenRouter Configuration](file:a8bb5aa6-a800-44ae-8cbe-5672feeea7f1)

#### Step 3: Browse Available Models

Once configured, click on **OpenRouter** to see all available models. Since OpenRouter offers hundreds of models, you should bookmark your favorite models for quick access. Bookmarked models will appear at the top of the list, making them easily accessible from within the pane whenever you need them.

![Available Models](file:206f880a-a70f-4524-bdac-6e3d038f811d)

You’ll have access to models from various providers including:

* Anthropic Claude models
* Google Gemini models
* Meta Llama models
* OpenAI GPT models
* And hundreds more

![Extended Model List](file:0bbc4049-ee3b-4ff1-8015-d6b538ad1294)

#### Step 4: Start Using AI in Xcode

Head back to the chat interface (icon at the top) and start chatting with your selected models directly in Xcode.

![Xcode Chat Interface](file:f96568f3-fb4e-46b4-a601-7001462215d0)

### Using Apple Intelligence Features

Once configured, you can use Apple Intelligence features in Xcode with OpenRouter models:

* **Code Completion**: Get intelligent code suggestions
* **Code Explanation**: Ask questions about your code
* **Refactoring Assistance**: Get help improving your code structure
* **Documentation Generation**: Generate comments and documentation

![Apple Intelligence Interface](file:e7750c63-ac3f-4be1-9c04-4333239b9d1f)

*Image credit: [Apple Developer Documentation](https://developer.apple.com/documentation/Xcode/writing-code-with-intelligence-in-xcode)*

### Learn More

* **Apple Intelligence Documentation**: [Writing Code with Intelligence in Xcode](https://developer.apple.com/documentation/Xcode/writing-code-with-intelligence-in-xcode)
* **OpenRouter Quick Start**: [Getting Started with OpenRouter](https://openrouter.ai/docs/quickstart)
* **Available Models**: [Browse OpenRouter Models](https://openrouter.ai/models)

Was this page helpful?

YesNo

[Previous](/docs/community/vercel-ai-sdk)[#### Zapier

Build AI automations with OpenRouter & Zapier

Next](/docs/community/zapier)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T15:30:01.083Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T15:30:01.083Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
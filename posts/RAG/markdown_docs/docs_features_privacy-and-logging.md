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

* [Within OpenRouter](#within-openrouter)
* [Provider Policies](#provider-policies)
* [Training on Prompts](#training-on-prompts)
* [Data Retention & Logging](#data-retention--logging)
* [Enterprise EU in-region routing](#enterprise-eu-in-region-routing)

[Features](/docs/features/privacy-and-logging)

# Privacy, Logging, and Data Collection

Copy page

Making sure your data is safe

When using AI through OpenRouter, whether via the chat interface or the API, your prompts and responses go through multiple touchpoints. You have control over how your data is handled at each step.

This page is designed to give a practical overview of how your data is handled, stored, and used. More information is available in the [privacy policy](/privacy) and [terms of service](/terms).

## Within OpenRouter

OpenRouter does not store your prompts or responses, *unless* you have explicitly opted in to prompt logging in your account settings. It’s as simple as that.

OpenRouter samples a small number of prompts for categorization to power our reporting and model ranking. If you are not opted in to prompt logging, any categorization of your prompts is stored completely anonymously and never associated with your account or user ID. The categorization is done by model with a zero-data-retention policy.

OpenRouter does store metadata (e.g. number of prompt and completion tokens, latency, etc) for each request. This is used to power our reporting and model ranking, and your [activity feed](/activity).

## Provider Policies

### Training on Prompts

Each provider on OpenRouter has its own data handling policies. We reflect those policies in structured data on each AI endpoint that we offer.

On your account settings page, you can set whether you would like to allow routing to providers that may train on your data (according to their own policies). There are separate settings for paid and free models.

Wherever possible, OpenRouter works with providers to ensure that prompts will not be trained on, but there are exceptions. If you opt out of training in your account settings, OpenRouter will not route to providers that train. This setting has no bearing on OpenRouter’s own policies and what we do with your prompts.

##### Data Policy Filtering

You can [restrict individual requests](/docs/features/provider-routing#requiring-providers-to-comply-with-data-policies)
to only use providers with a certain data policy.

This is also available as an account-wide setting in [your privacy settings](https://openrouter.ai/settings/privacy).

### Data Retention & Logging

Providers also have their own data retention policies, often for compliance reasons. OpenRouter does not have routing rules that change based on data retention policies of providers, but the retention policies as reflected in each provider’s terms are shown below. Any user of OpenRouter can ignore providers that don’t meet their own data retention requirements.

The full terms of service for each provider are linked from the provider’s page, and aggregated in the [documentation](/docs/features/provider-routing#terms-of-service).

| Provider | Data Retention | Train on Prompts |
| --- | --- | --- |

## Enterprise EU in-region routing

For enterprise customers, OpenRouter supports EU in-region routing. When enabled for your account, your prompts and completions are processed within the European Union and do not leave the EU. Use the base URL [http://eu.openrouter.ai](http://eu.openrouter.ai/) for API requests to keep traffic and data within Europe. This feature is only enabled for enterprise customers by request.

If you’re interested, please contact our enterprise team at <https://openrouter.ai/enterprise/form>.

Was this page helpful?

YesNo

[Previous](/docs/overview/models)[#### Zero Data Retention

How OpenRouter gives you control over your data

Next](/docs/features/zdr)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
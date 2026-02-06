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

* [How OpenRouter Manages Data Policies](#how-openrouter-manages-data-policies)
* [Per-Request ZDR Enforcement](#per-request-zdr-enforcement)
* [Usage](#usage)
* [Caching](#caching)
* [OpenRouter’s Retention Policy](#openrouters-retention-policy)
* [Zero Retention Endpoints](#zero-retention-endpoints)

[Features](/docs/features/privacy-and-logging)

# Zero Data Retention

Copy page

How OpenRouter gives you control over your data

Zero Data Retention (ZDR) means that a provider will not store your data for any period of time.

OpenRouter has a [setting](/settings/privacy) that, when enabled, only allows you to route to endpoints that have a Zero Data Retention policy.

Providers that do not retain your data are also unable to train on your data. However we do have some endpoints & providers who do not train on your data but *do* retain it (e.g. to scan for abuse or for legal reasons). OpenRouter gives you controls over both of these policies.

## How OpenRouter Manages Data Policies

OpenRouter works with providers to understand each of their data policies and structures the policy data in a way that gives you control over which providers you want to route to.

Note that a provider’s general policy may differ from the specific policy for a given endpoint. OpenRouter keeps track of the specific policy for each endpoint, works with providers to keep these policies up to date, and in some cases creates special agreements with providers to ensure data retention or training policies that are more privacy-focused than their default policies.

#####

If OpenRouter is not able to establish or ascertain a clear policy for a provider or endpoint, we take a conservative stance and assume that the endpoint both retains and trains on data and mark it as such.

A full list of providers and their data policies can be found [here](/docs/features/privacy-and-logging#data-retention--logging). Note that this list shows the default policy for each provider; if there is a particular endpoint that has a policy that differs from the provider default, it may not be available if “ZDR Only” is enabled.

## Per-Request ZDR Enforcement

In addition to the global ZDR setting in your [privacy settings](/settings/privacy), you can enforce Zero Data Retention on a per-request basis using the `zdr` parameter in your API calls.

The request-level `zdr` parameter operates as an “OR” with your account-wide ZDR setting - if either is enabled, ZDR enforcement will be applied. This means the per-request parameter can only be used to ensure ZDR is enabled for a specific request, not to override or disable account-wide ZDR enforcement.

This is useful for customers who don’t want to globally enforce ZDR but need to ensure specific requests only route to ZDR endpoints.

### Usage

Include the `zdr` parameter in your provider preferences:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "gpt-4", |
| 3 | "messages": [...], |
| 4 | "provider": { |
| 5 | "zdr": true |
| 6 | } |
| 7 | } |
```

When `zdr` is set to `true`, the request will only be routed to endpoints that have a Zero Data Retention policy. When `zdr` is `false` or not provided, ZDR enforcement will still apply if enabled in your account settings.

## Caching

Some endpoints/models provide implicit caching of prompts. This keeps repeated prompt data in an in-memory cache in the provider’s datacenter, so that the repeated part of the prompt does not need to be re-processed. This can lead to considerable cost savings.

OpenRouter has taken the stance that in-memory caching of prompts is *not* considered “retaining” data, and we therefore allow endpoints/models with implicit caching to be hit when a ZDR routing policy is in effect.

## OpenRouter’s Retention Policy

OpenRouter itself has a ZDR policy; your prompts are not retained unless you specifically opt in to prompt logging.

## Zero Retention Endpoints

The following endpoints have a ZDR policy. Note that this list is also available progammatically via <https://openrouter.ai/api/v1/endpoints/zdr>. It is automatically updated when there are changes to a provider’s data policy.:

| Model | Provider | Implicit Caching |
| --- | --- | --- |

Was this page helpful?

YesNo

[Previous](/docs/features/privacy-and-logging)[#### Model Routing

Dynamically route requests to models

Next](/docs/features/model-routing)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
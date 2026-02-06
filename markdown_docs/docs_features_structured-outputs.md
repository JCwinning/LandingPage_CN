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

* [Overview](#overview)
* [Using Structured Outputs](#using-structured-outputs)
* [Model Support](#model-support)
* [Best Practices](#best-practices)
* [Example Implementation](#example-implementation)
* [Streaming with Structured Outputs](#streaming-with-structured-outputs)
* [Error Handling](#error-handling)

[Features](/docs/features/privacy-and-logging)

# Structured Outputs

Copy page

Return structured data from your models

OpenRouter supports structured outputs for compatible models, ensuring responses follow a specific JSON Schema format. This feature is particularly useful when you need consistent, well-formatted responses that can be reliably parsed by your application.

## Overview

Structured outputs allow you to:

* Enforce specific JSON Schema validation on model responses
* Get consistent, type-safe outputs
* Avoid parsing errors and hallucinated fields
* Simplify response handling in your application

## Using Structured Outputs

To use structured outputs, include a `response_format` parameter in your request, with `type` set to `json_schema` and the `json_schema` object containing your schema:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "messages": [ |
| 3 | { "role": "user", "content": "What's the weather like in London?" } |
| 4 | ], |
| 5 | "response_format": { |
| 6 | "type": "json_schema", |
| 7 | "json_schema": { |
| 8 | "name": "weather", |
| 9 | "strict": true, |
| 10 | "schema": { |
| 11 | "type": "object", |
| 12 | "properties": { |
| 13 | "location": { |
| 14 | "type": "string", |
| 15 | "description": "City or location name" |
| 16 | }, |
| 17 | "temperature": { |
| 18 | "type": "number", |
| 19 | "description": "Temperature in Celsius" |
| 20 | }, |
| 21 | "conditions": { |
| 22 | "type": "string", |
| 23 | "description": "Weather conditions description" |
| 24 | } |
| 25 | }, |
| 26 | "required": ["location", "temperature", "conditions"], |
| 27 | "additionalProperties": false |
| 28 | } |
| 29 | } |
| 30 | } |
| 31 | } |
```

The model will respond with a JSON object that strictly follows your schema:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "location": "London", |
| 3 | "temperature": 18, |
| 4 | "conditions": "Partly cloudy with light drizzle" |
| 5 | } |
```

## Model Support

Structured outputs are supported by select models.

You can find a list of models that support structured outputs on the [models page](https://openrouter.ai/models?order=newest&supported_parameters=structured_outputs).

* OpenAI models (GPT-4o and later versions) [Docs](https://platform.openai.com/docs/guides/structured-outputs)
* Google Gemini models [Docs](https://ai.google.dev/gemini-api/docs/structured-output)
* Anthropic models (Sonnet 4.5 and Opus 4.1) [Docs](https://docs.claude.com/en/docs/build-with-claude/structured-outputs)
* Most open-source models
* All Fireworks provided models [Docs](https://docs.fireworks.ai/structured-responses/structured-response-formatting#structured-response-modes)

To ensure your chosen model supports structured outputs:

1. Check the model’s supported parameters on the [models page](https://openrouter.ai/models)
2. Set `require_parameters: true` in your provider preferences (see [Provider Routing](/docs/features/provider-routing))
3. Include `response_format` and set `type: json_schema` in the required parameters

## Best Practices

1. **Include descriptions**: Add clear descriptions to your schema properties to guide the model
2. **Use strict mode**: Always set `strict: true` to ensure the model follows your schema exactly

## Example Implementation

Here’s a complete example using the Fetch API:

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
| 7 | const response = await openRouter.chat.send({ |
| 8 | model: '{{MODEL}}', |
| 9 | messages: [ |
| 10 | { role: 'user', content: 'What is the weather like in London?' }, |
| 11 | ], |
| 12 | responseFormat: { |
| 13 | type: 'json_schema', |
| 14 | jsonSchema: { |
| 15 | name: 'weather', |
| 16 | strict: true, |
| 17 | schema: { |
| 18 | type: 'object', |
| 19 | properties: { |
| 20 | location: { |
| 21 | type: 'string', |
| 22 | description: 'City or location name', |
| 23 | }, |
| 24 | temperature: { |
| 25 | type: 'number', |
| 26 | description: 'Temperature in Celsius', |
| 27 | }, |
| 28 | conditions: { |
| 29 | type: 'string', |
| 30 | description: 'Weather conditions description', |
| 31 | }, |
| 32 | }, |
| 33 | required: ['location', 'temperature', 'conditions'], |
| 34 | additionalProperties: false, |
| 35 | }, |
| 36 | }, |
| 37 | }, |
| 38 | stream: false, |
| 39 | }); |
| 40 |  |
| 41 | const weatherInfo = response.choices[0].message.content; |
```

## Streaming with Structured Outputs

Structured outputs are also supported with streaming responses. The model will stream valid partial JSON that, when complete, forms a valid response matching your schema.

To enable streaming with structured outputs, simply add `stream: true` to your request:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "stream": true, |
| 3 | "response_format": { |
| 4 | "type": "json_schema", |
| 5 | // ... rest of your schema |
| 6 | } |
| 7 | } |
```

## Error Handling

When using structured outputs, you may encounter these scenarios:

1. **Model doesn’t support structured outputs**: The request will fail with an error indicating lack of support
2. **Invalid schema**: The model will return an error if your JSON Schema is invalid

Was this page helpful?

YesNo

[Previous](/docs/features/prompt-caching)[#### Tool & Function Calling

Use tools in your prompts

Next](/docs/features/tool-calling)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
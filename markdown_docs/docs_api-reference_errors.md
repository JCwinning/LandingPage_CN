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

* [Error Codes](#error-codes)
* [Moderation Errors](#moderation-errors)
* [Provider Errors](#provider-errors)
* [When No Content is Generated](#when-no-content-is-generated)
* [Streaming Error Formats](#streaming-error-formats)
* [Pre-Stream Errors](#pre-stream-errors)
* [Mid-Stream Errors](#mid-stream-errors)
* [OpenAI Responses API Error Events](#openai-responses-api-error-events)
* [Error Event Types](#error-event-types)
* [Error Code Transformations](#error-code-transformations)
* [API-Specific Error Handling](#api-specific-error-handling)
* [OpenAI Chat Completions API (/api/v1/chat/completions)](#openai-chat-completions-api-apiv1chatcompletions)
* [OpenAI Responses API (/api/alpha/responses)](#openai-responses-api-apialpharesponses)
* [Error Response Type Definitions](#error-response-type-definitions)

[API Reference](/docs/api-reference/overview)

# Errors

Copy page

API Errors

For errors, OpenRouter returns a JSON response with the following shape:

```
|  |  |
| --- | --- |
| 1 | type ErrorResponse = { |
| 2 | error: { |
| 3 | code: number; |
| 4 | message: string; |
| 5 | metadata?: Record<string, unknown>; |
| 6 | }; |
| 7 | }; |
```

The HTTP Response will have the same status code as `error.code`, forming a request error if:

* Your original request is invalid
* Your API key/account is out of credits

Otherwise, the returned HTTP response status will be `200` and any error occurred while the LLM is producing the output will be emitted in the response body or as an SSE data event.

Example code for printing errors in JavaScript:

```
|  |  |
| --- | --- |
| 1 | const request = await fetch('https://openrouter.ai/...'); |
| 2 | console.log(request.status); // Will be an error code unless the model started processing your request |
| 3 | const response = await request.json(); |
| 4 | console.error(response.error?.status); // Will be an error code |
| 5 | console.error(response.error?.message); |
```

## Error Codes

* **400**: Bad Request (invalid or missing params, CORS)
* **401**: Invalid credentials (OAuth session expired, disabled/invalid API key)
* **402**: Your account or API key has insufficient credits. Add more credits and retry the request.
* **403**: Your chosen model requires moderation and your input was flagged
* **408**: Your request timed out
* **429**: You are being rate limited
* **502**: Your chosen model is down or we received an invalid response from it
* **503**: There is no available model provider that meets your routing requirements

## Moderation Errors

If your input was flagged, the `error.metadata` will contain information about the issue. The shape of the metadata is as follows:

```
|  |  |
| --- | --- |
| 1 | type ModerationErrorMetadata = { |
| 2 | reasons: string[]; // Why your input was flagged |
| 3 | flagged_input: string; // The text segment that was flagged, limited to 100 characters. If the flagged input is longer than 100 characters, it will be truncated in the middle and replaced with ... |
| 4 | provider_name: string; // The name of the provider that requested moderation |
| 5 | model_slug: string; |
| 6 | }; |
```

## Provider Errors

If the model provider encounters an error, the `error.metadata` will contain information about the issue. The shape of the metadata is as follows:

```
|  |  |
| --- | --- |
| 1 | type ProviderErrorMetadata = { |
| 2 | provider_name: string; // The name of the provider that encountered the error |
| 3 | raw: unknown; // The raw error from the provider |
| 4 | }; |
```

## When No Content is Generated

Occasionally, the model may not generate any content. This typically occurs when:

* The model is warming up from a cold start
* The system is scaling up to handle more requests

Warm-up times usually range from a few seconds to a few minutes, depending on the model and provider.

If you encounter persistent no-content issues, consider implementing a simple retry mechanism or trying again with a different provider or model that has more recent activity.

Additionally, be aware that in some cases, you may still be charged for the prompt processing cost by the upstream provider, even if no content is generated.

## Streaming Error Formats

When using streaming mode (`stream: true`), errors are handled differently depending on when they occur:

### Pre-Stream Errors

Errors that occur before any tokens are sent follow the standard error format above, with appropriate HTTP status codes.

### Mid-Stream Errors

Errors that occur after streaming has begun are sent as Server-Sent Events (SSE) with a unified structure that includes both the error details and a completion choice:

```
|  |  |
| --- | --- |
| 1 | type MidStreamError = { |
| 2 | id: string; |
| 3 | object: 'chat.completion.chunk'; |
| 4 | created: number; |
| 5 | model: string; |
| 6 | provider: string; |
| 7 | error: { |
| 8 | code: string | number; |
| 9 | message: string; |
| 10 | }; |
| 11 | choices: [{ |
| 12 | index: 0; |
| 13 | delta: { content: '' }; |
| 14 | finish_reason: 'error'; |
| 15 | native_finish_reason?: string; |
| 16 | }]; |
| 17 | }; |
```

Example SSE data:

```
|  |
| --- |
| data: {"id":"cmpl-abc123","object":"chat.completion.chunk","created":1234567890,"model":"gpt-3.5-turbo","provider":"openai","error":{"code":"server_error","message":"Provider disconnected"},"choices":[{"index":0,"delta":{"content":""},"finish_reason":"error"}]} |
```

Key characteristics:

* The error appears at the **top level** alongside standard response fields
* A `choices` array is included with `finish_reason: "error"` to properly terminate the stream
* The HTTP status remains 200 OK since headers were already sent
* The stream is terminated after this event

## OpenAI Responses API Error Events

The OpenAI Responses API (`/api/alpha/responses`) uses specific event types for streaming errors:

### Error Event Types

1. **`response.failed`** - Official failure event

   ```
   |  |  |
   | --- | --- |
   | 1 | { |
   | 2 | "type": "response.failed", |
   | 3 | "response": { |
   | 4 | "id": "resp_abc123", |
   | 5 | "status": "failed", |
   | 6 | "error": { |
   | 7 | "code": "server_error", |
   | 8 | "message": "Internal server error" |
   | 9 | } |
   | 10 | } |
   | 11 | } |
   ```
2. **`response.error`** - Error during response generation

   ```
   |  |  |
   | --- | --- |
   | 1 | { |
   | 2 | "type": "response.error", |
   | 3 | "error": { |
   | 4 | "code": "rate_limit_exceeded", |
   | 5 | "message": "Rate limit exceeded" |
   | 6 | } |
   | 7 | } |
   ```
3. **`error`** - Plain error event (undocumented but sent by OpenAI)

   ```
   |  |  |
   | --- | --- |
   | 1 | { |
   | 2 | "type": "error", |
   | 3 | "error": { |
   | 4 | "code": "invalid_api_key", |
   | 5 | "message": "Invalid API key provided" |
   | 6 | } |
   | 7 | } |
   ```

### Error Code Transformations

The Responses API transforms certain error codes into successful completions with specific finish reasons:

| Error Code | Transformed To | Finish Reason |
| --- | --- | --- |
| `context_length_exceeded` | Success | `length` |
| `max_tokens_exceeded` | Success | `length` |
| `token_limit_exceeded` | Success | `length` |
| `string_too_long` | Success | `length` |

This allows for graceful handling of limit-based errors without treating them as failures.

## API-Specific Error Handling

Different OpenRouter API endpoints handle errors in distinct ways:

### OpenAI Chat Completions API (`/api/v1/chat/completions`)

* **No tokens sent**: Returns standalone `ErrorResponse`
* **Some tokens sent**: Embeds error information within the `choices` array of the final response
* **Streaming**: Errors sent as SSE events with top-level error field

### OpenAI Responses API (`/api/alpha/responses`)

* **Error transformations**: Certain errors become successful responses with appropriate finish reasons
* **Streaming events**: Uses typed events (`response.failed`, `response.error`, `error`)
* **Graceful degradation**: Handles provider-specific errors with fallback behavior

### Error Response Type Definitions

```
|  |  |
| --- | --- |
| 1 | // Standard error response |
| 2 | interface ErrorResponse { |
| 3 | error: { |
| 4 | code: number; |
| 5 | message: string; |
| 6 | metadata?: Record<string, unknown>; |
| 7 | }; |
| 8 | } |
| 9 |  |
| 10 | // Mid-stream error with completion data |
| 11 | interface StreamErrorChunk { |
| 12 | error: { |
| 13 | code: string | number; |
| 14 | message: string; |
| 15 | }; |
| 16 | choices: Array<{ |
| 17 | delta: { content: string }; |
| 18 | finish_reason: 'error'; |
| 19 | native_finish_reason: string; |
| 20 | }>; |
| 21 | } |
| 22 |  |
| 23 | // Responses API error event |
| 24 | interface ResponsesAPIErrorEvent { |
| 25 | type: 'response.failed' | 'response.error' | 'error'; |
| 26 | error?: { |
| 27 | code: string; |
| 28 | message: string; |
| 29 | }; |
| 30 | response?: { |
| 31 | id: string; |
| 32 | status: 'failed'; |
| 33 | error: { |
| 34 | code: string; |
| 35 | message: string; |
| 36 | }; |
| 37 | }; |
| 38 | } |
```

Was this page helpful?

YesNo

[Previous](/docs/api-reference/parameters)[#### Responses API Beta

OpenAI-compatible Responses API (Beta)

Next](/docs/api-reference/responses-api/overview)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
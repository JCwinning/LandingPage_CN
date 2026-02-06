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

* [PKCE Guide](#pkce-guide)
* [Step 1: Send your user to OpenRouter](#step-1-send-your-user-to-openrouter)
* [How to Generate a Code Challenge](#how-to-generate-a-code-challenge)
* [Localhost Apps](#localhost-apps)
* [Step 2: Exchange the code for a user-controlled API key](#step-2-exchange-the-code-for-a-user-controlled-api-key)
* [Step 3: Use the API key](#step-3-use-the-api-key)
* [Error Codes](#error-codes)
* [External Tools](#external-tools)

[Use Cases](/docs/use-cases/byok)

# OAuth PKCE

Copy page

Connect your users to OpenRouter

Users can connect to OpenRouter in one click using [Proof Key for Code Exchange (PKCE)](https://oauth.net/2/pkce/).

Here’s a step-by-step guide:

## PKCE Guide

### Step 1: Send your user to OpenRouter

To start the PKCE flow, send your user to OpenRouter’s `/auth` URL with a `callback_url` parameter pointing back to your site:

With S256 Code Challenge (Recommended)With Plain Code ChallengeWithout Code Challenge

```
|  |
| --- |
| https://openrouter.ai/auth?callback_url=<YOUR_SITE_URL>&code_challenge=<CODE_CHALLENGE>&code_challenge_method=S256 |
```

The `code_challenge` parameter is optional but recommended.

Your user will be prompted to log in to OpenRouter and authorize your app. After authorization, they will be redirected back to your site with a `code` parameter in the URL:

![Alt text](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/pages/use-cases/auth-request.png)

##### Use SHA-256 for Maximum Security

For maximum security, set `code_challenge_method` to `S256`, and set `code_challenge` to the base64 encoding of the sha256 hash of `code_verifier`.

For more info, [visit Auth0’s docs](https://auth0.com/docs/get-started/authentication-and-authorization-flow/call-your-api-using-the-authorization-code-flow-with-pkce#parameters).

#### How to Generate a Code Challenge

The following example leverages the Web Crypto API and the Buffer API to generate a code challenge for the S256 method. You will need a bundler to use the Buffer API in the web browser:

Generate Code Challenge

```
|  |  |
| --- | --- |
| 1 | import { Buffer } from 'buffer'; |
| 2 |  |
| 3 | async function createSHA256CodeChallenge(input: string) { |
| 4 | const encoder = new TextEncoder(); |
| 5 | const data = encoder.encode(input); |
| 6 | const hash = await crypto.subtle.digest('SHA-256', data); |
| 7 | return Buffer.from(hash).toString('base64url'); |
| 8 | } |
| 9 |  |
| 10 | const codeVerifier = 'your-random-string'; |
| 11 | const generatedCodeChallenge = await createSHA256CodeChallenge(codeVerifier); |
```

#### Localhost Apps

If your app is a local-first app or otherwise doesn’t have a public URL, it is recommended to test with `http://localhost:3000` as the callback and referrer URLs.

When moving to production, replace the localhost/private referrer URL with a public GitHub repo or a link to your project website.

### Step 2: Exchange the code for a user-controlled API key

After the user logs in with OpenRouter, they are redirected back to your site with a `code` parameter in the URL:

![Alt text](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/pages/use-cases/code-challenge.png)

Extract this code using the browser API:

Extract Code

```
|  |  |
| --- | --- |
| 1 | const urlParams = new URLSearchParams(window.location.search); |
| 2 | const code = urlParams.get('code'); |
```

Then use it to make an API call to `https://openrouter.ai/api/v1/auth/keys` to exchange the code for a user-controlled API key:

Exchange Code

```
|  |  |
| --- | --- |
| 1 | const response = await fetch('https://openrouter.ai/api/v1/auth/keys', { |
| 2 | method: 'POST', |
| 3 | headers: { |
| 4 | 'Content-Type': 'application/json', |
| 5 | }, |
| 6 | body: JSON.stringify({ |
| 7 | code: '<CODE_FROM_QUERY_PARAM>', |
| 8 | code_verifier: '<CODE_VERIFIER>', // If code_challenge was used |
| 9 | code_challenge_method: '<CODE_CHALLENGE_METHOD>', // If code_challenge was used |
| 10 | }), |
| 11 | }); |
| 12 |  |
| 13 | const { key } = await response.json(); |
```

And that’s it for the PKCE flow!

### Step 3: Use the API key

Store the API key securely within the user’s browser or in your own database, and use it to [make OpenRouter requests](/api-reference/completion).

TypeScript SDKTypeScript (fetch)

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: key, // The key from Step 2 |
| 5 | }); |
| 6 |  |
| 7 | const completion = await openRouter.chat.send({ |
| 8 | model: 'openai/gpt-4o', |
| 9 | messages: [ |
| 10 | { |
| 11 | role: 'user', |
| 12 | content: 'Hello!', |
| 13 | }, |
| 14 | ], |
| 15 | stream: false, |
| 16 | }); |
| 17 |  |
| 18 | console.log(completion.choices[0].message); |
```

## Error Codes

* `400 Invalid code_challenge_method`: Make sure you’re using the same code challenge method in step 1 as in step 2.
* `403 Invalid code or code_verifier`: Make sure your user is logged in to OpenRouter, and that `code_verifier` and `code_challenge_method` are correct.
* `405 Method Not Allowed`: Make sure you’re using `POST` and `HTTPS` for your request.

## External Tools

* [PKCE Tools](https://example-app.com/pkce)
* [Online PKCE Generator](https://tonyxu-io.github.io/pkce-generator/)

Was this page helpful?

YesNo

[Previous](/docs/use-cases/crypto-api)[#### Using MCP Servers with OpenRouter

Use MCP Servers with OpenRouter

Next](/docs/use-cases/mcp-servers)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
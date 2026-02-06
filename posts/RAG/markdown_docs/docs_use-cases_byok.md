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

* [Bring your own API Keys](#bring-your-own-api-keys)
* [Key Priority and Fallback](#key-priority-and-fallback)
* [Azure API Keys](#azure-api-keys)
* [AWS Bedrock API Keys](#aws-bedrock-api-keys)
* [Option 1: Bedrock API Keys (Recommended)](#option-1-bedrock-api-keys-recommended)
* [Option 2: AWS Credentials](#option-2-aws-credentials)
* [Google Vertex API Keys](#google-vertex-api-keys)

[Use Cases](/docs/use-cases/byok)

# BYOK

Copy page

Bring your own provider API keys

## Bring your own API Keys

OpenRouter supports both OpenRouter credits and the option to bring your own provider keys (BYOK).

When you use OpenRouter credits, your rate limits for each provider are managed by OpenRouter.

Using provider keys enables direct control over rate limits and costs via your provider account.

Your provider keys are securely encrypted and used for all requests routed through the specified provider.

Manage keys in your [account settings](/settings/integrations).

The cost of using custom provider keys on OpenRouter is **5% of what the same model/provider would cost normally on OpenRouter** and will be deducted from your OpenRouter credits.
This fee is waived for the first 1M BYOK requests per-month.

### Key Priority and Fallback

OpenRouter always prioritizes using your provider keys when available. By default, if your key encounters a rate limit or failure, OpenRouter will fall back to using shared OpenRouter credits.

You can configure individual keys with “Always use this key” to prevent any fallback to OpenRouter credits. When this option is enabled, OpenRouter will only use your key for requests to that provider, which may result in rate limit errors if your key is exhausted, but ensures all requests go through your account.

### Azure API Keys

To use Azure AI Services with OpenRouter, you’ll need to provide your Azure API key configuration in JSON format. Each key configuration requires the following fields:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model_slug": "the-openrouter-model-slug", |
| 3 | "endpoint_url": "https://<resource>.services.ai.azure.com/deployments/<model-id>/chat/completions?api-version=<api-version>", |
| 4 | "api_key": "your-azure-api-key", |
| 5 | "model_id": "the-azure-model-id" |
| 6 | } |
```

You can find these values in your Azure AI Services resource:

1. **endpoint\_url**: Navigate to your Azure AI Services resource in the Azure portal. In the “Overview” section, you’ll find your endpoint URL. Make sure to append `/chat/completions` to the base URL. You can read more in the [Azure Foundry documentation](https://learn.microsoft.com/en-us/azure/ai-foundry/model-inference/concepts/endpoints?tabs=python).
2. **api\_key**: In the same “Overview” section of your Azure AI Services resource, you can find your API key under “Keys and Endpoint”.
3. **model\_id**: This is the name of your model deployment in Azure AI Services.
4. **model\_slug**: This is the OpenRouter model identifier you want to use this key for.

Since Azure supports multiple model deployments, you can provide an array of configurations for different models:

```
|  |  |
| --- | --- |
| 1 | [ |
| 2 | { |
| 3 | "model_slug": "mistralai/mistral-large", |
| 4 | "endpoint_url": "https://example-project.openai.azure.com/openai/deployments/mistral-large/chat/completions?api-version=2024-08-01-preview", |
| 5 | "api_key": "your-azure-api-key", |
| 6 | "model_id": "mistral-large" |
| 7 | }, |
| 8 | { |
| 9 | "model_slug": "openai/gpt-4o", |
| 10 | "endpoint_url": "https://example-project.openai.azure.com/openai/deployments/gpt-4o/chat/completions?api-version=2024-08-01-preview", |
| 11 | "api_key": "your-azure-api-key", |
| 12 | "model_id": "gpt-4o" |
| 13 | } |
| 14 | ] |
```

Make sure to replace the url with your own project url. Also the url should end with /chat/completions with the api version that you would like to use.

### AWS Bedrock API Keys

To use Amazon Bedrock with OpenRouter, you can authenticate using either Bedrock API keys or traditional AWS credentials.

#### Option 1: Bedrock API Keys (Recommended)

Amazon Bedrock API keys provide a simpler authentication method. Simply provide your Bedrock API key as a string:

```
|  |
| --- |
| your-bedrock-api-key-here |
```

**Note:** Bedrock API keys are tied to a specific AWS region and cannot be used to change regions. If you need to use models in different regions, use the AWS credentials option below.

You can generate Bedrock API keys in the AWS Management Console. Learn more in the [Amazon Bedrock API keys documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/api-keys.html).

#### Option 2: AWS Credentials

Alternatively, you can use traditional AWS credentials in JSON format. This option allows you to specify the region and provides more flexibility:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "accessKeyId": "your-aws-access-key-id", |
| 3 | "secretAccessKey": "your-aws-secret-access-key", |
| 4 | "region": "your-aws-region" |
| 5 | } |
```

You can find these values in your AWS account:

1. **accessKeyId**: This is your AWS Access Key ID. You can create or find your access keys in the AWS Management Console under “Security Credentials” in your AWS account.
2. **secretAccessKey**: This is your AWS Secret Access Key, which is provided when you create an access key.
3. **region**: The AWS region where your Amazon Bedrock models are deployed (e.g., “us-east-1”, “us-west-2”).

Make sure your AWS IAM user or role has the necessary permissions to access Amazon Bedrock services. At minimum, you’ll need permissions for:

* `bedrock:InvokeModel`
* `bedrock:InvokeModelWithResponseStream` (for streaming responses)

Example IAM policy:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "Version": "2012-10-17", |
| 3 | "Statement": [ |
| 4 | { |
| 5 | "Effect": "Allow", |
| 6 | "Action": [ |
| 7 | "bedrock:InvokeModel", |
| 8 | "bedrock:InvokeModelWithResponseStream" |
| 9 | ], |
| 10 | "Resource": "*" |
| 11 | } |
| 12 | ] |
| 13 | } |
```

For enhanced security, we recommend creating dedicated IAM users with limited permissions specifically for use with OpenRouter.

Learn more in the [AWS Bedrock Getting Started with the API](https://docs.aws.amazon.com/bedrock/latest/userguide/getting-started-api.html) documentation, [IAM Permissions Setup](https://docs.aws.amazon.com/bedrock/latest/userguide/security-iam.html) guide, or the [AWS Bedrock API Reference](https://docs.aws.amazon.com/bedrock/latest/APIReference/welcome.html).

### Google Vertex API Keys

To use Google Vertex AI with OpenRouter, you’ll need to provide your Google Cloud service account key in JSON format. The service account key should include all standard Google Cloud service account fields, with an optional `region` field for specifying the deployment region.

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "type": "service_account", |
| 3 | "project_id": "your-project-id", |
| 4 | "private_key_id": "your-private-key-id", |
| 5 | "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n", |
| 6 | "client_email": "[email protected]", |
| 7 | "client_id": "your-client-id", |
| 8 | "auth_uri": "https://accounts.google.com/o/oauth2/auth", |
| 9 | "token_uri": "https://oauth2.googleapis.com/token", |
| 10 | "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs", |
| 11 | "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/[email protected]", |
| 12 | "universe_domain": "googleapis.com", |
| 13 | "region": "global" |
| 14 | } |
```

You can find these values in your Google Cloud Console:

1. **Service Account Key**: Navigate to the Google Cloud Console, go to “IAM & Admin” > “Service Accounts”, select your service account, and create/download a JSON key.
2. **region** (optional): Specify the region for your Vertex AI deployment. Use `"global"` to allow requests to run in any available region, or specify a specific region like `"us-central1"` or `"europe-west1"`.

Make sure your service account has the necessary permissions to access Vertex AI services:

* `aiplatform.endpoints.predict`
* `aiplatform.endpoints.streamingPredict` (for streaming responses)

Example IAM policy:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "bindings": [ |
| 3 | { |
| 4 | "role": "roles/aiplatform.user", |
| 5 | "members": [ |
| 6 | "serviceAccount:[email protected]" |
| 7 | ] |
| 8 | } |
| 9 | ] |
| 10 | } |
```

Learn more in the [Google Cloud Vertex AI documentation](https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform) and [Service Account setup guide](https://cloud.google.com/iam/docs/service-accounts-create).

Was this page helpful?

YesNo

[Previous](/docs/sdks/typescript/responses)[#### Crypto API

Purchase credits with crypto

Next](/docs/use-cases/crypto-api)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
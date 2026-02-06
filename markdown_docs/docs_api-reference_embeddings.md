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

* [What are Embeddings?](#what-are-embeddings)
* [Common Use Cases](#common-use-cases)
* [How to Use Embeddings](#how-to-use-embeddings)
* [Basic Request](#basic-request)
* [Batch Processing](#batch-processing)
* [API Reference](#api-reference)
* [Available Models](#available-models)
* [Practical Example: Semantic Search](#practical-example-semantic-search)
* [Best Practices](#best-practices)
* [Provider Routing](#provider-routing)
* [Error Handling](#error-handling)
* [Limitations](#limitations)
* [Related Resources](#related-resources)

[API Reference](/docs/api-reference/overview)

# Embeddings

Copy page

Generate vector embeddings from text

Embeddings are numerical representations of text that capture semantic meaning. They convert text into vectors (arrays of numbers) that can be used for various machine learning tasks. OpenRouter provides a unified API to access embedding models from multiple providers.

## What are Embeddings?

Embeddings transform text into high-dimensional vectors where semantically similar texts are positioned closer together in vector space. For example, “cat” and “kitten” would have similar embeddings, while “cat” and “airplane” would be far apart.

These vector representations enable machines to understand relationships between pieces of text, making them essential for many AI applications.

## Common Use Cases

Embeddings are used in a wide variety of applications:

**RAG (Retrieval-Augmented Generation)**: Build RAG systems that retrieve relevant context from a knowledge base before generating answers. Embeddings help find the most relevant documents to include in the LLM’s context.

**Semantic Search**: Convert documents and queries into embeddings, then find the most relevant documents by comparing vector similarity. This provides more accurate results than traditional keyword matching because it understands meaning rather than just matching words.

**Recommendation Systems**: Generate embeddings for items (products, articles, movies) and user preferences to recommend similar items. By comparing embedding vectors, you can find items that are semantically related even if they don’t share obvious keywords.

**Clustering and Classification**: Group similar documents together or classify text into categories by analyzing embedding patterns. Documents with similar embeddings likely belong to the same topic or category.

**Duplicate Detection**: Identify duplicate or near-duplicate content by comparing embedding similarity. This works even when text is paraphrased or reworded.

**Anomaly Detection**: Detect unusual or outlier content by identifying embeddings that are far from typical patterns in your dataset.

## How to Use Embeddings

### Basic Request

To generate embeddings, send a POST request to `/embeddings` with your text input and chosen model:

TypeScript SDKPythonTypeScript (fetch)Shell

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | const response = await openRouter.embeddings.generate({ |
| 8 | model: '{{MODEL}}', |
| 9 | input: 'The quick brown fox jumps over the lazy dog', |
| 10 | }); |
| 11 |  |
| 12 | console.log(response.data[0].embedding); |
```

### Batch Processing

You can generate embeddings for multiple texts in a single request by passing an array of strings:

TypeScript SDKPythonTypeScript (fetch)Shell

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | const response = await openRouter.embeddings.generate({ |
| 8 | model: '{{MODEL}}', |
| 9 | input: [ |
| 10 | 'Machine learning is a subset of artificial intelligence', |
| 11 | 'Deep learning uses neural networks with multiple layers', |
| 12 | 'Natural language processing enables computers to understand text' |
| 13 | ], |
| 14 | }); |
| 15 |  |
| 16 | // Process each embedding |
| 17 | response.data.forEach((item, index) => { |
| 18 | console.log(`Embedding ${index}: ${item.embedding.length} dimensions`); |
| 19 | }); |
```

## API Reference

For detailed information about request parameters, response format, and all available options, see the [Embeddings API Reference](/docs/api-reference/embeddings/create-embeddings).

## Available Models

OpenRouter provides access to various embedding models from different providers. You can view all available embedding models at:

<https://openrouter.ai/models?fmt=cards&output_modalities=embeddings>

To list all available embedding models programmatically:

TypeScript SDKPythonTypeScript (fetch)Shell

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | const models = await openRouter.embeddings.listModels(); |
| 8 | console.log(models.data); |
```

## Practical Example: Semantic Search

Here’s a complete example of building a semantic search system using embeddings:

TypeScript SDKPython

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '{{API_KEY_REF}}', |
| 5 | }); |
| 6 |  |
| 7 | // Sample documents |
| 8 | const documents = [ |
| 9 | "The cat sat on the mat", |
| 10 | "Dogs are loyal companions", |
| 11 | "Python is a programming language", |
| 12 | "Machine learning models require training data", |
| 13 | "The weather is sunny today" |
| 14 | ]; |
| 15 |  |
| 16 | // Function to calculate cosine similarity |
| 17 | function cosineSimilarity(a: number[], b: number[]): number { |
| 18 | const dotProduct = a.reduce((sum, val, i) => sum + val * b[i], 0); |
| 19 | const magnitudeA = Math.sqrt(a.reduce((sum, val) => sum + val * val, 0)); |
| 20 | const magnitudeB = Math.sqrt(b.reduce((sum, val) => sum + val * val, 0)); |
| 21 | return dotProduct / (magnitudeA * magnitudeB); |
| 22 | } |
| 23 |  |
| 24 | async function semanticSearch(query: string, documents: string[]) { |
| 25 | // Generate embeddings for all documents and the query |
| 26 | const response = await openRouter.embeddings.generate({ |
| 27 | model: '{{MODEL}}', |
| 28 | input: [query, ...documents], |
| 29 | }); |
| 30 |  |
| 31 | const queryEmbedding = response.data[0].embedding; |
| 32 | const docEmbeddings = response.data.slice(1); |
| 33 |  |
| 34 | // Calculate similarity scores |
| 35 | const results = documents.map((doc, i) => ({ |
| 36 | document: doc, |
| 37 | similarity: cosineSimilarity( |
| 38 | queryEmbedding as number[], |
| 39 | docEmbeddings[i].embedding as number[] |
| 40 | ), |
| 41 | })); |
| 42 |  |
| 43 | // Sort by similarity (highest first) |
| 44 | results.sort((a, b) => b.similarity - a.similarity); |
| 45 |  |
| 46 | return results; |
| 47 | } |
| 48 |  |
| 49 | // Search for documents related to pets |
| 50 | const results = await semanticSearch("pets and animals", documents); |
| 51 | console.log("Search results:"); |
| 52 | results.forEach((result, i) => { |
| 53 | console.log(`${i + 1}. ${result.document} (similarity: ${result.similarity.toFixed(4)})`); |
| 54 | }); |
```

Expected output:

```
|  |
| --- |
| Search results: |
| 1. Dogs are loyal companions (similarity: 0.8234) |
| 2. The cat sat on the mat (similarity: 0.7891) |
| 3. The weather is sunny today (similarity: 0.3456) |
| 4. Machine learning models require training data (similarity: 0.2987) |
| 5. Python is a programming language (similarity: 0.2654) |
```

## Best Practices

**Choose the Right Model**: Different embedding models have different strengths. Smaller models (like qwen/qwen3-embedding-0.6b or openai/text-embedding-3-small) are faster and cheaper, while larger models (like openai/text-embedding-3-large) provide better quality. Test multiple models to find the best fit for your use case.

**Batch Your Requests**: When processing multiple texts, send them in a single request rather than making individual API calls. This reduces latency and costs.

**Cache Embeddings**: Embeddings for the same text are deterministic (they don’t change). Store embeddings in a database or vector store to avoid regenerating them repeatedly.

**Normalize for Comparison**: When comparing embeddings, use cosine similarity rather than Euclidean distance. Cosine similarity is scale-invariant and works better for high-dimensional vectors.

**Consider Context Length**: Each model has a maximum input length (context window). Longer texts may need to be chunked or truncated. Check the model’s specifications before processing long documents.

**Use Appropriate Chunking**: For long documents, split them into meaningful chunks (paragraphs, sections) rather than arbitrary character limits. This preserves semantic coherence.

## Provider Routing

You can control which providers serve your embedding requests using the `provider` parameter. This is useful for:

* Ensuring data privacy with specific providers
* Optimizing for cost or latency
* Using provider-specific features

Example with provider preferences:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "model": "openai/text-embedding-3-small", |
| 3 | "input": "Your text here", |
| 4 | "provider": { |
| 5 | "order": ["openai", "azure"], |
| 6 | "allow_fallbacks": true, |
| 7 | "data_collection": "deny" |
| 8 | } |
| 9 | } |
```

For more information, see [Provider Routing](/docs/features/provider-routing).

## Error Handling

Common errors you may encounter:

**400 Bad Request**: Invalid input format or missing required parameters. Check that your `input` and `model` parameters are correctly formatted.

**401 Unauthorized**: Invalid or missing API key. Verify your API key is correct and included in the Authorization header.

**402 Payment Required**: Insufficient credits. Add credits to your OpenRouter account.

**404 Not Found**: The specified model doesn’t exist or isn’t available for embeddings. Check the model name and verify it’s an embedding model.

**429 Too Many Requests**: Rate limit exceeded. Implement exponential backoff and retry logic.

**529 Provider Overloaded**: The provider is temporarily overloaded. Enable `allow_fallbacks: true` to automatically use backup providers.

## Limitations

* **No Streaming**: Unlike chat completions, embeddings are returned as complete responses. Streaming is not supported.
* **Token Limits**: Each model has a maximum input length. Texts exceeding this limit will be truncated or rejected.
* **Deterministic Output**: Embeddings for the same input text will always be identical (no temperature or randomness).
* **Language Support**: Some models are optimized for specific languages. Check model documentation for language capabilities.

## Related Resources

* [Models Page](https://openrouter.ai/models?fmt=cards&output_modalities=embeddings) - Browse all available embedding models
* [Provider Routing](/docs/features/provider-routing) - Control which providers serve your requests
* [Authentication](/docs/api/authentication) - Learn about API key authentication
* [Errors](/docs/api/errors) - Detailed error codes and handling

Was this page helpful?

YesNo

[Previous](/docs/api-reference/streaming)[#### Limits

Rate Limits

Next](/docs/api-reference/limits)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
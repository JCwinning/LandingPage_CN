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

* [Getting Started with Organizations](#getting-started-with-organizations)
* [Creating an Organization](#creating-an-organization)
* [Switching Between Personal and Organization Accounts](#switching-between-personal-and-organization-accounts)
* [Credit Management](#credit-management)
* [Shared Credit Pool](#shared-credit-pool)
* [Admin-Only Credit Management](#admin-only-credit-management)
* [Transferring Credits from Personal to Organization](#transferring-credits-from-personal-to-organization)
* [API Key Management](#api-key-management)
* [Member Permissions](#member-permissions)
* [Administrator Permissions](#administrator-permissions)
* [Activity and Usage Tracking](#activity-and-usage-tracking)
* [Organization-Wide Activity Feed](#organization-wide-activity-feed)
* [Usage Analytics](#usage-analytics)
* [Administrative Controls](#administrative-controls)
* [Admin-Only Settings](#admin-only-settings)
* [Member Role Management](#member-role-management)
* [Use Cases and Benefits](#use-cases-and-benefits)
* [For Development Teams](#for-development-teams)
* [For Companies](#for-companies)
* [For Research Organizations](#for-research-organizations)
* [Frequently Asked Questions](#frequently-asked-questions)
* [Getting Help](#getting-help)

[Use Cases](/docs/use-cases/byok)

# Organization Management

Copy page

Manage teams and shared resources with OpenRouter organizations

OpenRouter organizations enable teams and companies to collaborate effectively by sharing credits, managing API keys centrally, and tracking usage across all team members. Organizations are ideal for companies that want to pool resources, manage inference costs centrally, and maintain oversight of AI usage across their team.

## Getting Started with Organizations

### Creating an Organization

To create an organization:

1. Navigate to [Settings > Preferences](https://openrouter.ai/settings/preferences)
2. In the Organization section, click **Create Organization**
3. Follow the setup process to configure your organization details
4. Invite team members to join your organization

#####

You must have a verified email address to create an organization.

### Switching Between Personal and Organization Accounts

Once you’re part of an organization, you can easily switch between your personal account and organization context:

* Use the **organization switcher** at the top of the web application
* When in organization mode, all actions (API usage, credit purchases, key management) are performed on behalf of the organization
* When in personal mode, you’re working with your individual account resources

## Credit Management

### Shared Credit Pool

Organizations maintain a shared credit pool that offers several advantages:

* **Centralized Billing**: All credits purchased in the organization account can be used by any organization member
* **Simplified Accounting**: Track all AI inference costs in one place
* **Budget Control**: Administrators can manage spending and monitor usage across the entire team

### Admin-Only Credit Management

Only organization administrators can:

* Purchase credits for the organization
* View detailed billing information
* Manage payment methods and invoicing settings

#####

Regular organization members cannot purchase credits or access billing information. Contact your organization administrator for credit-related requests.

### Transferring Credits from Personal to Organization

If you need to transfer credits from your personal account to your organization account:

1. Email [[email protected]](/cdn-cgi/l/email-protection#6b181e1b1b04191f2b041b0e0519041e1f0e19450a02) with your request
2. Include your organization details and the amount you wish to transfer
3. Our support team will process the transfer manually

#####

Credit transfers from personal to organization accounts require manual processing by our support team and cannot be done automatically through the interface.

## API Key Management

Organizations provide flexible API key management with role-based permissions:

### Member Permissions

* **Create API Keys**: All organization members can create API keys
* **View Own Keys**: Members can only view and manage API keys they created
* **Use Organization Keys**: Keys created by any organization member can be used by all members
* **Shared Usage**: API usage from any organization key is billed to the organization’s credit pool

### Administrator Permissions

* **View All Keys**: Administrators can view all API keys created within the organization
* **Manage All Keys**: Full access to edit, disable, or delete any organization API key
* **Monitor Usage**: Access to detailed usage analytics for all organization keys

#####

When creating API keys within an organization, consider using descriptive names that indicate the key’s purpose or the team member responsible for it.

## Activity and Usage Tracking

### Organization-Wide Activity Feed

When viewing your activity feed while in organization context, you’ll see:

* **All Member Activity**: Usage data from all organization members appears in the activity feed
* **Metadata Only**: Activity shows model usage, costs, and request metadata
* **Key Filtering**: Activity can be filtered by a specific API key to view usage for that key only

#####

**Known Limitation**: The activity feed currently shows all organization member activity when in organization context, not just your individual activity. Usage metadata (model used, cost, timing) is visible to all organization members.

### Usage Analytics

Organizations benefit from comprehensive usage analytics:

* Track spending across all team members
* Monitor model usage patterns
* Identify cost optimization opportunities
* Generate reports for budget planning

## Administrative Controls

### Admin-Only Settings

Organization administrators have exclusive access to:

* **Provider Settings**: Configure preferred model providers and routing preferences
* **Privacy Settings**: Manage data retention and privacy policies for the organization
* **Member Management**: Add, remove, and manage member roles
* **Billing Configuration**: Set up invoicing, payment methods, and billing contacts

### Member Role Management

Organizations support role-based access control:

* **Admin**: Full access to all organization features and settings
* **Member**: Access to create keys, use organization resources, and view own activity

## Use Cases and Benefits

### For Development Teams

* **Shared Resources**: Pool credits across multiple developers and projects
* **Centralized Management**: Manage all API keys and usage from a single dashboard
* **Cost Tracking**: Monitor spending per project or team member
* **Simplified Onboarding**: New team members can immediately access organization resources

### For Companies

* **Budget Control**: Administrators control spending and resource allocation
* **Compliance**: Centralized logging and usage tracking for audit purposes
* **Scalability**: Easy to add new team members and projects
* **Cost Optimization**: Identify usage patterns and optimize model selection

### For Research Organizations

* **Resource Sharing**: Share expensive model access across research teams
* **Usage Monitoring**: Track research spending and resource utilization
* **Collaboration**: Enable seamless collaboration on AI projects
* **Reporting**: Generate usage reports for grant applications and budget planning

## Frequently Asked Questions

###### Can I convert my personal account to an organization?

No, organizations are separate entities. You’ll need to create a new organization and transfer resources as needed. Contact [[email protected]](/cdn-cgi/l/email-protection#c5b6b0b5b5aab7b185aab5a0abb7aab0b1a0b7eba4ac) for assistance with credit transfers.

###### How many members can an organization have?

An organization can only have 10 members. Contact support if you need more.

###### Can organization members see each other's usage data?

Organization members can see usage metadata (model used, cost, timing) for all organization activity in the activity feed. OpenRouter does not store prompts or responses.

###### What happens if I leave an organization?

When you leave an organization, you lose access to organization resources, credits, and API keys. Your personal account remains unaffected.

###### Can I be a member of multiple organizations?

Yes, you can be a member of multiple organizations and switch between them using the organization switcher.

## Getting Help

If you need assistance with organization management:

* **General Questions**: Check our [FAQ](/docs/faq) for common questions
* **Technical Support**: Email [[email protected]](/cdn-cgi/l/email-protection#8dfef8fdfde2fff9cde2fde8e3ffe2f8f9e8ffa3ece4)
* **Credit Transfers**: Email [[email protected]](/cdn-cgi/l/email-protection#a8dbddd8d8c7dadce8c7d8cdc6dac7dddccdda86c9c1) with transfer requests
* **Enterprise Sales**: Contact our sales team for large organization needs

Organizations make it easy to collaborate on AI projects while maintaining control over costs and resources. Get started by creating your organization today!

Was this page helpful?

YesNo

[Previous](/docs/use-cases/mcp-servers)[#### Provider Integration

Next](/docs/use-cases/for-providers)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
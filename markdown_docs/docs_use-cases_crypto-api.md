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

* [Getting Credit Purchase Calldata](#getting-credit-purchase-calldata)
* [Sending the Transaction](#sending-the-transaction)
* [Detecting Low Balance](#detecting-low-balance)

[Use Cases](/docs/use-cases/byok)

# Crypto API

Copy page

Purchase credits with crypto

You can purchase credits using cryptocurrency through our Coinbase integration. This can either happen through the UI, on your [credits page](https://openrouter.ai/settings/credits), or through our API as described below. While other forms of payment are possible, this guide specifically shows how to pay with the chain’s native token.

Headless credit purchases involve three steps:

1. Getting the calldata for a new credit purchase
2. Sending a transaction on-chain using that data
3. Detecting low account balance, and purchasing more

## Getting Credit Purchase Calldata

Make a POST request to `/api/v1/credits/coinbase` to create a new charge. You’ll include the amount of credits you want to purchase (in USD, up to $100000), the address you’ll be sending the transaction from, and the EVM chain ID of the network you’ll be sending on.

Currently, we only support the following chains (mainnet only):

* Ethereum (1)
* Polygon (137)
* Base (8453) ***recommended***

```
|  |  |
| --- | --- |
| 1 | const response = await fetch('https://openrouter.ai/api/v1/credits/coinbase', { |
| 2 | method: 'POST', |
| 3 | headers: { |
| 4 | Authorization: 'Bearer <OPENROUTER_API_KEY>', |
| 5 | 'Content-Type': 'application/json', |
| 6 | }, |
| 7 | body: JSON.stringify({ |
| 8 | amount: 10, // Target credit amount in USD |
| 9 | sender: '0x9a85CB3bfd494Ea3a8C9E50aA6a3c1a7E8BACE11', |
| 10 | chain_id: 8453, |
| 11 | }), |
| 12 | }); |
| 13 | const responseJSON = await response.json(); |
```

The response includes the charge details and transaction data needed to execute the on-chain payment:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "data": { |
| 3 | "id": "...", |
| 4 | "created_at": "2024-01-01T00:00:00Z", |
| 5 | "expires_at": "2024-01-01T01:00:00Z", |
| 6 | "web3_data": { |
| 7 | "transfer_intent": { |
| 8 | "metadata": { |
| 9 | "chain_id": 8453, |
| 10 | "contract_address": "0x03059433bcdb6144624cc2443159d9445c32b7a8", |
| 11 | "sender": "0x9a85CB3bfd494Ea3a8C9E50aA6a3c1a7E8BACE11" |
| 12 | }, |
| 13 | "call_data": { |
| 14 | "recipient_amount": "...", |
| 15 | "deadline": "...", |
| 16 | "recipient": "...", |
| 17 | "recipient_currency": "...", |
| 18 | "refund_destination": "...", |
| 19 | "fee_amount": "...", |
| 20 | "id": "...", |
| 21 | "operator": "...", |
| 22 | "signature": "...", |
| 23 | "prefix": "..." |
| 24 | } |
| 25 | } |
| 26 | } |
| 27 | } |
| 28 | } |
```

## Sending the Transaction

You can use [viem](https://viem.sh/) (or another similar evm client) to execute the transaction on-chain.

In this example, we’ll be fulfilling the charge using the [swapAndTransferUniswapV3Native()](https://github.com/coinbase/commerce-onchain-payment-protocol/blob/d891289bd1f41bb95f749af537f2b6a36b17f889/contracts/interfaces/ITransfers.sol#L168-L171) function. Other methods of swapping are also available, and you can learn more by checking out Coinbase’s [onchain payment protocol here](https://github.com/coinbase/commerce-onchain-payment-protocol/tree/master). Note, if you are trying to pay in a less common ERC-20, there is added complexity in needing to make sure that there is sufficient liquidity in the pool to swap the tokens.

```
|  |  |
| --- | --- |
| 1 | import { createPublicClient, createWalletClient, http, parseEther } from 'viem'; |
| 2 | import { privateKeyToAccount } from 'viem/accounts'; |
| 3 | import { base } from 'viem/chains'; |
| 4 |  |
| 5 | // The ABI for Coinbase's onchain payment protocol |
| 6 | const abi = [ |
| 7 | { |
| 8 | inputs: [ |
| 9 | { |
| 10 | internalType: 'contract IUniversalRouter', |
| 11 | name: '_uniswap', |
| 12 | type: 'address', |
| 13 | }, |
| 14 | { internalType: 'contract Permit2', name: '_permit2', type: 'address' }, |
| 15 | { internalType: 'address', name: '_initialOperator', type: 'address' }, |
| 16 | { |
| 17 | internalType: 'address', |
| 18 | name: '_initialFeeDestination', |
| 19 | type: 'address', |
| 20 | }, |
| 21 | { |
| 22 | internalType: 'contract IWrappedNativeCurrency', |
| 23 | name: '_wrappedNativeCurrency', |
| 24 | type: 'address', |
| 25 | }, |
| 26 | ], |
| 27 | stateMutability: 'nonpayable', |
| 28 | type: 'constructor', |
| 29 | }, |
| 30 | { inputs: [], name: 'AlreadyProcessed', type: 'error' }, |
| 31 | { inputs: [], name: 'ExpiredIntent', type: 'error' }, |
| 32 | { |
| 33 | inputs: [ |
| 34 | { internalType: 'address', name: 'attemptedCurrency', type: 'address' }, |
| 35 | ], |
| 36 | name: 'IncorrectCurrency', |
| 37 | type: 'error', |
| 38 | }, |
| 39 | { inputs: [], name: 'InexactTransfer', type: 'error' }, |
| 40 | { |
| 41 | inputs: [{ internalType: 'uint256', name: 'difference', type: 'uint256' }], |
| 42 | name: 'InsufficientAllowance', |
| 43 | type: 'error', |
| 44 | }, |
| 45 | { |
| 46 | inputs: [{ internalType: 'uint256', name: 'difference', type: 'uint256' }], |
| 47 | name: 'InsufficientBalance', |
| 48 | type: 'error', |
| 49 | }, |
| 50 | { |
| 51 | inputs: [{ internalType: 'int256', name: 'difference', type: 'int256' }], |
| 52 | name: 'InvalidNativeAmount', |
| 53 | type: 'error', |
| 54 | }, |
| 55 | { inputs: [], name: 'InvalidSignature', type: 'error' }, |
| 56 | { inputs: [], name: 'InvalidTransferDetails', type: 'error' }, |
| 57 | { |
| 58 | inputs: [ |
| 59 | { internalType: 'address', name: 'recipient', type: 'address' }, |
| 60 | { internalType: 'uint256', name: 'amount', type: 'uint256' }, |
| 61 | { internalType: 'bool', name: 'isRefund', type: 'bool' }, |
| 62 | { internalType: 'bytes', name: 'data', type: 'bytes' }, |
| 63 | ], |
| 64 | name: 'NativeTransferFailed', |
| 65 | type: 'error', |
| 66 | }, |
| 67 | { inputs: [], name: 'NullRecipient', type: 'error' }, |
| 68 | { inputs: [], name: 'OperatorNotRegistered', type: 'error' }, |
| 69 | { inputs: [], name: 'PermitCallFailed', type: 'error' }, |
| 70 | { |
| 71 | inputs: [{ internalType: 'bytes', name: 'reason', type: 'bytes' }], |
| 72 | name: 'SwapFailedBytes', |
| 73 | type: 'error', |
| 74 | }, |
| 75 | { |
| 76 | inputs: [{ internalType: 'string', name: 'reason', type: 'string' }], |
| 77 | name: 'SwapFailedString', |
| 78 | type: 'error', |
| 79 | }, |
| 80 | { |
| 81 | anonymous: false, |
| 82 | inputs: [ |
| 83 | { |
| 84 | indexed: false, |
| 85 | internalType: 'address', |
| 86 | name: 'operator', |
| 87 | type: 'address', |
| 88 | }, |
| 89 | { |
| 90 | indexed: false, |
| 91 | internalType: 'address', |
| 92 | name: 'feeDestination', |
| 93 | type: 'address', |
| 94 | }, |
| 95 | ], |
| 96 | name: 'OperatorRegistered', |
| 97 | type: 'event', |
| 98 | }, |
| 99 | { |
| 100 | anonymous: false, |
| 101 | inputs: [ |
| 102 | { |
| 103 | indexed: false, |
| 104 | internalType: 'address', |
| 105 | name: 'operator', |
| 106 | type: 'address', |
| 107 | }, |
| 108 | ], |
| 109 | name: 'OperatorUnregistered', |
| 110 | type: 'event', |
| 111 | }, |
| 112 | { |
| 113 | anonymous: false, |
| 114 | inputs: [ |
| 115 | { |
| 116 | indexed: true, |
| 117 | internalType: 'address', |
| 118 | name: 'previousOwner', |
| 119 | type: 'address', |
| 120 | }, |
| 121 | { |
| 122 | indexed: true, |
| 123 | internalType: 'address', |
| 124 | name: 'newOwner', |
| 125 | type: 'address', |
| 126 | }, |
| 127 | ], |
| 128 | name: 'OwnershipTransferred', |
| 129 | type: 'event', |
| 130 | }, |
| 131 | { |
| 132 | anonymous: false, |
| 133 | inputs: [ |
| 134 | { |
| 135 | indexed: false, |
| 136 | internalType: 'address', |
| 137 | name: 'account', |
| 138 | type: 'address', |
| 139 | }, |
| 140 | ], |
| 141 | name: 'Paused', |
| 142 | type: 'event', |
| 143 | }, |
| 144 | { |
| 145 | anonymous: false, |
| 146 | inputs: [ |
| 147 | { |
| 148 | indexed: true, |
| 149 | internalType: 'address', |
| 150 | name: 'operator', |
| 151 | type: 'address', |
| 152 | }, |
| 153 | { indexed: false, internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 154 | { |
| 155 | indexed: false, |
| 156 | internalType: 'address', |
| 157 | name: 'recipient', |
| 158 | type: 'address', |
| 159 | }, |
| 160 | { |
| 161 | indexed: false, |
| 162 | internalType: 'address', |
| 163 | name: 'sender', |
| 164 | type: 'address', |
| 165 | }, |
| 166 | { |
| 167 | indexed: false, |
| 168 | internalType: 'uint256', |
| 169 | name: 'spentAmount', |
| 170 | type: 'uint256', |
| 171 | }, |
| 172 | { |
| 173 | indexed: false, |
| 174 | internalType: 'address', |
| 175 | name: 'spentCurrency', |
| 176 | type: 'address', |
| 177 | }, |
| 178 | ], |
| 179 | name: 'Transferred', |
| 180 | type: 'event', |
| 181 | }, |
| 182 | { |
| 183 | anonymous: false, |
| 184 | inputs: [ |
| 185 | { |
| 186 | indexed: false, |
| 187 | internalType: 'address', |
| 188 | name: 'account', |
| 189 | type: 'address', |
| 190 | }, |
| 191 | ], |
| 192 | name: 'Unpaused', |
| 193 | type: 'event', |
| 194 | }, |
| 195 | { |
| 196 | inputs: [], |
| 197 | name: 'owner', |
| 198 | outputs: [{ internalType: 'address', name: '', type: 'address' }], |
| 199 | stateMutability: 'view', |
| 200 | type: 'function', |
| 201 | }, |
| 202 | { |
| 203 | inputs: [], |
| 204 | name: 'pause', |
| 205 | outputs: [], |
| 206 | stateMutability: 'nonpayable', |
| 207 | type: 'function', |
| 208 | }, |
| 209 | { |
| 210 | inputs: [], |
| 211 | name: 'paused', |
| 212 | outputs: [{ internalType: 'bool', name: '', type: 'bool' }], |
| 213 | stateMutability: 'view', |
| 214 | type: 'function', |
| 215 | }, |
| 216 | { |
| 217 | inputs: [], |
| 218 | name: 'permit2', |
| 219 | outputs: [{ internalType: 'contract Permit2', name: '', type: 'address' }], |
| 220 | stateMutability: 'view', |
| 221 | type: 'function', |
| 222 | }, |
| 223 | { |
| 224 | inputs: [], |
| 225 | name: 'registerOperator', |
| 226 | outputs: [], |
| 227 | stateMutability: 'nonpayable', |
| 228 | type: 'function', |
| 229 | }, |
| 230 | { |
| 231 | inputs: [ |
| 232 | { internalType: 'address', name: '_feeDestination', type: 'address' }, |
| 233 | ], |
| 234 | name: 'registerOperatorWithFeeDestination', |
| 235 | outputs: [], |
| 236 | stateMutability: 'nonpayable', |
| 237 | type: 'function', |
| 238 | }, |
| 239 | { |
| 240 | inputs: [], |
| 241 | name: 'renounceOwnership', |
| 242 | outputs: [], |
| 243 | stateMutability: 'nonpayable', |
| 244 | type: 'function', |
| 245 | }, |
| 246 | { |
| 247 | inputs: [{ internalType: 'address', name: 'newSweeper', type: 'address' }], |
| 248 | name: 'setSweeper', |
| 249 | outputs: [], |
| 250 | stateMutability: 'nonpayable', |
| 251 | type: 'function', |
| 252 | }, |
| 253 | { |
| 254 | inputs: [ |
| 255 | { |
| 256 | components: [ |
| 257 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 258 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 259 | { |
| 260 | internalType: 'address payable', |
| 261 | name: 'recipient', |
| 262 | type: 'address', |
| 263 | }, |
| 264 | { |
| 265 | internalType: 'address', |
| 266 | name: 'recipientCurrency', |
| 267 | type: 'address', |
| 268 | }, |
| 269 | { |
| 270 | internalType: 'address', |
| 271 | name: 'refundDestination', |
| 272 | type: 'address', |
| 273 | }, |
| 274 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 275 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 276 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 277 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 278 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 279 | ], |
| 280 | internalType: 'struct TransferIntent', |
| 281 | name: '_intent', |
| 282 | type: 'tuple', |
| 283 | }, |
| 284 | { |
| 285 | components: [ |
| 286 | { internalType: 'address', name: 'owner', type: 'address' }, |
| 287 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 288 | ], |
| 289 | internalType: 'struct EIP2612SignatureTransferData', |
| 290 | name: '_signatureTransferData', |
| 291 | type: 'tuple', |
| 292 | }, |
| 293 | ], |
| 294 | name: 'subsidizedTransferToken', |
| 295 | outputs: [], |
| 296 | stateMutability: 'nonpayable', |
| 297 | type: 'function', |
| 298 | }, |
| 299 | { |
| 300 | inputs: [ |
| 301 | { |
| 302 | components: [ |
| 303 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 304 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 305 | { |
| 306 | internalType: 'address payable', |
| 307 | name: 'recipient', |
| 308 | type: 'address', |
| 309 | }, |
| 310 | { |
| 311 | internalType: 'address', |
| 312 | name: 'recipientCurrency', |
| 313 | type: 'address', |
| 314 | }, |
| 315 | { |
| 316 | internalType: 'address', |
| 317 | name: 'refundDestination', |
| 318 | type: 'address', |
| 319 | }, |
| 320 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 321 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 322 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 323 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 324 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 325 | ], |
| 326 | internalType: 'struct TransferIntent', |
| 327 | name: '_intent', |
| 328 | type: 'tuple', |
| 329 | }, |
| 330 | { internalType: 'uint24', name: 'poolFeesTier', type: 'uint24' }, |
| 331 | ], |
| 332 | name: 'swapAndTransferUniswapV3Native', |
| 333 | outputs: [], |
| 334 | stateMutability: 'payable', |
| 335 | type: 'function', |
| 336 | }, |
| 337 | { |
| 338 | inputs: [ |
| 339 | { |
| 340 | components: [ |
| 341 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 342 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 343 | { |
| 344 | internalType: 'address payable', |
| 345 | name: 'recipient', |
| 346 | type: 'address', |
| 347 | }, |
| 348 | { |
| 349 | internalType: 'address', |
| 350 | name: 'recipientCurrency', |
| 351 | type: 'address', |
| 352 | }, |
| 353 | { |
| 354 | internalType: 'address', |
| 355 | name: 'refundDestination', |
| 356 | type: 'address', |
| 357 | }, |
| 358 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 359 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 360 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 361 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 362 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 363 | ], |
| 364 | internalType: 'struct TransferIntent', |
| 365 | name: '_intent', |
| 366 | type: 'tuple', |
| 367 | }, |
| 368 | { |
| 369 | components: [ |
| 370 | { |
| 371 | components: [ |
| 372 | { |
| 373 | components: [ |
| 374 | { internalType: 'address', name: 'token', type: 'address' }, |
| 375 | { internalType: 'uint256', name: 'amount', type: 'uint256' }, |
| 376 | ], |
| 377 | internalType: 'struct ISignatureTransfer.TokenPermissions', |
| 378 | name: 'permitted', |
| 379 | type: 'tuple', |
| 380 | }, |
| 381 | { internalType: 'uint256', name: 'nonce', type: 'uint256' }, |
| 382 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 383 | ], |
| 384 | internalType: 'struct ISignatureTransfer.PermitTransferFrom', |
| 385 | name: 'permit', |
| 386 | type: 'tuple', |
| 387 | }, |
| 388 | { |
| 389 | components: [ |
| 390 | { internalType: 'address', name: 'to', type: 'address' }, |
| 391 | { |
| 392 | internalType: 'uint256', |
| 393 | name: 'requestedAmount', |
| 394 | type: 'uint256', |
| 395 | }, |
| 396 | ], |
| 397 | internalType: 'struct ISignatureTransfer.SignatureTransferDetails', |
| 398 | name: 'transferDetails', |
| 399 | type: 'tuple', |
| 400 | }, |
| 401 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 402 | ], |
| 403 | internalType: 'struct Permit2SignatureTransferData', |
| 404 | name: '_signatureTransferData', |
| 405 | type: 'tuple', |
| 406 | }, |
| 407 | { internalType: 'uint24', name: 'poolFeesTier', type: 'uint24' }, |
| 408 | ], |
| 409 | name: 'swapAndTransferUniswapV3Token', |
| 410 | outputs: [], |
| 411 | stateMutability: 'nonpayable', |
| 412 | type: 'function', |
| 413 | }, |
| 414 | { |
| 415 | inputs: [ |
| 416 | { |
| 417 | components: [ |
| 418 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 419 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 420 | { |
| 421 | internalType: 'address payable', |
| 422 | name: 'recipient', |
| 423 | type: 'address', |
| 424 | }, |
| 425 | { |
| 426 | internalType: 'address', |
| 427 | name: 'recipientCurrency', |
| 428 | type: 'address', |
| 429 | }, |
| 430 | { |
| 431 | internalType: 'address', |
| 432 | name: 'refundDestination', |
| 433 | type: 'address', |
| 434 | }, |
| 435 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 436 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 437 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 438 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 439 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 440 | ], |
| 441 | internalType: 'struct TransferIntent', |
| 442 | name: '_intent', |
| 443 | type: 'tuple', |
| 444 | }, |
| 445 | { internalType: 'address', name: '_tokenIn', type: 'address' }, |
| 446 | { internalType: 'uint256', name: 'maxWillingToPay', type: 'uint256' }, |
| 447 | { internalType: 'uint24', name: 'poolFeesTier', type: 'uint24' }, |
| 448 | ], |
| 449 | name: 'swapAndTransferUniswapV3TokenPreApproved', |
| 450 | outputs: [], |
| 451 | stateMutability: 'nonpayable', |
| 452 | type: 'function', |
| 453 | }, |
| 454 | { |
| 455 | inputs: [ |
| 456 | { internalType: 'address payable', name: 'destination', type: 'address' }, |
| 457 | ], |
| 458 | name: 'sweepETH', |
| 459 | outputs: [], |
| 460 | stateMutability: 'nonpayable', |
| 461 | type: 'function', |
| 462 | }, |
| 463 | { |
| 464 | inputs: [ |
| 465 | { internalType: 'address payable', name: 'destination', type: 'address' }, |
| 466 | { internalType: 'uint256', name: 'amount', type: 'uint256' }, |
| 467 | ], |
| 468 | name: 'sweepETHAmount', |
| 469 | outputs: [], |
| 470 | stateMutability: 'nonpayable', |
| 471 | type: 'function', |
| 472 | }, |
| 473 | { |
| 474 | inputs: [ |
| 475 | { internalType: 'address', name: '_token', type: 'address' }, |
| 476 | { internalType: 'address', name: 'destination', type: 'address' }, |
| 477 | ], |
| 478 | name: 'sweepToken', |
| 479 | outputs: [], |
| 480 | stateMutability: 'nonpayable', |
| 481 | type: 'function', |
| 482 | }, |
| 483 | { |
| 484 | inputs: [ |
| 485 | { internalType: 'address', name: '_token', type: 'address' }, |
| 486 | { internalType: 'address', name: 'destination', type: 'address' }, |
| 487 | { internalType: 'uint256', name: 'amount', type: 'uint256' }, |
| 488 | ], |
| 489 | name: 'sweepTokenAmount', |
| 490 | outputs: [], |
| 491 | stateMutability: 'nonpayable', |
| 492 | type: 'function', |
| 493 | }, |
| 494 | { |
| 495 | inputs: [], |
| 496 | name: 'sweeper', |
| 497 | outputs: [{ internalType: 'address', name: '', type: 'address' }], |
| 498 | stateMutability: 'view', |
| 499 | type: 'function', |
| 500 | }, |
| 501 | { |
| 502 | inputs: [ |
| 503 | { |
| 504 | components: [ |
| 505 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 506 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 507 | { |
| 508 | internalType: 'address payable', |
| 509 | name: 'recipient', |
| 510 | type: 'address', |
| 511 | }, |
| 512 | { |
| 513 | internalType: 'address', |
| 514 | name: 'recipientCurrency', |
| 515 | type: 'address', |
| 516 | }, |
| 517 | { |
| 518 | internalType: 'address', |
| 519 | name: 'refundDestination', |
| 520 | type: 'address', |
| 521 | }, |
| 522 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 523 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 524 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 525 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 526 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 527 | ], |
| 528 | internalType: 'struct TransferIntent', |
| 529 | name: '_intent', |
| 530 | type: 'tuple', |
| 531 | }, |
| 532 | ], |
| 533 | name: 'transferNative', |
| 534 | outputs: [], |
| 535 | stateMutability: 'payable', |
| 536 | type: 'function', |
| 537 | }, |
| 538 | { |
| 539 | inputs: [{ internalType: 'address', name: 'newOwner', type: 'address' }], |
| 540 | name: 'transferOwnership', |
| 541 | outputs: [], |
| 542 | stateMutability: 'nonpayable', |
| 543 | type: 'function', |
| 544 | }, |
| 545 | { |
| 546 | inputs: [ |
| 547 | { |
| 548 | components: [ |
| 549 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 550 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 551 | { |
| 552 | internalType: 'address payable', |
| 553 | name: 'recipient', |
| 554 | type: 'address', |
| 555 | }, |
| 556 | { |
| 557 | internalType: 'address', |
| 558 | name: 'recipientCurrency', |
| 559 | type: 'address', |
| 560 | }, |
| 561 | { |
| 562 | internalType: 'address', |
| 563 | name: 'refundDestination', |
| 564 | type: 'address', |
| 565 | }, |
| 566 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 567 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 568 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 569 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 570 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 571 | ], |
| 572 | internalType: 'struct TransferIntent', |
| 573 | name: '_intent', |
| 574 | type: 'tuple', |
| 575 | }, |
| 576 | { |
| 577 | components: [ |
| 578 | { |
| 579 | components: [ |
| 580 | { |
| 581 | components: [ |
| 582 | { internalType: 'address', name: 'token', type: 'address' }, |
| 583 | { internalType: 'uint256', name: 'amount', type: 'uint256' }, |
| 584 | ], |
| 585 | internalType: 'struct ISignatureTransfer.TokenPermissions', |
| 586 | name: 'permitted', |
| 587 | type: 'tuple', |
| 588 | }, |
| 589 | { internalType: 'uint256', name: 'nonce', type: 'uint256' }, |
| 590 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 591 | ], |
| 592 | internalType: 'struct ISignatureTransfer.PermitTransferFrom', |
| 593 | name: 'permit', |
| 594 | type: 'tuple', |
| 595 | }, |
| 596 | { |
| 597 | components: [ |
| 598 | { internalType: 'address', name: 'to', type: 'address' }, |
| 599 | { |
| 600 | internalType: 'uint256', |
| 601 | name: 'requestedAmount', |
| 602 | type: 'uint256', |
| 603 | }, |
| 604 | ], |
| 605 | internalType: 'struct ISignatureTransfer.SignatureTransferDetails', |
| 606 | name: 'transferDetails', |
| 607 | type: 'tuple', |
| 608 | }, |
| 609 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 610 | ], |
| 611 | internalType: 'struct Permit2SignatureTransferData', |
| 612 | name: '_signatureTransferData', |
| 613 | type: 'tuple', |
| 614 | }, |
| 615 | ], |
| 616 | name: 'transferToken', |
| 617 | outputs: [], |
| 618 | stateMutability: 'nonpayable', |
| 619 | type: 'function', |
| 620 | }, |
| 621 | { |
| 622 | inputs: [ |
| 623 | { |
| 624 | components: [ |
| 625 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 626 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 627 | { |
| 628 | internalType: 'address payable', |
| 629 | name: 'recipient', |
| 630 | type: 'address', |
| 631 | }, |
| 632 | { |
| 633 | internalType: 'address', |
| 634 | name: 'recipientCurrency', |
| 635 | type: 'address', |
| 636 | }, |
| 637 | { |
| 638 | internalType: 'address', |
| 639 | name: 'refundDestination', |
| 640 | type: 'address', |
| 641 | }, |
| 642 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 643 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 644 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 645 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 646 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 647 | ], |
| 648 | internalType: 'struct TransferIntent', |
| 649 | name: '_intent', |
| 650 | type: 'tuple', |
| 651 | }, |
| 652 | ], |
| 653 | name: 'transferTokenPreApproved', |
| 654 | outputs: [], |
| 655 | stateMutability: 'nonpayable', |
| 656 | type: 'function', |
| 657 | }, |
| 658 | { |
| 659 | inputs: [], |
| 660 | name: 'unpause', |
| 661 | outputs: [], |
| 662 | stateMutability: 'nonpayable', |
| 663 | type: 'function', |
| 664 | }, |
| 665 | { |
| 666 | inputs: [], |
| 667 | name: 'unregisterOperator', |
| 668 | outputs: [], |
| 669 | stateMutability: 'nonpayable', |
| 670 | type: 'function', |
| 671 | }, |
| 672 | { |
| 673 | inputs: [ |
| 674 | { |
| 675 | components: [ |
| 676 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 677 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 678 | { |
| 679 | internalType: 'address payable', |
| 680 | name: 'recipient', |
| 681 | type: 'address', |
| 682 | }, |
| 683 | { |
| 684 | internalType: 'address', |
| 685 | name: 'recipientCurrency', |
| 686 | type: 'address', |
| 687 | }, |
| 688 | { |
| 689 | internalType: 'address', |
| 690 | name: 'refundDestination', |
| 691 | type: 'address', |
| 692 | }, |
| 693 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 694 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 695 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 696 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 697 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 698 | ], |
| 699 | internalType: 'struct TransferIntent', |
| 700 | name: '_intent', |
| 701 | type: 'tuple', |
| 702 | }, |
| 703 | { |
| 704 | components: [ |
| 705 | { |
| 706 | components: [ |
| 707 | { |
| 708 | components: [ |
| 709 | { internalType: 'address', name: 'token', type: 'address' }, |
| 710 | { internalType: 'uint256', name: 'amount', type: 'uint256' }, |
| 711 | ], |
| 712 | internalType: 'struct ISignatureTransfer.TokenPermissions', |
| 713 | name: 'permitted', |
| 714 | type: 'tuple', |
| 715 | }, |
| 716 | { internalType: 'uint256', name: 'nonce', type: 'uint256' }, |
| 717 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 718 | ], |
| 719 | internalType: 'struct ISignatureTransfer.PermitTransferFrom', |
| 720 | name: 'permit', |
| 721 | type: 'tuple', |
| 722 | }, |
| 723 | { |
| 724 | components: [ |
| 725 | { internalType: 'address', name: 'to', type: 'address' }, |
| 726 | { |
| 727 | internalType: 'uint256', |
| 728 | name: 'requestedAmount', |
| 729 | type: 'uint256', |
| 730 | }, |
| 731 | ], |
| 732 | internalType: 'struct ISignatureTransfer.SignatureTransferDetails', |
| 733 | name: 'transferDetails', |
| 734 | type: 'tuple', |
| 735 | }, |
| 736 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 737 | ], |
| 738 | internalType: 'struct Permit2SignatureTransferData', |
| 739 | name: '_signatureTransferData', |
| 740 | type: 'tuple', |
| 741 | }, |
| 742 | ], |
| 743 | name: 'unwrapAndTransfer', |
| 744 | outputs: [], |
| 745 | stateMutability: 'nonpayable', |
| 746 | type: 'function', |
| 747 | }, |
| 748 | { |
| 749 | inputs: [ |
| 750 | { |
| 751 | components: [ |
| 752 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 753 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 754 | { |
| 755 | internalType: 'address payable', |
| 756 | name: 'recipient', |
| 757 | type: 'address', |
| 758 | }, |
| 759 | { |
| 760 | internalType: 'address', |
| 761 | name: 'recipientCurrency', |
| 762 | type: 'address', |
| 763 | }, |
| 764 | { |
| 765 | internalType: 'address', |
| 766 | name: 'refundDestination', |
| 767 | type: 'address', |
| 768 | }, |
| 769 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 770 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 771 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 772 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 773 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 774 | ], |
| 775 | internalType: 'struct TransferIntent', |
| 776 | name: '_intent', |
| 777 | type: 'tuple', |
| 778 | }, |
| 779 | ], |
| 780 | name: 'unwrapAndTransferPreApproved', |
| 781 | outputs: [], |
| 782 | stateMutability: 'nonpayable', |
| 783 | type: 'function', |
| 784 | }, |
| 785 | { |
| 786 | inputs: [ |
| 787 | { |
| 788 | components: [ |
| 789 | { internalType: 'uint256', name: 'recipientAmount', type: 'uint256' }, |
| 790 | { internalType: 'uint256', name: 'deadline', type: 'uint256' }, |
| 791 | { |
| 792 | internalType: 'address payable', |
| 793 | name: 'recipient', |
| 794 | type: 'address', |
| 795 | }, |
| 796 | { |
| 797 | internalType: 'address', |
| 798 | name: 'recipientCurrency', |
| 799 | type: 'address', |
| 800 | }, |
| 801 | { |
| 802 | internalType: 'address', |
| 803 | name: 'refundDestination', |
| 804 | type: 'address', |
| 805 | }, |
| 806 | { internalType: 'uint256', name: 'feeAmount', type: 'uint256' }, |
| 807 | { internalType: 'bytes16', name: 'id', type: 'bytes16' }, |
| 808 | { internalType: 'address', name: 'operator', type: 'address' }, |
| 809 | { internalType: 'bytes', name: 'signature', type: 'bytes' }, |
| 810 | { internalType: 'bytes', name: 'prefix', type: 'bytes' }, |
| 811 | ], |
| 812 | internalType: 'struct TransferIntent', |
| 813 | name: '_intent', |
| 814 | type: 'tuple', |
| 815 | }, |
| 816 | ], |
| 817 | name: 'wrapAndTransfer', |
| 818 | outputs: [], |
| 819 | stateMutability: 'payable', |
| 820 | type: 'function', |
| 821 | }, |
| 822 | { stateMutability: 'payable', type: 'receive' }, |
| 823 | ]; |
| 824 |  |
| 825 | // Set up viem clients |
| 826 | const publicClient = createPublicClient({ |
| 827 | chain: base, |
| 828 | transport: http(), |
| 829 | }); |
| 830 | const account = privateKeyToAccount('0x...'); |
| 831 | const walletClient = createWalletClient({ |
| 832 | chain: base, |
| 833 | transport: http(), |
| 834 | account, |
| 835 | }); |
| 836 |  |
| 837 | // Use the calldata included in the charge response |
| 838 | const { contract_address } = |
| 839 | responseJSON.data.web3_data.transfer_intent.metadata; |
| 840 | const call_data = responseJSON.data.web3_data.transfer_intent.call_data; |
| 841 |  |
| 842 | // When transacting in ETH, a pool fees tier of 500 (the lowest) is very |
| 843 | // likely to be sufficient. However, if you plan to swap with a different |
| 844 | // contract method, using less-common ERC-20 tokens, it is recommended to |
| 845 | // call that chain's Uniswap QuoterV2 contract to check its liquidity. |
| 846 | // Depending on the results, choose the lowest fee tier which has enough |
| 847 | // liquidity in the pool. |
| 848 | const poolFeesTier = 500; |
| 849 |  |
| 850 | // Simulate the transaction first to prevent most common revert reasons |
| 851 | const { request } = await publicClient.simulateContract({ |
| 852 | abi, |
| 853 | account, |
| 854 | address: contract_address, |
| 855 | functionName: 'swapAndTransferUniswapV3Native', |
| 856 | args: [ |
| 857 | { |
| 858 | recipientAmount: BigInt(call_data.recipient_amount), |
| 859 | deadline: BigInt( |
| 860 | Math.floor(new Date(call_data.deadline).getTime() / 1000), |
| 861 | ), |
| 862 | recipient: call_data.recipient, |
| 863 | recipientCurrency: call_data.recipient_currency, |
| 864 | refundDestination: call_data.refund_destination, |
| 865 | feeAmount: BigInt(call_data.fee_amount), |
| 866 | id: call_data.id, |
| 867 | operator: call_data.operator, |
| 868 | signature: call_data.signature, |
| 869 | prefix: call_data.prefix, |
| 870 | }, |
| 871 | poolFeesTier, |
| 872 | ], |
| 873 | // Transaction value in ETH. You'll want to include a little extra to |
| 874 | // ensure the transaction & swap is successful. All excess funds return |
| 875 | // back to your sender address afterwards. |
| 876 | value: parseEther('0.004'), |
| 877 | }); |
| 878 |  |
| 879 | // Send the transaction on chain |
| 880 | const txHash = await walletClient.writeContract(request); |
| 881 | console.log('Transaction hash:', txHash); |
|  |
```

Once the transaction succeeds on chain, we’ll add credits to your account. You can track the transaction status using the returned transaction hash.

Credit purchases lower than $500 will be immediately credited once the transaction is on chain. Above $500, there is a ~15 minute confirmation delay, ensuring the chain does not re-org your purchase.

## Detecting Low Balance

While it is possible to simply run down the balance until your app starts receiving 402 error codes for insufficient credits, this gap in service while topping up might not be desirable.

To avoid this, you can periodically call the `GET /api/v1/credits` endpoint to check your available credits.

TypeScript SDKTypeScript (fetch)

```
|  |  |
| --- | --- |
| 1 | import { OpenRouter } from '@openrouter/sdk'; |
| 2 |  |
| 3 | const openRouter = new OpenRouter({ |
| 4 | apiKey: '<OPENROUTER_API_KEY>', |
| 5 | }); |
| 6 |  |
| 7 | const credits = await openRouter.credits.get(); |
| 8 | console.log('Available credits:', credits.totalCredits - credits.totalUsage); |
```

The response includes your total credits purchased and usage, where your current balance is the difference between the two:

```
|  |  |
| --- | --- |
| 1 | { |
| 2 | "data": { |
| 3 | "total_credits": 50.0, |
| 4 | "total_usage": 42.0 |
| 5 | } |
| 6 | } |
```

Note that these values are cached, and may be up to 60 seconds stale.

Was this page helpful?

YesNo

[Previous](/docs/use-cases/byok)[#### OAuth PKCE

Connect your users to OpenRouter

Next](/docs/use-cases/oauth-pkce)[Built with](https://buildwithfern.com/?utm_campaign=buildWith&utm_medium=docs&utm_source=openrouter.ai)

[![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo.svg)![Logo](https://files.buildwithfern.com/openrouter.docs.buildwithfern.com/docs/2025-11-21T16:36:36.134Z/content/assets/logo-white.svg)](https://openrouter.ai/)

[API](/docs/api-reference/overview)[Models](https://openrouter.ai/models)[Chat](https://openrouter.ai/chat)[Ranking](https://openrouter.ai/rankings)
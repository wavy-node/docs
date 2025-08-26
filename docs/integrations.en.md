---
title: Create an Integration
description: Learn how to create a WavyNode integration from scratch
---

# Create a WavyNode Integration

Integrations are a powerful way to connect your application with WavyNode. By creating an integration, you can receive real-time notifications about your users' on-chain activity and provide WavyNode with the necessary data to monitor their wallets.

This guide will walk you through the process of creating a new integration from scratch.

## Getting Started

There are two ways to create a new integration:

1.  **Using the template:** We provide a [template repository](https://github.com/wavy-node/integration) that you can use as a starting point. The template is a [Nitro](https://nitro.unjs.io/) application with all the required endpoints and authentication already set up.

2.  **From scratch:** You can create a new integration from scratch using any framework or language you prefer. This guide will focus on this approach.

## `@wavynode/utils` Package

We provide an npm package with utilities to help you with the integration process. You can install it using your favorite package manager:

```bash
npm install @wavynode/utils
```

This package includes the `validateSignature` function, which you will need to authenticate requests from WavyNode.

## Authentication

WavyNode uses a simple but secure authentication method to verify that incoming requests are from us. All requests sent to your integration will include the following headers:

*   `x-wavynode-hmac`: A base64 encoded HMAC-SHA256 signature of the request body.
*   `x-wavynode-timestamp`: The timestamp of the request in milliseconds.

To verify the signature, you will need to use the `validateSignature` function from the `@wavynode/utils` package. This function takes the following parameters:

*   `method`: The HTTP method of the request.
*   `path`: The path of the request.
*   `body`: The request body.
*   `timestamp`: The timestamp from the `x-wavynode-timestamp` header.
*   `secret`: Your integration's secret.
*   `timeTolerance`: The time tolerance in milliseconds. This is to prevent replay attacks. We recommend a value of `300000` (5 minutes).
*   `signature`: The signature from the `x-wavynode-hmac` header.

Here is an example of how to use the `validateSignature` function:

```typescript
import { validateSignature } from '@wavynode/utils';

const isValid = validateSignature({
    method: 'POST',
    path: '/webhook',
    body: req.body,
    timestamp: parseInt(req.headers['x-wavynode-timestamp']),
    secret: process.env.SECRET,
    timeTolerance: 300000,
    signature: req.headers['x-wavynode-hmac']
});

if (!isValid) {
    // The request is not from WavyNode
    // or has been tampered with.
    // You should reject the request.
}
```

## Endpoints

Your integration must expose the following endpoints:

### `GET /users/:userId`

This endpoint is used by WavyNode to retrieve information about a specific user. The `:userId` parameter will be the user's ID in your system.

Your endpoint should return a JSON object with the following properties:

[to be defined]


### `POST /webhook`

This endpoint is used by WavyNode to send you real-time notifications. The request body will be a JSON object with the following properties:

*   `type`: The type of the notification. This can be `notification` or `error`.
*   `data`: The notification payload.

#### Notification Payload

If the `type` is `notification`, the `data` object will contain the following properties:

[to be defined]

#### Error Payload

If the `type` is `error`, the `data` object will be a string containing the error message.


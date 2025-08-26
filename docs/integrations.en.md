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

To verify the signature, you can use the `validateSignature` function from the `@wavynode/utils` package. This function takes the following parameters:

*   `method`: The HTTP method of the request.
*   `path`: The path of the request.
*   `body`: The request body.
*   `timestamp`: The timestamp from the `x-wavynode-timestamp` header.
*   `secret`: Your integration's secret. You can find this in your project's settings in the WavyNode dashboard.
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

### Manual Authentication

If you are not using the `@wavynode/utils` package, you can implement the authentication logic yourself. Here is how to create the canonical string and the HMAC signature:

1.  **Create the canonical string:** The canonical string is a concatenation of the following, separated by `::`:
    *   The uppercase HTTP method (`GET`, `POST`, etc.).
    *   The lowercase request path (e.g., `/webhook`).
    *   The stringified request body with its keys sorted alphabetically.
    *   The timestamp from the `x-wavynode-timestamp` header.

    Here is an example of how to create the canonical string in JavaScript:

    ```javascript
    const sortObjectAlphabetically = (obj) => {
        const sortedKeys = Object.keys(obj).sort();
        const sortedObject = {};
        for (const key of sortedKeys) {
            if (typeof obj[key] === 'object' && obj[key] != null && !Array.isArray(obj[key])) {
                sortedObject[key] = sortObjectAlphabetically(obj[key]);
                continue;
            }
            sortedObject[key] = obj[key];
        }
        return sortedObject;
    };

    const formCanonicalMessage = ({ method, path, body, timestamp }) => {
        const sortedBody = sortObjectAlphabetically(body);
        return `${method.toUpperCase()}::${path.toLowerCase()}::${JSON.stringify(sortedBody)}::${timestamp}`;
    };
    ```

2.  **Create the HMAC signature:** The signature is a `sha256` HMAC of the canonical string, using your integration's secret as the key. The result should be base64 encoded.

    ```javascript
    import crypto from 'node:crypto';

    const createHmacSignature = (message, secret) => {
        const hmac = crypto.createHmac('sha256', secret);
        hmac.update(message);
        return hmac.digest('base64');
    };
    ```

3.  **Compare the signatures:** Compare the signature you created with the one from the `x-wavynode-hmac` header. If they are identical, the request is authentic.

## Endpoints

Your integration must expose the following endpoints:

### `GET /users/:userId`

This endpoint is used by WavyNode to retrieve information about a specific user. The `:userId` parameter will be the user's ID in your system.

Your endpoint should return a JSON object with the following properties:

*   `name`: The user's name.
*   `rfc`: The user's RFC.

Here is an example of a valid response:

```json
{
    "name": "Julio César Chávez",
    "rfc": "CHCJ990712ABC"
}
```

### `POST /webhook`

This endpoint is used by WavyNode to send you real-time notifications. The request body will be a JSON object with the following properties:

*   `type`: The type of the notification. This can be `notification` or `error`.
*   `data`: The notification payload.

#### Notification Payload

If the `type` is `notification`, the `data` object will contain the following properties:

*   `id`: The notification's ID.
*   `projectId`: The ID of the project this notification belongs to.
*   `chainId`: The ID of the chain where the transaction occurred.
*   `address`: An object containing information about the address involved in the transaction.
    *   `id`: The address' ID.
    *   `address`: The address.
    *   `description`: The address' description.
*   `txHash`: The transaction hash.
*   `timestamp`: The timestamp of the transaction.
*   `amount`: An object containing the transaction's amount.
    *   `value`: The amount in the token's smallest unit.
    *   `usd`: The amount in USD.
*   `token`: An object containing information about the token used in the transaction.
    *   `name`: The token's name.
    *   `symbol`: The token's symbol.
    *   `decimals`: The token's decimals.
    *   `address`: The token's address.
*   `inflictedLaws`: An array of objects containing information about the laws inflicted by the transaction.
    *   `name`: The law's name.
    *   `description`: The law's description.
    *   `source`: The law's source.
    *   `risk`: The law's risk. This can be `warn` or `illegal`.
    *   `country`: The law's country.
    *   `countryCode`: The law's country code.

#### Error Payload

If the `type` is `error`, the `data` object will be a string containing the error message.

Here is an example of a valid notification payload:

```json
{
    "type": "notification",
    "data": {
        "id": 1,
        "projectId": 1,
        "chainId": 42161,
        "address": {
            "id": 543,
            "address": "0xyour-address-involved",
            "description": "Your address' description"
        },
        "txHash": "some-tx-hash",
        "timestamp": "2025-08-20T05:10:57.228Z",
        "amount": {
            "value": 1000000000000000000,
            "usd": 3000
        },
        "token": {
            "name": "Ethereum",
            "symbol": "ETH",
            "decimals": 18,
            "address": null
        },
        "inflictedLaws": [
            {
                "name": "The name of the law inflicted",
                "description": "Description of the law",
                "source": "Source of the law",
                "risk": "warn",
                "country": "mexico",
                "countryCode": "MEX"
            }
        ]
    }
}
```


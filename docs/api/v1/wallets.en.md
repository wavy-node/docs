# Wallets
The `wallets` API allows developers to retrieve information about specific wallets, including their status and balance.

## Status
* **Description**: Retrieves the status of a specific `{wallet}`.
* **Endpoint**: `/v1/wallets/{wallet}/status`
* **Method**: GET
* **Request:**
    * **Route params:**
        * `wallet` (required): The EVM address to query
    * **Query params:**
        * `chainId` (required): The chain to perform the query on
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": {
                "status": "dirty",
                "tags": [
                    "tornado"
                ],
                "lastTxs": [],
                "lastDapps": []
            }
        }
        ```

## Report
* **Description**: Generates a written report about the status of a specific `{wallet}` using AI.
* **Endpoint**: `/v1/wallets/{wallet}/report`
* **Method**: GET
* **Request:**
    * **Route params:**
        * `wallet` (required): The EVM address to query
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": "(The AI-generated report in markdown)"
        }
        ```

## Balance
* **Description**: Retrieves the total balance of the `{wallet}`.
* **Endpoint**: `/v1/wallets/{wallet}/balance`
* **Method**: GET
* **Request:**
    * **Route params:**
        * `wallet` (required): The EVM address to query
    * **Query params:**
        * `chainId` (required): The chain to perform the query on
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": {
                "total_balance": 100,
                "assets": []
            }
        }
        ```

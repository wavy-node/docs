# Chains.
Provides information about the different chains supported by Wavy Node.

## All chains
* **Description**: Retrieves all the chains supported by Wavy Node.
* **Endpoint**: `/v1/chains`
* **Method**: GET
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": [
                {
                    "id": 42161,
                    "name": "Arbitrum",
                    "rpc_url": "https://arb1.arbitrum.io/rpc",
                    "explorer_url": "https://arbiscan.io",
                    "currency_symbol": "ETH",
                    "currency_decimals": 18
                }
            ]
        }
        ```

## One chain
* **Description**: Retrieves information about a specific chain
* **Endpoint**: `/v1/chains/{chainId}`
* **Method**: GET
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": {
                "name": "Arbitrum",
                "chain_id": 42161,
                "currency_symbol": "ETH",
                "market_cap": "329663113187.488651200",
                "coin_price": "2735.25",
                "coin_image": "https://coin-images.coingecko.com/coins/images/279/small/ethereum.png?1696501628",
                "total_addresses": "67136720",
                "total_blocks": "302652520",
                "total_transactions": "1292967936",
                "tvl": null
            }
        }
        ```

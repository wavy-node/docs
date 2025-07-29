# Chains
Presenta información acerca de las diferentes cadenas compatibles con Wavy Node.

## Todas las cadenas
* **Descripción**: Obtiene todas las cadenas compatibles con Wavy Node.
* **Endpoint**: `/v1/chains`
* **Método**: GET
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
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

## Una cadena
* **Descripción**: Obtiene información de una cadena en específico
* **Endpoint**: `/v1/chains/{chainId}`
* **Método**: GET
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
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

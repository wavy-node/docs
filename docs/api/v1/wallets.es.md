# Wallets
La `wallets` API le permite a los desarrolladores obtener información acerca de wallets específicas, obtener su estado y balance.

## Estado
* **Descripción**: Obtiene el estado de una determinada `{wallet}`.
* **Endpoint**: `/v1/wallets/{wallet}/status`
* **Método**: GET
* **Request:**
    * **Route params:**
        * `wallet` (requerido): La dirección EVM a consultar
    * **Query params:**
        * `chainId` (requerido): La chain sobre la cual hacer la consulta.
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
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

## Reporte
* **Descripción**: Genera un reporte redactado acerca del estado de una determinada `{wallet}` usando IA.
* **Endpoint**: `/v1/wallets/{wallet}/report`
* **Método**: GET
* **Request:**
    * **Route params:**
        * `wallet` (requerido): La dirección EVM a consultar
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": "(El reporte generado por IA en markdown)"
        }
        ```

## Balance
* **Descripción**: Obtiene el balance total de la `{wallet}`.
* **Endpoint**: `/v1/wallets/{wallet}/balance`
* **Método**: GET
* **Request:**
    * **Route params:**
        * `wallet` (requerido): La dirección EVM a consultar
    * **Query params:**
        * `chainId` (requerido): La chain sobre la cual hacer la consulta.
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": {
                "total_balance": 100, // en usd
                "assets": []
            }
        }
        ```

# Direcciones
Permite configurar las direcciones en cadena relevantes para un proyecto específico.

!!! info

    Las direcciones relevantes son aquellas por las que se enviarán notificaciones de compliance con las legislaciones y alertas de interacciones con direcciones maliciosas al webhook configurado.

## Todas las direcciones
* **Descripción**: Obtiene todas las direcciones de un proyecto.
* **Endpoint**: `/v1/projects/{projectId}/addresses`
* **Método**: GET
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
    * **Route params:**
        * `projectId` (requerido): El proyecto a consultar
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": [
                {
                    "id": 1,
                    "address": "0x...",
                    "description": "Mi primera dirección relevante"
                }
            ]
        }
        ```

## Crear nueva dirección relevante
* **Descripción**: Crea una nueva dirección relevante para escuchar eventos de un proyecto.
* **Endpoint**: `/v1/projects/{projectId}/addresses`
* **Método**: POST
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
    * **Route params:**
        * `projectId` (requerido): El proyecto a consultar
    * **Body:** (JSON)
        ```json
        {
            "address": "0x<tu-direccion>",
            "description": "Alguna descripción aquí"
        }
        ```
* **Response:**
    ```json
    {
        "success": true
    }
    ```

## Eliminar una dirección
* **Descripción**: Remueve una dirección relevante de la lista de direcciones relevantes del proyecto.
* **Endpoint**: `/v1/projects/{projectId}/addresses/{addressId}`
* **Método**: DELETE
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
    * **Route params:**
        * `projectId` (requerido): El proyecto a consultar
        * `addressId` (requerido): La dirección a eliminar
* **Response:**
    ```json
    {
        "success": true
    }
    ```

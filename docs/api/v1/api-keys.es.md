---
title: API keys
---

# API keys
Gestión de API keys por medio de la API para un proyecto específico.

!!! warning

    Esta documentación describe la `keys` API para proyectos. Si buscas información acerca de cómo crear una API key desde el dashboard lee [Uso de la API](/api/).

## Todas las API keys
* **Descripción**: Obtiene todas las API keys de un proyecto.
* **Endpoint**: `/v1/projects/{projectId}/keys`
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
                    "created_at": "2025-01-29T05:12:15+00:00",
                    "label": "Alguna etiqueta",
                    "key": "wavy_..."
                }
            ]
        }
        ```

## Crear API key
* **Descripción**: Crea una API key para un proyecto
* **Endpoint**: `/v1/projects/{projectId}/keys`
* **Método**: POST
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
    * **Route params:**
        * `projectId` (requerido): El proyecto a consultar
    * **Body:** (JSON)
        ```json
        {
            "label": "Alguna etiqueta"
        }
        ```
* **Response:**
    ```json
    {
        "success": true,
        "data": 1
    }
    ```

## Eliminar una API key
* **Descripción**: Elimina una API key de un proyecto
* **Endpoint**: `/v1/projects/{projectId}/keys/{keyId}`
* **Método**: DELETE
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (requerido): Tu API key
    * **Route params:**
        * `projectId` (requerido): El proyecto a consultar
        * `keyId` (requerido): La key a eliminar
* **Response:**
    ```json
    {
        "success": true
    }
    ```

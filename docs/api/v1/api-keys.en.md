---
title: API keys
---

# API keys
API key management through the API for a specific project.

!!! warning

    This documentation describes the `keys` API for projects. If you're looking for information on how to create an API key from the dashboard, read [Using the API](/en/api).

## All keys
* **Description**: Retrieves all the API keys for a given project.
* **Endpoint**: `/v1/projects/{projectId}/keys`
* **Method**: GET
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
    * **Route params:**
        * `projectId` (required): The project to query
* **Response:**
    * **Body:** (JSON)
        ```json
        {
            "success": true,
            "data": [
                {
                    "id": 1,
                    "created_at": "2025-01-29T05:12:15+00:00",
                    "label": "Some label",
                    "key": "wavy_..."
                }
            ]
        }
        ```

## Create API key
* **Description**: Creates an API key for a given project
* **Endpoint**: `/v1/projects/{projectId}/keys`
* **Method**: POST
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
    * **Route params:**
        * `projectId` (required): The project to query
    * **Body:** (JSON)
        ```json
        {
            "label": "Some label"
        }
        ```
* **Response:**
    ```json
    {
        "success": true,
        "data": 1
    }
    ```

## Delete an API key
* **Description**: Deletes an API key for a given project
* **Endpoint**: `/v1/projects/{projectId}/keys/{keyId}`
* **Method**: DELETE
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
    * **Route params:**
        * `projectId` (required): The project to query
        * `keyId` (required): The key to delete
* **Response:**
    ```json
    {
        "success": true
    }
    ```

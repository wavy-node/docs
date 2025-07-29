# Addresses
Allows you to configure relevant on-chain addresses for a specific project.

!!! info

    Relevant addresses are those for which compliance notifications and alerts about interactions with malicious addresses will be sent to the configured webhook.

## All addresses
* **Description**: Retrieves all the addresses for a given project.
* **Endpoint**: `/v1/projects/{projectId}/addresses`
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
                    "address": "0x...",
                    "description": "My first relevant address"
                }
            ]
        }
        ```

## Create new relevant address
* **Description**: Creates a new relevant address to listen for events for a given project.
* **Endpoint**: `/v1/projects/{projectId}/addresses`
* **Method**: POST
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
    * **Route params:**
        * `projectId` (required): The project to query
    * **Body:** (JSON)
        ```json
        {
            "address": "0x<your-address>",
            "description": "Some description here"
        }
        ```
* **Response:**
    ```json
    {
        "success": true
    }
    ```

## Delete an address
* **Description**: Removes a relevant address from the project's relevant address list.
* **Endpoint**: `/v1/projects/{projectId}/addresses/{addressId}`
* **Method**: DELETE
* **Request:**
    * **Headers:**
        * `x-api-header: ApiKey <api-key>` (required): Your API key
    * **Route params:**
        * `projectId` (required): The project to query
        * `addressId` (required): The address to delete
* **Response:**
    ```json
    {
        "success": true
    }
    ```

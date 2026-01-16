---
title: Crear una Integración
description: Aprende a crear una integración de WavyNode desde cero
---

# Crear una Integración de WavyNode

Las integraciones son una forma poderosa de conectar tu aplicación con WavyNode. Al crear una integración, puedes recibir notificaciones en tiempo real sobre la actividad en cadena de tus usuarios y proporcionar a WavyNode los datos necesarios para monitorear sus billeteras.

Esta guía te guiará a través del proceso de creación de una nueva integración desde cero.

## Empezando

Hay dos formas de crear una nueva integración:

1.  **Usando la plantilla:** Proporcionamos un [repositorio de plantilla](https://github.com/wavy-node/integration) que puedes usar como punto de partida. La plantilla es una aplicación [Nitro](https://nitro.unjs.io/) con todos los endpoints y la autenticación necesarios ya configurados.

2.  **Desde cero:** Puedes crear una nueva integración desde cero utilizando cualquier framework o lenguaje que prefieras. Esta guía se centrará en este enfoque.

## Paquete `@wavynode/utils`

Proporcionamos un paquete de npm con utilidades para ayudarte con el proceso de integración. Puedes instalarlo usando tu gestor de paquetes favorito:

```bash
npm install @wavynode/utils
```

Este paquete incluye la función `validateSignature`, que necesitarás para autenticar las solicitudes de WavyNode.

## Autenticación

WavyNode utiliza un método de autenticación simple pero seguro para verificar que las solicitudes entrantes provienen de nosotros. Todas las solicitudes enviadas a tu integración incluirán los siguientes encabezados:

*   `x-wavynode-hmac`: Una firma HMAC-SHA256 codificada en base64 del cuerpo de la solicitud.
*   `x-wavynode-timestamp`: La marca de tiempo de la solicitud en milisegundos.

Para verificar la firma, puedes usar la función `validateSignature` del paquete `@wavynode/utils`. Esta función toma los siguientes parámetros:

*   `method`: El método HTTP de la solicitud.
*   `path`: La ruta de la solicitud.
*   `body`: El cuerpo de la solicitud.
*   `timestamp`: La marca de tiempo del encabezado `x-wavynode-timestamp`.
*   `secret`: El secreto de tu integración. Puedes encontrarlo en la configuración de tu proyecto en el panel de WavyNode.
*   `timeTolerance`: La tolerancia de tiempo en milisegundos. Esto es para prevenir ataques de repetición. Recomendamos un valor de `300000` (5 minutos).
*   `signature`: La firma del encabezado `x-wavynode-hmac`.

Aquí hay un ejemplo de cómo usar la función `validateSignature`:

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
    // La solicitud no es de WavyNode
    // o ha sido manipulada.
    // Deberías rechazar la solicitud.
}
```

### Autenticación Manual

Si no estás utilizando el paquete `@wavynode/utils`, puedes implementar la lógica de autenticación tú mismo. Así es como se crea la cadena canónica y la firma HMAC:

1.  **Crear la cadena canónica:** La cadena canónica es una concatenación de lo siguiente, separado por `::`:
    *   El método HTTP en mayúsculas (`GET`, `POST`, etc.).
    *   La ruta de la solicitud en minúsculas (p. ej., `/webhook`).
    *   El cuerpo de la solicitud en formato de cadena con sus claves ordenadas **alfabéticamente**, o un objeto vacío `{}` si no se proporciona ningún cuerpo en la solicitud.
    *   La marca de tiempo del encabezado `x-wavynode-timestamp`.

    Aquí hay un ejemplo de una cadena canónica válida:

    ```
    GET::/users/123::{}::1757050233763
    ```

2.  **Crear la firma HMAC:** La firma es un HMAC `sha256` de la cadena canónica, utilizando el secreto de tu integración como clave. El resultado debe estar codificado en base64.

    ```javascript
    import crypto from 'node:crypto';

    const createHmacSignature = (message, secret) => {
        const hmac = crypto.createHmac('sha256', secret);
        hmac.update(message);
        return hmac.digest('base64');
    };
    ```

3.  **Comparar las firmas:** Compara la firma que creaste con la del encabezado `x-wavynode-hmac`. Si son idénticas, la solicitud es auténtica.

## Endpoints

Tu integración debe exponer los siguientes endpoints:

### `GET /users/:userId`

Este endpoint es utilizado por WavyNode para recuperar información sobre un usuario específico. El parámetro `:userId` será el ID del usuario en tu sistema.

Tu endpoint debe devolver un objeto JSON con las siguientes propiedades:

*   `givenName`: El nombre(s) de pila de la persona.
*   `maternalSurname`: El apellido materno de la persona.
*   `paternalSurname`: El apellido paterno de la persona.
*   `birthdate`: Fecha de nacimiento en formato 'YYYY-MM-DD' (ISO 8601).
*   `nationality`: Código de país ISO 3166-1 alfa-2.
*   `phoneNumber`: Un objeto que contiene el número de teléfono del usuario.
*   `email`: La dirección de correo electrónico del usuario.
*   `address`: Un objeto que contiene la dirección física del usuario.
*   `mexico`: (Opcional) Un objeto con campos requeridos para los reportes de legislación mexicana. Solo es necesario si tienes activada la conformidad con la legislación mexicana en tu panel de control.

Aquí hay un ejemplo de una respuesta válida:

```json
{
  "givenName": "Maria Guadalupe",
  "maternalSurname": "Sánchez",
  "paternalSurname": "Rodríguez",
  "birthdate": "1992-05-15",
  "nationality": "MX",
  "phoneNumber": {
    "countryCode": "+52",
    "phoneNumber": 5512345678
  },
  "email": "maria.guadalupe@example.com",
  "address": {
    "country": "MX",
    "region": "CDMX",
    "city": "Ciudad de México",
    "street": "Avenida Insurgentes Sur",
    "colonia": "Condesa",
    "exteriorNumber": "123",
    "interiorNumber": "4B",
    "postalCode": "06100"
  },
  "mexico": {
    "rfc": "ROSM920515XXX",
    "curp": "ROSM920515MDFRXXXX",
    "actividadEconomica": 612012,
    "cuentaRelacionada": "1234567890",
    "monedaCuentaRelacionada": 1,
    "documentoIdentificacion": {
      "tipoIdentificacion": 1,
      "numeroIdentificacion": "IDMEX12345678"
    }
  }
}
```

### `POST /webhook`

Este endpoint es utilizado por WavyNode para enviarte notificaciones en tiempo real. El cuerpo de la solicitud será un objeto JSON con las siguientes propiedades:

*   `type`: El tipo de la notificación. Puede ser `notification` o `error`.
*   `data`: El payload de la notificación.

#### Payload de Notificación

Si el `type` es `notification`, el objeto `data` contendrá las siguientes propiedades:

*   `id`: El ID de la notificación.
*   `projectId`: El ID del proyecto al que pertenece esta notificación.
*   `chainId`: El ID de la cadena donde ocurrió la transacción.
*   `address`: Un objeto que contiene información sobre la dirección involucrada en la transacción.
    *   `id`: El ID de la dirección.
    *   `userId`: El ID del usuario en tu sistema que es dueño de esta dirección.
    *   `address`: La dirección de la billetera.
    *   `description`: La descripción de la dirección.
*   `txHash`: El hash de la transacción.
*   `timestamp`: La marca de tiempo de la transacción.
*   `amount`: Un objeto que contiene el monto de la transacción.
    *   `value`: El monto en la unidad más pequeña del token.
    *   `usd`: El monto en USD.
*   `token`: Un objeto que contiene información sobre el token utilizado en la transacción.
    *   `name`: El nombre del token.
    *   `symbol`: El símbolo del token.
    *   `decimals`: Los decimales del token.
    *   `address`: La dirección del token.
*   `inflictedLaws`: Un arreglo de objetos que contienen información sobre las leyes infringidas por la transacción.
    *   `name`: El nombre de la ley.
    *   `description`: La descripción de la ley.
    *   `source`: La fuente de la ley.
    *   `risk`: El riesgo de la ley. Puede ser `warn` o `illegal`.
    *   `country`: El país de la ley.
    *   `countryCode`: El código de país de la ley.

#### Payload de Error

Si el `type` es `error`, el objeto `data` será una cadena que contiene el mensaje de error.

Aquí hay un ejemplo de un payload de notificación válido:

```json
{
    "type": "notification",
    "data": {
        "id": 1,
        "projectId": 1,
        "chainId": 42161,
        "address": {
            "id": 543,
            "userId": "user-in-your-db-123",
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
                "countryCode": "MX"
            }
        ]
    }
}
```

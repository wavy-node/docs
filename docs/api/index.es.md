# Uso de la API
## API key
Para hacer uso correcto de la API se necesita crear una API key desde el [dashboard de WavyNode](https://wavynode.com/dashboard) y agregarla a cada request usando el header HTTP `x-api-header`:
Por ejemplo:
```bash
curl -H "x-api-header: ApiKey tu-api-key" https://api.wavynode.com/v1/chains
```

> **Nota:** Algunas rutas de la API requieren que proporciones explícitamente el `projectId` en la URL. Puedes encontrar el `projectId` de tu proyecto en el dashboard de WavyNode, en la sección de configuración del proyecto ("Project Settings").


## Hacer una petición
* Endpoint base: `https://api.wavynode.com`
* Autenticación: Header `x-api-header` (ver arriba)

### Formato de respuesta
| Campo | Tipo | Opcional | 
|-------|------|----------|
| `success` | booleano | no |
| `data` | any | si |
| `message` | string | si |

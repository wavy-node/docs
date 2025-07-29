# API Usage

## API Key

To properly use the API, you need to create an API key from the [WavyNode dashboard](https://wavynode.com/dashboard) and include it in every request using the `x-api-header` HTTP header:

```bash
curl -H "x-api-header: ApiKey your-api-key" https://api.wavynode.com/v1/chains
```

> **Note:** Some API routes require you to explicitly provide the `projectId` in the URL. You can find your project's `projectId` in the WavyNode dashboard, under the project settings section ("Project Settings").


## Make a request
* Endpoint base: `https://api.wavynode.com`
* Authentication: `x-api-header` header (see above)

### Response format
| Field | Type | Optional | 
|-------|------|----------|
| `success` | boolean | no |
| `data` | any | yes |
| `message` | string | yes |
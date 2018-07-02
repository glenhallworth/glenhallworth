Title: Retrieving Esri/ArcGIS Maps Token in Dotnet Core
Published: 12/18/2017
Tags: [".NET Core", "Esri", "ArcGIS"]

---

[Esri](http://www.esri.com/data/streetmap) has a premium service which requires adding a token to REST API requests for services like batch geocoding. The service has a few gotchas. The sample code is:

```csharp
    public class EsriTokenResponse
    {
        [JsonProperty(PropertyName = "access_token")]
        public string AccessToken { get; set; }
        [JsonProperty(PropertyName = "expires_in")]
        public int ExpiresInMinutes { get; set; }
    }

    public class EsriClient
    {
        private readonly HttpClient _httpClient;
        private readonly string tokenUrl = "https://www.arcgis.com/sharing/rest/oauth2/token";
        private readonly string clientId = "add_client_id"; // update
        private readonly string clientSecret = "add_client_secret"; // update
        private readonly string grantType = "client_credentials";
        private readonly int expirationInMinutes = 120;

        public EsriClient(HttpClient httpClient)
        {
            _httpClient = httpClient;
        }
        public async Task<EsriTokenResponse> GetToken()
        {
            var url =
                $"{tokenUrl}?client_id={clientId}&client_secret={clientSecret}&grant_type={grantType}&expiration={ExpirationInMinutes}";
            var response = await _httpClient.PostAsync(url, null);
            var result =
                await response.Content.ReadAsStringAsync();
            var token = JsonConvert.DeserializeObject<EsriTokenResponse>(result);
            if (string.IsNullOrWhiteSpace(token.AccessToken)) // Esri does not respect HTTP status codes and will always return 200. It puts errors in the body.
            {
                throw new Exception("Could not retrieve Esri Token");
            }
            return token;
        }
    }
```

### Gotchas

[ArcGIS](https://developers.arcgis.com/documentation/core-concepts/rest-api/) has a REST-like API. It doesn't adhere strictly to REST best practices.

When retrieving the token, I did not expect to put all the parameters into the query string.

Error handling is more laborious than need be, it always returns HTTP 200 OK. For example, in the case of a bad request, the error details are in the body.

```javascript
{
    "error": {
        "code": 400,
        "error": "invalid_client_id",
        "error_description": "Invalid client_id",
        "message": "Invalid client_id",
        "details": []
    }
}
```

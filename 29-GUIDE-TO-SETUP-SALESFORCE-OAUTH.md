# Guide to Setting up Salesforce OAuth

1. Login to your Org
2. Go to `Setup | App Manager`
3. Click `New Connected App`
4. Specify the `Connected App Name`, `API Name`, and `Contact Email`.
5. Enable `Enable OAuth Settings`
6. Set the Callback URL to a success page URL. A localhost url will do but it has to be https `(e.g. https://localhost:3000)`.
7. Add all `Available OAuth Scopes` to `Selected OAuth Scopes`.
8. Optionally, Enter a URL for `Info URL`.
9. Click `Save`.
10. Go to your browser and enter the following url and parameters:
```
https://login.salesforce.com/services/oauth2/authorize?response_type=code
&client_id=YOUR_CLIENT_ID&redirect_uri=YOUR_REDIRECT_URI
```
- `YOUR_CLIENT_ID` is the Consumer Key from the Connected App info page
- `YOUR_REDIRECT_URI` is the callback URL specified earlier (e.g. http://localhost:3000)

11. Execute the URL in the browser
12. Assuming, you used localhost:3000, your address bar will change to something like:
```
https://localhost:3000/?code=aPrxshT49BthlzJeik7p6fUxxxxxxxxxxxxxxxdzhQQBhDbTb13LFeDkSYNT1AEHetgjQEMBsQ%3D%3D
```

14. Copy the code parameter
13. If you used google chrome, the end of the code will typically become `%3D%3D`. Change this to `==`.
```
aPrxshT49BthlzJeik7p6fUxxxxxxxxxxxxxxxdzhQQBhDbTb13LFeDkSYNT1AEHetgjQEMBsQ==
```

14. Create a `POST` request using any REST client (e.g. POSTMAN) with the following specifications:
```json
POST https://login.salesforce.com/services/oauth2/token
Content-Type: application/x-www-form-urlencoded

"grant_type" : "authorization_code",
"client_secret" : "YOUR CONSUMER SECRET",
"client_id" : "YOUR CONSUMER KEY",
"redirect_uri" : "YOUR CALLBACK URL",
"code" : "THE CODE YOU RECEIVED FROM THE LAST STEP"
```
- e.g. code `aPrxshT49BthlzJeik7p6fUxxxxxxxxxxxxxxxdzhQQBhDbTb13LFeDkSYNT1AEHetgjQEMBsQ==`

15. Execute and you will receive the token in the following format:
```json
{
    "access_token": "YOUR ACCESS TOKEN",
    "refresh_token": "REFRESH TOKEN",
    "signature": "xxxxxxxx=",
    "scope": "visualforce refresh_token wave_api custom_permissions openid chatter_api api id eclair_api full",
    "id_token": "A REALLY LONG ID TOKEN",
    "instance_url": "https://YOUR-ORG.salesforce.com",
    "id": "https://login.salesforce.com/id/xxxxxx/yyyyyy",
    "token_type": "Bearer",
    "issued_at": "1523268989479"
}
```

16. Test the access token by calling any API endpoint with the `Authorization Bearer YOUR ACCESS TOKEN` header.
```json
POST https://YOUR-ORG.salesforce.com/services/data/v42.0/sobjects/contact
Authorization: Bearer xxxxxxxxxx
```

---

## Reference
- [Defining Connected Apps](https://developer.salesforce.com/docs/atlas.en-us.212.0.api_rest.meta/api_rest/intro_defining_remote_access_applications.htm)
- [Understanding the Web Server OAuth Authentication Flow](https://developer.salesforce.com/docs/atlas.en-us.212.0.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm)
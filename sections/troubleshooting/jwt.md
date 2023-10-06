# Debugging JWT Authentication

Users are advised to test their algorithms for generating JWT Authentication headers by testing with [Hello World APIs](sections/auth/jwt-hello-world.md). The Hello World APIs are able to validate the header and also generate the SHA-256 hash based on a given payload.

?> **Note:** If JWKS endpoint is specified, the user may need to validate that APEX has [sufficient outwards connectivity to the JWKS endpoint](sections/troubleshooting/network) and using [valid CA certificate authority](sections/faqs/trusted-cert-authorities).

## Error Codes

If you encounter errors during the JWT authentication process, refer to the tables below to help you get more information on the root cause of the error.

?> **Note:** You can collapse the left sidebar navigation for a better view of the tables.

### Pre-authentication errors

| Errors| Error Code | Error Name | Error / Issue | Tested Result (Code / Error Body) | Comments |
|---|---|---|---|---|---|
| 403/432 | Client Error | Unable to download JWKS from endpoint. Please check that the URL is valid and is reachable. | 403 | |
| 433 | Client Error | Invalid JWKS in Application. Please ensure that JWKS is correct and is of the correct algorithm. | 433<br>Invalid JWKS in Application. Please ensure that JWKS is correct and is of the correct algorithm. | |
| 434 | Client Error | JWT header is missing. | 434<br>JWT header is missing. | |
| 435 | Client Error | Invalid JWT format. | 435<br>Invalid JWT format. Please double-check JWT with [JWT.io](https://jwt.io/). | |
| 436 | Client Error | Missing ‘iss’ claim. | 436<br>Invalid JWT format. Please ensure that iss claim is valid. | |
| 437 | Client Error | Unable to find Key Id in JWKS. | 437<br>1) Invalid JWT format. Please ensure that kid claim is valid.<br>2) Invalid kid claim. Please ensure that key ID exists in JWKS. | |
| 438 | Client Error | Invalid ‘alg’ claim - algorithm. | 438<br>Invalid JWT format. Please ensure that alg claim is valid and is of value ‘ES256’ or ‘RS256’. | Not RS256 or ES256<br>Missing alg+: (alg missing is not possible to test due to library) |
| 439 | Client Error | Invalid ‘typ’ claim or JWT Type. | 439<br>Invalid JWT format. Please ensure that typ claim is valid. | |
| 440 | Client Error | Missing API Key. | 440<br>Missing API Key in ‘iss’ claim. | |
| 441 | Client Error | Missing ‘iat’ claim. | 441<br>Invalid JWT format. Please ensure that iat claim is valid. | |
| 442 | Client Error | Missing ‘aud’ claim. | 442<br>Invalid JWT format. Please ensure that aud claim is valid. | |
| 443 | Client Error | Missing ‘jti’ claim. | 443 | |
| 445 | Client Error | Missing ‘sub’ claim. | 445 - Invalid JWT format. Please ensure that sub claim is valid. | |
| 446 | Client Error | Missing data hash claim. | 446 - Invalid JWT format. Please ensure that data claim is valid. | |
| 447 | Client Error | Missing ‘exp’ claim. | 447 - Invalid exp claim. Please ensure that exp claim exists and JWT is not expired. | 

### Authentication errors

| Error Code | Error Name | Error / Issue | Tested Result (Code / Error Body) | Comments |
|---|---|---|---|---|
| 450 | Client Error | Authentication Error - Invalid API Key | 450 Authentication Error - Invalid API Key | |
| 452 | Client Error | Authentication Error - Signature | 452 Authentication Error - JWT Signature. Ensure JWT is signed with correct key | |

## Common Issues / FAQs

<details>
<summary>How do we troubleshoot incorrect data hash (Error 446)?</summary>

Error 446 may mean that your data hash of your body is incorrect. Ensure that your data is [formatted correctly](sections/auth/jwt-auth?#apex-standardized-json-payload) (generally without spaces and carriage returns/new lines) and do test your hash values with [Hello World APIs](sections/auth/jwt-hello-world#sha-256-generator) /helloworld/sha256 with x-apex-returncontent and seeing the return value of x-apex-hash and the content returned.

If you are using SOAP and your payload has special characters, [raise a service request](https://go.gov.sg/apex-support).
</details>

<details>
<summary>How do we troubleshoot invalid iat claim (Error 441)?</summary>

This could be due to the "iat" issued at time claim, which is in the future compared to date/time of APEX.

There could be time skew at the Consumer's server/application causing it to be earlier than universal NTP servers, hence the Consumer server needs to be checked that it is synced correctly to global NTP clocks.
</details>

<details>
<summary>How do we troubleshoot JWT Header Missing (Error 443)?</summary>

The JWT header is not reaching APEX. Do register a new backend by going to this URL https://webhook.site and with the registered backend (eg. https://webhook.site/123456-1234-1234-1234-1234567890), use your application to call this. From your registered URL, you will be able to see if the header x-apex-jwt is indeed reaching the backend.
</details>

<details>
<summary>How do we troubleshoot Invalid JWKS (Error 433)?</summary>

Do check that the JWKS is formatted correct in JSON (use only double quotes, key values need to be double-quoted, curly brackets and square brackets are correct).
Also, do ensure that the JWKs have all the required keys - "x", "y", "use", "crv", "kid", "alg", "kty". Any of these omitted will cause an error.
</details>

<details>
<summary>How do we troubleshoot error 490-499?</summary>

Ensure that you are not reusing JWT tokens or using any forbidden headers. If you are still encountering issues, do open a troubleshooting case with APEX.
</details>

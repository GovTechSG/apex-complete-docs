# Introduction to JWT

JWT stands for JSON Web Token and is defined in [RFC7519](https://www.rfc-editor.org/rfc/rfc7519).

Generally it consists of 3 sections: **_Header, Payload_** and **_Signature_**, delimited by dot '**_._**'.

Example of JWT:

```JWT
eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt
cGxlLmNvbS9pc19yb290Ijp0cnVlfQ.dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

The **_Header_** and **_Payload_** are base64-encoded and consists of various claims which define the attribute of the JWT as well as the API request.

The **_Signature_** is a generated hash of the header and payload using a private key which the API requester owns.

JWTs can be generated using typical JWT libraries or by online [tools](https://www.scottbrady91.com/tools/jwt).

JWTs (Header or Payload) can be base64 decoded to be read, or read using an online tool such as [jwt.io](https://jwt.io).

Example of decoded JWT header and payload in the above example:

```JSON
{
  "typ": "JWT",
  "alg": "HS256"
}

{
  "iss": "joe",
  "exp": 1300819380,
  "http://example.com/is_root": true
}
```

An explanation of typical JWT claims used are as follows:

| S/No | JWT Claim | Comments                                                                                                       |
| ---- | --------- | -------------------------------------------------------------------------------------------------------------- |
| 1    | alg       | Signing algorithm of the JWT signature.                                                                        |
| 2    | typ       | Type of JWT.                                                                                                   |
| 3    | kid       | This is the key ID of the public key used by the API endpoint to validate the signature.                       |
| 4    | iat       | This is the epoch time in seconds issued at the of API call. This is usually generated by the signing library. |
| 5    | exp       | This is the epoch time in seconds of the expiry time of this JWT.                                              |
| 6    | jti       | This provides a unique identifier for the JWT. It is a random text string (nonce) for this JWT.                |
| 7    | iss       | This identifies the principal that issued the JWT.                                                             |
| 8    | aud       | This identifies the recipients that the JWT is intended for.                                                   |
| 9    | sub       | This identifies the principal that is the subject of the JWT.                                                  |

Other guides for JWT can be found [here](https://auth0.com/docs/secure/tokens/json-web-tokens) and [here](https://jwt.io/introduction).

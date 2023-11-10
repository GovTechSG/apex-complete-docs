# JWKS Endpoint

APEX Supports JWKS Endpoint in Internet and Intranet Zone as well as textbox entry, as a secure means to secure the API transaction between a Consumer (API Requester) and Publisher (owner of API endpoint).

## JWK

A JWK (JSON Web Key) is based on that as defined in Rfc 7517. A JWK represents a cryptographic key and APEX only supports Elliptic Curve key of P-256. A single key should consist of the following parameters:

| S/No | Parameters           | Comments                                                                                                                                                          |
| ---- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | kty (Key Type)       | This represents cryptographic algorithm family used with key. APEX accepts only "EC"                                                                              |
| 2    | crv (Curve Type)     | This identifies the type of curve specified. For APEX only "P256" is allowed. More information about this can be found in Rfc 7518.                               |
| 3    | use (Public Key Use) | This identifies the intended use of public key (such as "sig" for signature and "enc" for encryption). For APEX "sig" is used for JWT signing.                    |
| 4    | alg (Algorithm)      | This identifies the algorithm intended for use with the key. The supported algorithm used in APEX is ES256. More information about this can be found in Rfc 7518. |
| 5    | kid (Key ID)         | This is the Key ID used to match the specific key used to sign a specific JWT. There may be multiple keys in a JWKS of different Key IDs to support key rotation.  |
| 6    | x (x-coordinate)     | Public x-coordinates displayed in JWKS endpoint. This is the x-coordinate of the elliptical curve.                                                                |
| 7    | y (y-coordinate)     | Public x-coordinates displayed in JWK endpoint. . This is the y-coordinate of the elliptical curve.                                                               |
| 8    | d (d-coordinate)     | Private d- coordinate. This is the y-coordinate of the elliptical curve.                                                                                          |

For the elliptical curve (EC) algorithm supported by APEX, a public JWK consists of **only x and y coordinates** , which are Endian coordinates of the P-256 EC curve.

## JWKS

A JSON Web Key Set (JWKS) is a set (or array) of one or more JWK(s) of different Key IDs that may be used for signing as defined in Rfc 7517.

## Example of JWKS Endpoint

A public JWKS endpoint may be a publicly published endpoint of URL (https://<domain>/.well-known/keys.json).

APEX also supports manual input of the JWKS endpoint text of the Consumer in the Application screen of the Consumer's Apex Portal.

Below is an example of what a JWKS endpoint looks like. Do note that the "d" coordinate is omitted as it will define the private key.

!> **Note:** Signature verification will fail if any of the parameters below are absent (eg. use, kid, alg), if the JSON format (double-quotes of key and value pairs, as well as curly and square brackets) is not correct, or "keys" key-value is not in lower-case.

```JSON
{
    "keys": [
        {
            "kty": "EC",
            "crv": "P-256",
            "x": "usZhq9AL4aC-hkzGCBK3RuJjmxKE6zqEdFyp-tQ8kh4",
            "y": "wHI1r6rQCHQQSAdNxaJDA0Tw5Fq3B-icq-mbMVlLZA4",
            "use": "sig",
            "kid": "apex-example",
            "alg": "ES256"
        }
    ]
}
```

A sample code of how this can be programmatically generated can be found [here](sections/auth/jwt-sample#jwks-endpoint).

## Utilizing JWKS endpoint in the Intranet

To utilize JWKS endpoint in the intranet, it is recommended for the Consumer of the API to contact the API Publisher to allow the JWKS endpoint URL/IP to be whitelisted.

## Generating JWKS

While it is recommended to generate the JWKS programmatically to allow programmatic/automatic rotation of JWKS keys, it can also be generated from a website like [https://mkjwk.org/](https://mkjwk.org/) with specifications set as Curve: P-256, Key Use: Signature, Algorithm: ES256, Key ID: SHA-256.

An example of JWKS generation is shown in the [sample code section](sections/auth/jwt-sample#jwks-endpoint).

## JWK Rotation

It is recommended that the JWK generated in the [prerequisite](sections/auth/jwt-auth#prerequisites-jwks-endpoint) step used in the signing of JWT be rotated at least monthly. As the JWKS consists of a collection (array) of JWKs, rotation can be done by generating a new JWK with a new Key ID, and adding that to the published JWKS endpoint beforehand.

Utilizing a JWKS endpoint with [programmatic generation of JWKS](sections/auth/jwt-sample#jwks-endpoint) can allow an automatic key rotation.

?> **Note:** The caching time for an Application's JWKS is 1 hour.

If there are several JWKs published in the JWKS endpoint, it is possible to effect a seamless key (JWK) rotation by signing with the new published key and reflecting the new Key ID in the JWT.

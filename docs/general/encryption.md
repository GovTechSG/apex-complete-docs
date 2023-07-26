# Encryption

## Background

- There is interest amongst the APEX core team and tenants into payload encryption.

- The JSON Web Encryption (JWE) proposed leverages on the JWKS endpoint established in JWT Authentication (APEX Cloud), by publishing the public encryption key unto there. Hence the process of key exchange is not too dissimilar to that of TLS.

## JWE Flow Diagram

### Simple Flow

Consumer Sends API To APEX and APEX Forwards Request to Publisher (1)

→ Publisher encrypts plain-text response in signed nested JWT (2)

→ Publisher returns response to consumer (3)

→ Consumer decrypts and verifies nested JWT (4)

→ Consumer reads plain-text response (5)

### Detailed FLow

- Square denotes encryption related activities
- Circle denotes non-encryption related activities
- Dotted line denotes reliance on JWKS endpoints.

![jwe-flow-diagram](./image/encryption/jwe-flow-diagram.png)

## APEX Recommendations and Security Considerations

| Encryption Algorithm Recommended | Purpose                              |
| -------------------------------- | ------------------------------------ |
| ECDH-ES+A256KW                   | Encrypt Content Encryption Key (CEK) |
| AES256GCM                        | Encrypt Plain-text                   |

## Pros of using JWE with Nested JWT

| Recommendation/Advantage                                                                                                                                                       | Comment                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hard-set of strong encryption algorithms used for payload encryption (AES256GCM) and content encryption key (CEK) encryption (ECDH-ES+A256KW)                                  |                                                                                                                                                                                   |
| Algorithm recommended has message integrity tag to ensure integrity of ciphertext, IV, AAD and hence encryption key                                                            |                                                                                                                                                                                   |
| Nonce used in encryption (IV) and randomly generated session encryption key to inject randomness                                                                               |                                                                                                                                                                                   |
| Nested JWT ensures non-repudiation of source of payload                                                                                                                        | It should use pre-agreed JWKS endpoint URLs and these JWT parameters should be disallowed - jwk, jku, x5u, x5c, x5t as these potentially introduce rogue signing/encryption keys. |
| Sending of key thumbprint validation in API request into JSON body (cnf→jkt), if appropriate and authorisation header (body of x-apex-jwt to add cnf->jwt) of request payload. |                                                                                                                                                                                   |
| APEX does not keep any private keys and is not involved in encryption process, hence there is no chance of snooping on contents.                                               |                                                                                                                                                                                   |

## Cons of using JWE with Nested JWT

| Disadvantage                                | Comment                       |
| ------------------------------------------- | ----------------------------- |
| JWKS endpoint needs to be set up beforehand |                               |
| Computationally expensive                   | Proper security is expensive. |

| Optional Recommendations                                                                                                                                     | Comment                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| Publishing of the encryption keys (in JWKS endpoints) of the Publisher and Consumer could be IP restricted to the other respective parties and not publicly. |                                                                                                             |
| Consumer could use the manual input for JWKS in the APEX dev portal                                                                                          | In the event chances of both Man-in-the-Middle attack and DNS poisoning occurring simultaneously is likely. |
| JWKS plug-in for intranet users to host JWKS                                                                                                                 | Please discuss with APEX Team                                                                               |
| JWKS plug-in for thumbprint validation and sending public encryption key to intranet users                                                                   | Please discuss with APEX Team                                                                               |

## Sample JWKS endpoint with encrpytion key

```
{
    "keys": [
        {
            "kty": "EC",
            "crv": "P-256",
            "x": "XW_TOC8n9z-0E3m6H-Z-22g1rHfnbxxA0dDKx2W2rVg",
            "y": "FREx0UMOlNHEoXC-JU7aQ71g5po6bP6A5CrhL_DaVic",
            "use": "enc",
            "kid": "apex-enc-v1",
            "alg": "ECDH-ES+A256KW"
        },
        {
            "kty": "EC",
            "crv": "P-256",
            "x": "EmXFLSBhh2-j-UnXI4V5OkQ-zbAtpvqPq_yd2HxoudU",
            "y": "KkjUcFLD12FMnGZeoyumL9Q3YVoibiq4ArnPgZd1aZg",
            "use": "sig",
            "kid": "apex-sig-v1",
            "alg": "ES256"
        }
    ]
}
```

## JWE Components

| Component                 | Purpose                                                                    | Encoding                          | Details                                                                                                                                                            |
| ------------------------- | -------------------------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| JWE Protected Header      | Provides details and methods of carrying out the encryption                | BASE64URL(UTF8(Protected Header)) | Contains details such as - alg: CEK key wrapping/encryption algorithm, enc: Payload encryption algorithm, epk: Ephemeral public key used in ECDH-ES key agreement. |
| JWE Encryption Key        | Specifies the actual encryption key (which itself is encrypted)            | BASE64URL(Encrypted CEK)          | Randomly generated key, also known as Content Encryption Key (CEK) (encrypted with the JWKS Public Encryption Key available in JWKS endpoint)                      |
| JWE Initialization Vector | Introduces randomness as to the encryption process                         | BASE64URL(Initialization Vector)  |                                                                                                                                                                    |
| JWE Ciphertext            | The actual encrypted payload                                               | BASE64URL(Ciphertext)             | Payload encrypted with Contented Encryption Key, encryption algorithm, Initialization Vector and Additional Authenticated Data.                                    |
| JWE Authentication Tag    | Output from encryption process which provides integrity to the encryption. | BASE64URL(Authentication Tag)     |                                                                                                                                                                    |

## Sample JWE Payload

Below is a sample JWE payload with line-breaks added between sections.

```
eyJhbGciOiJFQ0RILUVTK0EyNTZLVyIsImVuYyI6IkEyNTZHQ00iLCJlcGsiOnsieCI6Im45M1l3bzg5bWZDY2FxMTRqa09UeVpvR21FMkhpbGVxMW16eWV1NDh3QnMiLCJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieSI6InZCWW0xZDdXaEdpSXl5LUFPaTdMcVFfcnQzODBjcFhPZ0ZnaUd4MW42TjAifX0.
6HxUlNCtUB2KclFwE91-rJ3wIRgijXQqn938Wps9oJpqOtjNe5FBwQ.
4BBJKX0TksGmrbZ_.
AGqNFsCB8ugANLiKKPmuGTZGWDN337oX5hbA2voujVYtKS8drkQXqxYOSa2Wda5viN3FFzDk1mnHyF74YrJmkSnVHDDptss_0wRRCpLDOu5ZrI-qWwqS1w7FcnrJEzsLPUQXViga_Acq6RmsSy4_tH_RauxTU2JUzoUiADwtAUGo2SZPPM8qpV1eT9UWS4ejf5GT6QOVRi_XR9hZEstXgHkiJRsR1Em9w0WRWo5TlxZllqc3QaPS27h0e4kk6XJ8BfdvMLjBEu5kjgKGQehKPLnj1gTl7oKqUDZ2PvWWAfdaKnltR7iOXbNVgSm6XuqJiGRFuv76wgCL1czG5l3pMIj6ATa9f9bK8eP0tDvs1ELaR77MJ0GoLXP1T9CS24F2zsQR3_d-I0Gro23ORuaRCEAjxuv5dWmgl9TB38rT8geST9R2uK54ejtPRBit79A_PW3pU3e72_JyC7eeARa1mbU3zov2qcYDstlge8HhxydRQaQNVEaJzqjSpMsSzMGdIskdnIsRWLppgXcKOkmxsKGOTAgGlFszpUxNeytGaqi6NuMcqm2W07BjxwlWQwK98bKha1Ur9Ij2STls952nvsY872HC26D6Qh1y0KMKxY2d3jtqmv0BFYAEFEsZ-U93uk08kd7rOGw0j121kY82GVPU5MyE-GcCEuEzpshKpaEGIRDER6WYWHJi3BAwt61puOZQqIqtgUZrMLju83CmPZx_CKT-1GYw3U-TjZuauCiWWYW5ksCNaMu2baEa5ScLl_HbFmVOsZLzx94gAnkVyfs3UuulHS5GsyKu9HhGvCm72gKqk3s4UOMDtmP-EH8k1Z-Equ2tF9Vt3hLOnUM.
zPrHFrEqJiJFKttbO7g7WA
```

## Sample Code

- The Sample Code is available only for NodeJS and does not include validation and other security mechanisms.

- This includes both the Publisher's code to encrypt the payload (variable "text" is the plaintext API Response), as well as Consumer's Code to decrypt the received JWE. An example of generating the encryption key thumbprint and attaching it to body JSON is also included.

- Do note that this is for reference only and is not intended for production use.

```
// DEPENDENCIES
const jose = require("jose");
const {v4: uuidv4} = require('uuid');
const jwt = require('jsonwebtoken');

/*
    ***** FUNCTION TO RETURN base64 OUTPUT *****
*/
function base64(text) {
  return(Buffer.from(text).toString('base64'))
}

/*
    ***** FUNCTION TO RETURN base64 DECODED OUTPUT *****
*/
function base64Decode(base64Text) {
  return(Buffer.from(base64Text, 'base64').toString('ascii'))
}
/*
    ***** FUNCTION TO RETURN STRINGIFY OUTPUT *****
*/
function stringify(json) {
  return JSON.stringify(json,null);
}
/*
    ***** FUNCTION TO RETURN PRIVATE KEY IN PKCS8 FORMAT *****
*/
const importKey = async(key) => {
  const importedKey = await jose.importJWK(key, 'ES256')
  const privateKeyPKCS8 = await jose.exportPKCS8(importedKey);
  return privateKeyPKCS8;
}

/*
    ***** FUNCTION TO GENERATE JWT ******
*/
const getJWT = async (iss, sub, kid, aud, text, privateKey, thumbprint) => {

  /*
      ***** GET THE PKCS8 Signing Key *****
      This can be from a file or generated by a JWK in Endian format.
  */
  // const caPrivateKey = fs.readFileSync('./private.key', 'utf8');
  const caPrivateKey = await importKey(privateKey);

  const signOptions = {
    algorithm: 'ES256',
    keyid: kid,
    expiresIn: '180s',
    jwtid: uuidv4(),
    issuer: iss,
    audience: aud,
    subject: sub,
  };

  const payload = {

    // BASE64 of the payload
    payload: base64(text),

    // Thumbprint of the encryption key
    cnf: {
      jkt: thumbprint
    }
  };

  /*
      ***** CREATE JWT *****
      The JWT is created here to be used for the authentication header.
  */
  const jwtAuth = jwt.sign(payload, caPrivateKey, signOptions);
  const jwtDecoded = jwt.decode(jwtAuth,{complete:true});
  console.log(`Publisher Generated Nested JWT-\n${jwtAuth}\n`);

  return jwtAuth;
};

/*
    ***** CREATE JWE *****
    The JWE is created here.
*/
const encryptFunction = async function(generatedJwt,publicKey){

  const jwe = await new jose.CompactEncrypt(
    new TextEncoder().encode(generatedJwt),
  )
    .setProtectedHeader({ alg: 'ECDH-ES+A256KW', enc: 'A256GCM' })
    .encrypt(publicKey)
  console.log(`Publisher JWE-\n${jwe}\n`)
  return jwe;
}

/*
    ***** DECRYPT JWE *****
    The JWE is decrypted here.
*/
const decryptFunction = async function(jwe,privateKey){

  const { plaintext, protectedHeader } = await jose.compactDecrypt(jwe, privateKey)
  const decryptedJwt = await new TextDecoder().decode(plaintext);

  const jwtDecoded = await jwt.decode(decryptedJwt,{complete:true});
  const nestedJwt = stringify(jwtDecoded);
  const actualPayload = base64Decode(jwtDecoded.payload.payload);

  console.log(`Consumer Decrypted Nested JWT-\n${stringify(jwtDecoded)}\n`);
  console.log(`Consumer Decrypted Actual Payload-\n${actualPayload}\n`);

  return {decryptedJwt, nestedJwt, actualPayload}
}

// Execution of the program
const api = async function(){

  // This is encryption key from Consumer's JWKS endpoint
  const publicEncryptionKey={
    kty: 'EC',
    crv: 'P-256',
    use: 'enc',
    kid: 'apex-example-encrypt',
    x: 'lZU3Ic1QHBE5Ch9YajxQlqPicJL8lemiWfJga13RZrI',
    y: 'ddqibUSW8DiYexc4IUokdPYEcq5UO9grbaj13PkHGhM',
  }

  // Private key belonging to Consumer
  const privateEncryptionKey={
    kty: 'EC',
    crv: 'P-256',
    use: 'enc',
    kid: 'apex-example-encrypt',
    x: 'lZU3Ic1QHBE5Ch9YajxQlqPicJL8lemiWfJga13RZrI',
    y: 'ddqibUSW8DiYexc4IUokdPYEcq5UO9grbaj13PkHGhM',
    d: 'LUiL_tup7W-vapMlu2NpyTtFv73H1zETj-Oyr8UChzY'
  }

  // This is signing key from Publisher's JWKS endpoint
  const publicSigningKey = {
    kty: 'EC',
    crv: 'P-256',
    x: 'usZhq9AL4aC-hkzGCBK3RuJjmxKE6zqEdFyp-tQ8kh4',
    y: 'wHI1r6rQCHQQSAdNxaJDA0Tw5Fq3B-icq-mbMVlLZA4',
    use: 'sig',
    kid: 'apex-example-sign',
    alg: 'ES256',
  };

    // Private key belonging to Publisher
    const privateSigningKey = {
    kty: 'EC',
    crv: 'P-256',
    x: 'usZhq9AL4aC-hkzGCBK3RuJjmxKE6zqEdFyp-tQ8kh4',
    y: 'wHI1r6rQCHQQSAdNxaJDA0Tw5Fq3B-icq-mbMVlLZA4',
    d: 'w55YEByLRumO-Rnsc8jg2_MaYXfEiT_ioFVoGgrCTlg',
    use: 'sig',
    kid: 'apex-example-sign',
    alg: 'ES256',
  };

  // Variables for Publisher to carry out Nested JWT Signing
  const issuer = 'EXAMPLE PUBLISHER ORG-PROJECT TEAM';
  const subject = 'EXAMPLE PUBLISHER ORG-PROJECT TEAM';
  const keyId = publicSigningKey.kid
  const audience = 'https://public.api.gov.sg/agency/api';
  const thumbprint = await jose.calculateJwkThumbprint(publicEncryptionKey);
  const text = `It's a dangerous business, Frodo, going out your door.`;

  // PUBLISHER CREATES NESTED JWT
  // Assuming that the Publisher has already gotten the Consumer's encryption key from JWKS endpoint
  const generatedJwt = await getJWT(issuer, subject, keyId, audience, text, privateSigningKey, thumbprint);

  const josePublicKey = await jose.importJWK(publicEncryptionKey,'ES256')
  const josePrivateKey = await jose.importJWK(privateEncryptionKey,'ES256')

  // PUBLISHER CREATES ENCRYPTED JWE
  const encrypted = await encryptFunction(generatedJwt, josePublicKey);

  // CONSUMER DECRYPTS JWE
  // Do note that validation of Nested JWT and other security measures are not in the sample code
  const {decryptedJwt, jwePayload, nestedJwt, actualPayload} = await decryptFunction(encrypted, josePrivateKey);
}

api();
> node encrypt.js
Publisher Generated Nested JWT-
eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImFwZXgtZXhhbXBsZS1zaWduIn0.eyJwYXlsb2FkIjoiU1hRbmN5QmhJR1JoYm1kbGNtOTFjeUJpZFhOcGJtVnpjeXdnUm5KdlpHOHNJR2R2YVc1bklHOTFkQ0I1YjNWeUlHUnZiM0l1IiwiY25mIjp7ImprdCI6Imdyb1RQQzlteXJiMTBTeDFEbElZWTFkOEhzbnEyZDYxN3QtM0lldGxqSm8ifSwiaWF0IjoxNjcyOTAwMzI0LCJleHAiOjE2NzI5MDA1MDQsImF1ZCI6Imh0dHBzOi8vcHVibGljLmFwaS5nb3Yuc2cvYWdlbmN5L2FwaSIsImlzcyI6IkVYQU1QTEUgUFVCTElTSEVSIE9SRy1QUk9KRUNUIFRFQU0iLCJzdWIiOiJFWEFNUExFIFBVQkxJU0hFUiBPUkctUFJPSkVDVCBURUFNIiwianRpIjoiNWZiODgzMTctOTAzYS00ZjlmLWFmNmEtZjhjN2UzZDVhMzliIn0.25fsdMx4kr1y1fhpzofO-iFbBmB9P5eVab0I9LYLl3il297s9HrPwvM1mpv3rRRdExGPWzJ8lHvky07pbhK4tQ

Publisher JWE-
eyJhbGciOiJFQ0RILUVTK0EyNTZLVyIsImVuYyI6IkEyNTZHQ00iLCJlcGsiOnsieCI6IjgxSjhJYlVsRFBtRmtNc0RXX09QbS0tbDVySHlCTTd1d0ZPSDhpX0NvUTgiLCJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieSI6IkE0eDcwbU0xNGZudG9Za1pYWTNkM0NjMmh0VlFfZ2l2WWlFOWkxdlZCMVkifX0.LgkjxY-PtWHMnYNvNxBETAjC0uYkI02xRVsh5m6DNlA3GaYQOOV8RQ.OFOE59rYVFU5v1p1.0BwAUZ-BcMnkjaY18X3FqqrpDvqCK8WMS0Fx8T4TMqC0o_PhwW4tAZP6q_JPbxDZA2bL_iMIKAowBOa5GxgzUT3d14L82h1AUnklSxTMc8qPQc7xQ_P0OD-RVBNUBpJd5kaeB8XlkZpN5LQ3xV3yJUu3ByGe0f5ubjVHoMF9sB5mT1HZmXZproyKpOJmLw5AdCtFMJbjtC4ItmN7YOjr4vy0rB5BVd5AIx30pRAeEkOmjMR8nD_HUPu1WD-H9LErBvxIIr-mRSPArCWHyerilIzFiNcDnznWu7jWtyO4kWj1fvwjAY3Z1fvWAvBgn7U3JoB5qG9tX5XbOuC0zf4ux16mLr6NAUdEnQxaFUgDc2hZ4MSaqJ2GC1phGlYB4LVpivmDr3ZgJHZxg5GHGDpG49IKIyST1Tzg4IZ1FdKeD8NoObexZxs8EdVepKlMgA2SBeHAMUpaW3uKGZwb94pKeVoe_5FS9Nj9UwbVM5o1dPgMrwOUbApi6U7YecZHM3SzQmk7DQ9KCCbC3DF_lI0kN0FpwCWyLbJTc29-YJFmKvQh2JVwavK-_3_DMvB5oCkfdRSI2Zg8q99TBVI8kBhg61G9Tc_ITPAwmLUoQ92X8r_ZkTZmXb5k361DI00lO_ev_RyljQaPCqFSBmTsyIKzOQcEhRkdlww4nRce-Eq9C79zuwIRpM9VuZKPXtwgpG8edLsqGe42f235ovkDM9daXGbbxeWauIz3mpasKgCRkbsfzWZlTwoDfqOxoAamaNmiOVslEvdNjnyu-G1BrQ6PrhOcBSU6vNnmEzdmP18wDsyPdXRZPX7VefrPfAoPLyFGaNykcD3oz-QWFg.8OqOVUqAhAGjw3vQ0VAXZA

Consumer Decrypted Nested JWT-
{"header":{"alg":"ES256","typ":"JWT","kid":"apex-example-sign"},"payload":{"payload":"SXQncyBhIGRhbmdlcm91cyBidXNpbmVzcywgRnJvZG8sIGdvaW5nIG91dCB5b3VyIGRvb3Iu","cnf":{"jkt":"groTPC9myrb10Sx1DlIYY1d8Hsnq2d617t-3IetljJo"},"iat":1672900324,"exp":1672900504,"aud":"https://public.api.gov.sg/agency/api","iss":"EXAMPLE PUBLISHER ORG-PROJECT TEAM","sub":"EXAMPLE PUBLISHER ORG-PROJECT TEAM","jti":"5fb88317-903a-4f9f-af6a-f8c7e3d5a39b"},"signature":"25fsdMx4kr1y1fhpzofO-iFbBmB9P5eVab0I9LYLl3il297s9HrPwvM1mpv3rRRdExGPWzJ8lHvky07pbhK4tQ"}

Consumer Decrypted Actual Payload-
It's a dangerous business, Frodo, going out your door.
```

## References

- Information about JWE - RFC 7516 (https://datatracker.ietf.org/doc/rfc7516/)

- Information about proof of JWK using thumbprint - DPoP (https://datatracker.ietf.org/doc/html/draft-ietf-oauth-dpop)

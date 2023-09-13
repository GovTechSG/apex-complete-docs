# Introduction to Hello World!

Hello World! APIs are used to test authorization header (**_x-apex-jwt_**), to help developers in testing their signing codes.

## Hello World! using JWT Authentication

This API tests that the JWT in **x-apex-jwt** header is valid and will return with a Hello World! if successfully authenticated.

Example API Test:

```
POST /helloworld/jwt
Host: public-stg.api.gov.sg
x-apex-jwt: eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImFwZXgtZXhhbXBsZSJ9.eyJkYXRhIjoiY2M1NzVjNGVkNTU3NDgxZTMxZDlhMmEwNTgwYmM0NjRlODRiM2E3OWM1ZmM5NGU0ZmQ5NGJhMzNiM2U1NGRiYyIsImlhdCI6MTY2NzAyMDM2MSwiZXhwIjoxNjY3MDIwNTQxLCJhdWQiOiJodHRwczovL3B1YmxpYy1zdGcuYXBpLmdvdi5zZy9hZ2VuY3kvYXBpIiwiaXNzIjoieHh4eHh4eHgteHh4eC14eHh4LXh4eHgteHh4eHh4eHh4eHgseXl5eXl5eXkteXl5eS15eXl5LXl5eXkteXl5eXl5eXl5eXkiLCJzdWIiOiJQT1NUIiwianRpIjoiZWZhNjZlMWQtNjNjMS00MGViLWFkMWMtZmVkMTQ5OGYxMWU3In0.UzQzgMlFWJ-fSbRkq7SZu0QVtzgAVpEfVtjJYls4oRgmrtNAz4Dla1J5kqBmWt2V9Q-QoBm_6r1KZZ97qZtO4Q

Response:
HTTP/1.1 200 OK
Hello World!
```

<!-- TODO: Include Swagger and screenshot -->

## SHA-256 Generator

This API evaluates the sha-256 of the API payload based on the binary content and returns it in the header x-apex-sha256. By default, the API payload is returned in the response body.  If **x-apex-returncontent** is included as a header with any value, the API payload is not returned.

Example API Test:

```
POST /helloworld/sha256
Host: public-stg.api.gov.sg
x-apex-apikey: xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
{"payload":"data"}

Response:
HTTP/1.1 200 OK
x-apex-sha256: cc575c4ed557481e31d9a2a0580bc464e84b3a79c5fc94e4fd94ba33b3e54dbc

{"payload":"data"}
```

Example API Test with x-apex-returncontent:

```
POST /helloworld/sha256
Host: public-stg.api.gov.sg
x-apex-apikey: xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
x-apex-returncontent: false
{"payload":"data"}

Response:
HTTP/1.1 200 OK
x-apex-sha256: cc575c4ed557481e31d9a2a0580bc464e84b3a79c5fc94e4fd94ba33b3e54dbc
```

<!-- TODO: Include Swagger and screenshot -->

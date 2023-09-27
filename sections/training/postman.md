# Postman Scripts Distribution

Last Updated Date: **11 Aug 2023**

## Downloading the files

Please download the zip file containing the Postman scripts using this [**link**](https://go.gov.sg/apex-training-postman-script).

The downloaded zip file will contain the following JSON files:
1. APEX Cloud JWT Tester v1.3.postman_collection.json
2. APEX Cloud JWT Tester v1.3.postman_environment.json

## Importing the files

**Step 1:** Unzip and import the files into Postman

![Importing the Postman Scripts](./_assets/postman-vid1.gif)

**Step 2:** Verify that both files have been imported into Postman

![Verifying the Postman Scripts](./_assets/postman-vid2.gif)

## Setting up the Environment file

a. **jwks:** Use the private key (with 'd' coordinates). Can input one or multiple jwk.<br>
b. **api_key:** For cross-zone API call, need to enter 2 API keys ('apiKey1','apiKey2').<br>
c. **keyId:** For choosing which jwk to use in jwks variable. It is the 'kid' claim in the jwk.<br>
d. **DEBUG:** Set to 'true' to log more info in the Postman console for each API call.

![Setting Environment File](./_assets/postman-vid3.gif)

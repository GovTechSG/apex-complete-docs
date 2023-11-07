# Creating or editing an application

Applications allow app developers to generate credentials (API Key, OAuth) to consume APIs that are protected by authentication.

To create or edit an application, navigate through either flows:
 - **Applications** &rarr; **New Application** or **Create Application**
 - **Applications** &rarr; **[Existing application] &rarr; Edit Details**


![Image](./_assets/apps.png)

## Step 1 - Creating/Edit the Application

In the **Create New Application** or **Edit Application details** page, enter the application details.

<!-- 
![Image](./_assets/new-app-filled.png) -->

### Add the application information

| Field            | Description                                                                                                  | Remarks                    |
| ---------------- | ------------------------------------------------------------------------------------------------------------ | -------------------------- |
| Application Name | The unique name that identifies your application                                                             | Required                   |
| Organization     | The organization that the applciation will be in.                                                                     | Required                   |
| Description      |  Give more context on the app you are creating.                                                                  | Required                   |
| Phone            | Phone contact information for your application.                                                                       |                            |
| Email            | Email contact information for your application.                                                                       |                            |

![Image](./_assets/new-app-filled-info.png)

### Select a method to get JWKS 

#### Option 1 - Add the JWKS URL or endpoint

1. Enable the **Get JWKS by endpoint** toggle or select the **URL** option.
2. Enter the **JWKS endpoint** or **JWKS URL** that hosts the JWKS `.json`. Be sure to leave the JWKS field blank.

![Image](./_assets/new-app-jwks-endpoint.png)

For more information, refer to the [JWKS endpoint guide](https://docs.developer.tech.gov.sg/docs/complete-apex-user-guide/sections/oauth/create-jwks-endpoint).



#### Option 2 - Add the JWKS JSON directly

1. Disable the **Get JWKS by endpoint** toggle or select the **JSON** option.
2. In the **JWKS** or **JSON** field, enter the actual JWKS, in the JSON format: `{ "keys": [ {JWK_1}, {JWK_2} ] }`. Be sure to leave the endpoint or URL field blank.

![Image](./_assets/new-app-jwks-json.png)

For more information, refer to the [JWKS guide](https://docs.developer.tech.gov.sg/docs/complete-apex-user-guide/sections/auth/jwks).

## Step 2 - Requesting approval for the newly created application (If required)

If you are not an organization admin, and when you have successfully submitted the form to create a new application, you will notice that your new application is in the pending state.

![Image](./_assets/new-app-pending.png)

Here, you will not be able to continue the application workflow (creating API Keys, subscribing to APIs) until an organization admin approves your application.

> Do check in with your own organization admins and remind them if they have yet to approve your application through their inbox:
>
> ![Image](./_assets/pending-app-1.png) ![Image](./_assets/pending-app-2.png)

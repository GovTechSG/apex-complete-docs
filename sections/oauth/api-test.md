# Business API Test

> Before continuing, please ensure that you have already prepared:
>
> - [At least 1 application with an OAuth 2.1 Client](/sections/oauth/client.md)
> - [Understanding and implementing the Authorization Code flow](sections/oauth/authz-token)

The Publisher API Specs can usually be found [in our API Catalog](https://services-dev.api.developer.tech.gov.sg/approved-apis) (or if not, contact the Publisher directly).

The Business API request through APEX requires these additional headers below.

| Headers       | Definition                                                |
| ------------- | --------------------------------------------------------- |
| Authorization | Authorization Token obtained from Authorization Code flow |
| x-apex-apikey | API Key of your application obtained from Step 3 above    |
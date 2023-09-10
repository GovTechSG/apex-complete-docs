# Business API Test

> Before continuing, please ensure that you have already prepared:
>
> 1. [At least 1 application with an OAuth 2.1 Client](/sections/oauth/client.md)
> 1. [Understanding and implementing the Authorization Code flow](sections/oauth/authz-token)

The Publisher API Specs can usually be found [in our API Catalog](sections/consuming/browsing-apis?id=_1-discover-apis-through-the-api-catalog) (or if not, contact the Publisher directly).

The Business API request through APEX requires these additional headers below.

| Headers       | Definition                                                |
| ------------- | --------------------------------------------------------- |
| Authorization | Authorization Token obtained from Authorization Code flow |
| x-apex-apikey | API Key of your application obtained from Step 3 above    |

# Testing Business API

> Before continuing, please ensure that you have:
>
> - [At least 1 application with an OAuth 2.1 Client](/sections/oauth/client.md)
> - [Understood and implemented the Authorization Code flow](sections/oauth/authz-token)

The Publisher API specifications can usually be found [in our API Catalog](sections/consuming/browsing-apis?id=_1-discover-apis-through-the-api-catalog). If the specifications are not available, contact the API Publisher directly.

The Business API request through APEX requires these additional headers below.

| Headers       | Definition                                                |
| ------------- | --------------------------------------------------------- |
| Authorization | Authorization Token obtained from Authorization Code flow |
| x-apex-apikey | API Key of your application obtained from Step 3 above    |

!> Starting from 1 Dec 2023, please ensure that you update your HTTP Request Header to use **x-apex-apikey** instead of **ApiId** name. Please note that from 3 Feb 2024, **ApiId** will no longer be supported as a valid request header.


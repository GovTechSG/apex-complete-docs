# API Versioning

1. [Create new API](sections/publishing/create-api.md).

![1-create-backend-api](./_assets/api-versioning/1-create-backend-api.png)

2. Import **updated API** and [publish](sections/publishing/publish-api.md) it.

![2-import-to-frontend-api](./_assets/api-versioning/2-import-to-frontend-api.jpg)

3. **Specify the version** in the path (e.g. /api/v2). This is optional but it is the recommended way of creating same API with different versions.

![3-indicate-version](./_assets/api-versioning/3-indicate-version.jpg)

4. Select the older API > click **Manage selected** > choose **Upgrade access to newer API**.

![4-upgrade-existing-api](./_assets/api-versioning/4-upgrade-existing-api.jpg)

5. In the **upgrade dialog**, complete the following:

- **Upgrade to**: Choose the new API.
- **Deprecate**: Enable it to indicate if API is deprecate.
- **Retired**: Enable it to retired API and select date to retired it.
  ![5-upgrade-dialog](./_assets/api-versioning/5-upgrade-dialog.jpg)

6. Click **Upgrade**. If today's date is selected, the API will be retired immediately.

![6-retired-api-status](./_assets/api-versioning/6-retired-api-status.jpg)

> Note:
> 
> - Consumer will be able to see deprecated API(s).
> - Only Publisher will be able to view their own retired API(s).

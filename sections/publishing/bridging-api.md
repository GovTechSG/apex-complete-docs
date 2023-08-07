# Bridging API

## Backend hosted in Intranet and exposing in Internet

1. Intranet API Manager (Int): [Create](sections/publishing/create-api.md) and [Publish](sections/publishing/publish-api.md) API.

```
Frontend URL (Int): https://gw.int.api.gov.sg/{agency_id}/{product_name}/v{version}/{endpoint}
```

2. Internet API Manager (Ext): [Create](sections/publishing/create-api.md) and [Publish](sections/publishing/publish-api.md) API.

3. Ext: Select **Frontend** > **Select API** > **Outbound**.

4. Ext: Enter **Frontend URL (Int)** as **Backend service URL**.

```
Backend service URL: https://gw.int.api.gov.sg/{agency_id}/{product_name}/v{version}/{endpoint}
```

<!-- TODO: Add image -->

## Backend hosted in Internet and exposing in Intranet

1. Internet API Manager (Ext): [Create](sections/publishing/create-api.md) and [Publish](sections/publishing/publish-api.md) API.

```
Frontend URL (Ext): https://public.api.gov.sg/{agency_id}/{product_name}/v{version}/{endpoint}
```

2. Intranet API Manager (Int): [Create](sections/publishing/create-api.md) and [Publish](sections/publishing/publish-api.md) API.

3. Int: Select **Frontend** > **Select API** > **Outbound**.

4. Int: Enter **Frontend URL (Ext)** as **Backend service URL**.

```
Backend service URL: https://public.api.gov.sg/{agency_id}/{product_name}/v{version}/{endpoint}
```

<!-- TODO: Add image -->

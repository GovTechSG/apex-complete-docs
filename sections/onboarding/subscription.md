# Subscribe and add users to APEX Cloud 

To subscribe to APEX Cloud, perform the following steps in the [TechBiz](https://docs.developer.tech.gov.sg/docs/techbiz-documentation/) portal as a Subscription Admin (SA). 

?> The TechBiz portal is only accessible using a GSIB device.

## Setting up your subscription

### Step 1: Create a subscription account

Select **APEX (Production)** or **APEX (Staging)** in the product selection step. Refer to the [TechBiz guide on creating a TechBiz subscription account](https://docs.developer.tech.gov.sg/docs/techbiz-documentation/create-subscription-acc/request-for-techbiz-account).

If you already have a subscription account, refer to the steps in [Manage subscriptions](https://docs.developer.tech.gov.sg/docs/techbiz-documentation/manage-subscriptions?id=subscribe-to-other-sgts-products) to subscribe to APEX products.
    
### Step 2: Create a system

A TechBiz system allows you to share the usage of your APEX  subscription.  Each system is linked to a TechBiz subscription account, allowing for multiple systems per account. Refer to the [TechBiz guide on creating a system](https://docs.developer.tech.gov.sg/docs/techbiz-documentation/create-techbiz-system). 

In this step, provide the following:

| Field | Description |
| -- | -- |
| **System details** | Enter a system name and description.
| **DGP ID details** | Indicate if your system has a DGP ID.
| **Technical admin details** | Add a Technical admin, who can edit and manage systems. You can add up to three Technical admins.

### Step 3: Create a resource (APEX organization)

A resource represents the APEX Organization and environment. Refer to the [TechBiz guide on creating a resource](https://docs.developer.tech.gov.sg/docs/techbiz-documentation/create-configure-resources). 

In this step, provide the following:

| Field | Description |
| -- | -- |
| **Product** | Enter the product name, e.g. APEX (Production) or APEX (Staging).
| **Resource type** | Enter the resource type, e.g. Resource-APEXCLOUD.
| **Resource name** | Enter the Organization name to be used in APEX. The standard Organization naming is `<AGENCY>-<AGENCY/PROJECT>`.
| **Resource description** | Enter a resource description. 
| **System** | Enter the system name linked to the resource.

<!-- 
- **Product**: Enter the product name, e.g. APEX (Production).
- **Resource type**: Enter the environment, e.g. Resource-APEXCLOUD.
- **Resource name**: Enter the Organization name to be used in APEX. The standard Organization naming is `<AGENCY>-<AGENCY/PROJECT>`.
- **System**: Enter the system name created in the previous step. 
-->

?> Ensure that you scroll down if there are resource configuration settings.

## Managing users

While Subscription admins manage both account and system user access, Technical admins can only manage user access for the systems they administer.

Refer to the [TechBiz guide on managing user access](https://docs.developer.tech.gov.sg/docs/techbiz-documentation/manage-user-access-subscribed-sgts-products). 

## Next steps

- [Pre-onboarding](/sections/onboarding/introduction.md)
- [Onboarding as a Publisher](/sections/onboarding/publisher-onboarding.md)
- [Onboarding as a Developer](/sections/onboarding/developer-onboarding.md)
- [Post-onboarding](/sections/onboarding/post-onboarding.md)



# Debugging Common Issues in the API Manager Portal

In this section, you will find a list of common issues that might be encountered in the API Manager Portal and the corresponding steps to resolve each issue. 

?> **Note**: You can use the right sidebar to navigate through the list of common issues.

## Unable to find any APIs or Apps

**Issue:**  I am unable to find APIs or Apps.

**Solution:** Ensure that you are managing the APIs or Apps at the correct environment/zone.

- Staging (external): [https://go.gov.sg/apex-stg](https://go.gov.sg/apex-stg)
- Staging (internal): [https://go.gov.sg/apex-int-stg](https://go.gov.sg/apex-int-stg)
- Production (external): [https://go.gov.sg/apex](https://go.gov.sg/apex)
- Production (internal): [https://go.gov.sg/apex-int](https://go.gov.sg/apex-int)

Once logged in to the desired APEX Cloud API Manager Portal, please switch to the correct **Organization** to view/manage the APIs or Apps.

![Image](./_assets/sections-home-chng-org.png)

## Requesting for API access

**Issue:** As a consumer, how do I request for access to the API?

**Solution:** If you are an existing APEX Cloud consumer, you can email the API publisher and request for access to be given to the organisation which needs to access the API.

For new APEX Cloud consumers, please visit the [APEX Cloud Onboarding Guide](https://sections.developer.tech.gov.sg/sections/apex-cloud-onboarding/) for more information.

## Unable to edit the API

**Issue:** An API in a published state cannot be modified. If publishers were to unpublish the API to make any changes to it, the API will lose the API to App relationship. This relationship can only be established again with the help of the consumer.  Without the API to App linkage being re-established,  API calls, which require inbound authentication on APEX Cloud, will fail.

**Solution:** We recommend publishers to create a new API with the needed updates, and then upgrade the original API to the new API version. Publishers can refer to the [Update API](https://sections.developer.tech.gov.sg/sections/apex-cloud-user-guide/sections/publisher/update-api) workflow to make any changes to their API.

## Missing customised headers

**Issue:** Customised headers are missing after calling APEX Cloud.

**Solution:** API calls to APEX Cloud must conform to the regular expression `[-A-Za-z0-9]+` for any HTTP Header names. HTTP Headers with names that do not conform to the expression `[-A-Za-z0-9]+` will be considered as invalid HTTP headers and will be dropped, in accordance to IM8 Cloud Security Section Para 1.7 S6.

![Image](./_assets/im8-header.png)

You may refer to [the MOF Cloud Security content](https://intranet.mof.gov.sg/portal/IM/Themes/IT-Management/Cloud/Topics/Cloud-Security.aspx) for more information. This is an intranet link.

## Errors due to "Expect: 100-continue" header 

**Issue:** Errors are being encountered on the server due to the `Expect: 100-continue` header sent from the APEX Cloud API Gateway.

**Context:** The `Expect: 100-continue` header is enabled to send the request header and the request body separately. Possible uses cases for enabling the header include outbound authentication or providing bandwidth for a large request body.

Initially, the client calls the backend with only the request headers and the `Expect: 100-continue` header.  The server replies with `HTTP status 100` if it is ready to receive the request, and then the request body is sent. For more information on the the use of the `Expect: 100-continue` header, refer to [MDN documentation](https://developer.mozilla.org/en-US/sections/Web/HTTP/Headers/Expect).

Disabling the `Expect: 100-continue` header will cause the API Gateway to send the complete request, including both the request headers and body, without the need for additional requests. 

**Solution:**

There are two ways to resolve the errors:

1. Allow or whitelist the `Expect: 100-continue` header in your Web Application Firewall (WAF) or Content Delivery Network (CDN).

2. If the first option is not possible, [send a support ticket to the APEX Cloud team](https://form.gov.sg/63e0b55427939600132e0d5f) to disable the header for you.
# General specifications

Refer to the following table for technical APEX Cloud specifications.

| Feature | Defaults  | Can be increased/decreased? (Quotas) |
| -------------------------------- | ---------------------------- | ----------------------------------- |
| Authentication  | **Inbound:**<br>JWT Authentication<br>**Outbound:** <br>API Key, TLS, MTLS,<br>AWS Signature v4,<br>custom policies | Not Applicable |
| JWT Authentication<br>- JWT Issued At Time Drift | 5 seconds | No
| **JWT Authentication** **-**<br>**JWT Expiry Time** | 180 seconds | No
| **Authorization** | Token-based AuthZ with OAuth,<br>OAuth with Singpass/Corppass  | Not Applicable |
| **Protocols Supported** | SOAP, REST, JSON, XML | Not Applicable  |
| **Payload Size Limit (Send)**    | 10 MB  | No   |
| **Payload Size Limit (Receive)** | 10 MB  | No   |
| **Concurrent connections**       | 128    | Yes  |
| **Connection timeout**           | 30 seconds    | Yes   |
| **Active timeout**               | 30 seconds    | Yes   |
| **Max Transaction Per Second (API)** | 20 | Yes |
| **Max Transaction Per Second (Application)** | 20 | Yes |
| **API Deprecation Header** | x-api-deprecated | Not Applicable  |
| **API Retirement Date Header** | x-api-retire-time (in UTC) | Not Applicable  |
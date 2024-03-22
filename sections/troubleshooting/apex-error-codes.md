# APEX Error Codes

| Error Code | Error Name               | Error Description                                                   |     |     | Comments |
| ---------- | ------------------------ | ------------------------------------------------------------------- | --- | --- | -------- |
| 401        | Unauthorized             | Authentication error                                                |     |     |
| 403        | Forbidden                | Authorization error, possibly incorrect API Key                     |     |     |
| 405        | Method Not Allowed       | Method not specified for API                                        |     |     |
| 415        | Unsupported Media Type   | Content-type not supported                                          |     |     |
| 429        | Too Many Requests        | Application or API request per second exceeded                      |
| 433-452    | JWT Authentication Error | Refer to this [page](/sections/troubleshooting/jwt)                 |     |     |
| 500        | Internal Server Error    | General gateway/server error                                        |     |     |
| 502        | Bad Gateway Error        | Server error, could also be payload too large (more than 10MB)      |     |     |
| 504        | Gateway Timeout          | Backend server timed out or failed to respond within a certain time |     |     |
| 526        | Invalid SSL Certifate    | Backend SSL certificate error or SSL session incomplete             |     |     |

# Whitelist API Hosts

## Background

By default, APEX Cloud gateway does not allow traffic to an unknown API host.
To allow calls be able to reach your API, the API host must be whitelisted beforehand.

## To whitelist a host

1. Click **Services > Egress Whitelisting** in unified portal.
2. Click **Request New** button on top right of the page.
3. In the pop-up form, select an organization and enter url of the API host. 
   The format of the url should look like below: 
   ```
   http(s)://my.example.host(:8888)/any-optional-path
   ```
4. Click **Submit**.

Note:

- Port will be default to **80 for http** and **443 for https** if not specified in the url. If you are using a different port for your API host, you need to specify in the url.
- Whitelist entry is created based on the url's protocol, domain and port combination. 
  
  *__Example 1__: http://example.com and https://example.com should be whitelisted separately as they have different protocols.*
  
  *__Example 2__: http://example.com:5555 and http://example.com:6666 should be whitelisted separately too as they have different ports.*
  
  *__Example 3__: http://example.com/my-api1 and http://example.com/my-api2 are considered the same entry (thus the one added later will fail), as their protocol, domain and port combination are the same (http+example.com+80)*
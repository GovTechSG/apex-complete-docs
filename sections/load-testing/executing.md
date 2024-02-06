# Executing the End-to-End Load-Test

Load-testing on APEX can be carried out using any open-sourced load-testing tools, such as JMeter, K6 or Artillery.

The tool could be incorporated with a dynamic JWT header library. The load test will typically start with a short and simple test, followed by scaling up to the intended load, and a sustained load for a number of minutes, such as 15-30 minutes.

The scaling up could help cloud-based Application Load Balancers and/or backend instances (such as containers) to scale up their capacity instances. The steepness of scaling could depend on actual traffic pattern, meaning a spiky traffic could have very short scaling timing.

Be aware of the technologies employed for load-testing in your environment and pros/cons surrounding their uses:

| Technology Type     | Technology Used                 | Pros                                     | Cons                                                                                |
| ------------------- | ------------------------------- | ---------------------------------------- | ----------------------------------------------------------------------------------- |
| Client Machine      | Laptop                          | Usually quick to setup test application  | Low on resources, hence results may not be accurate                                 |
|                     | Server / Cloud Instances        | Higher resources available               | Slower to install load-testing applications, especially on restrictive environments |
| Transmission Media  | Wireless Network                | -                                        | Prone to latency and jitter, hence results may not be accurate                      |
|                     | Wired Network/ Cloud Networks   | Low Latency                              | -                                                                                   |
| Testing Application | Postman                         | Could be quick to setup test application | Limit imposed on test volume unless premium version of postman is used              |
|                     | Open-sourced load-testing Tools | Usually little limits on test throughput | -                                                                                   |

Therefore, the best setup would be carrying out the load-test using a **high performance open-sourced testing tool with servers or cloud instances**.

<br></br>

## Sample codes of files supporting load-testing using Artillery

Sample codes of how this could be achieved with Artillery is as below. Do note that the following code is for reference only and is not intended for production use.

```
###
### package.json - please use the latest library versions or equivalent libraries
###
{
  "name": "artillery",
  "version": "1.0.0",
  "description": "",
  "main": "jwt.js",
  "scripts": {
    "test": "exit 0"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "crypto": "^1.0.1",
    "fs": "^0.0.1-security",
    "jose": "^4.13.1",
    "jsonwebtoken": "^9.0.0",
    "uuid": "^9.0.0"
  }
}

###
### test_script.yaml
###
config:
  target: 'https://testing-url'
  http:
    timeout: 60
  phases:
    - duration: 10
      arrivalRate: 1
      name: Warm up
    - duration: 20     ## Ramp up phase in seconds, adjust as needed
      arrivalRate: 5
      rampTo: 20
      name: Ramp up load
    - duration: 10     ## Sustained load phase in seconds, adjust as needed
      arrivalRate: 20
      name: Sustained load
  processor: './jwt.js'
  payload:
scenarios:
  - name: 'Load Test'
    flow:
      - post:  ## Change this to get for GET method
          url: '/path'
          beforeRequest:
            - 'addBodyUrl'
            - 'addHeaders'
          afterResponse:
            - 'logResponse'


###
### jwt.js - please insert in your own URL, API Key and method
###
// DEPENDENCIES
const crypto = require('crypto');
const fs = require('fs');
const jose = require('jose');
const jwt = require('jsonwebtoken');
const {v4: uuidv4} = require('uuid');

// USER INPUT
const issuer = '<< API Key(s) >>';
const keyId = '<< Key ID >>';
const audience = 'https://<< APEX Endpoint >>;
const contentType = 'application/json';

const subject = 'POST';  // Change this to GET for GET method
const caPrivateKey = fs.readFileSync('./private.key', 'utf8');
const contents = fs.readFileSync('./payload.txt', 'utf8').trim();
const hash = sha256Hash(contents);

/*
 ***** FUNCTION TO RETURN SHA-256 ENCODING *****
 */
function sha256Hash(string) {
  return crypto.createHash('sha256').update(string).digest('hex');
}

/*
 ***** FUNCTION TO RETURN PRIVATE KEY IN PKCS8 FORMAT *****
 */
const importKey = async key => {
  const importedKey = await jose.importJWK(key, 'ES256');
  const privateKeyPKCS8 = await jose.exportPKCS8(importedKey);
  console.log(privateKeyPKCS8.trim());
  return privateKeyPKCS8;
};

/*
 ***** FUNCTION TO GENERATE JWT ******
 */
const getJWT = (issuer, subject, keyid, audience, hash, privateKey) => {
  const signOptions = {
    algorithm: 'ES256',
    keyid,
    expiresIn: '180s',
    jwtid: uuidv4(),
    issuer,
    audience,
    subject,
  };
  const payload = {
    data: hash,
  };

  var jwtAuth = jwt.sign(payload, caPrivateKey, signOptions);
  var jwtDecoded = jwt.decode(jwtAuth, {complete: true});
  return jwtAuth;
};

// Artillery required exported functions
module.exports = {
  logResponse,
  addBodyUrl,
  addHeaders,
};

function addBodyUrl(requestParams, context, ee, next) {
    requestParams.body = contents;  // Remove this for GET method
    requestParams.url = audience;
    return next();
}

function addHeaders(requestParams, context, ee, next) {
  requestParams.headers = {'x-apex-jwt': getJWT(issuer, subject, keyId, audience, hash, caPrivateKey),'Content-Type': contentType};
  return next();
}

function logResponse(requestParams, response, context, ee, next) {
  if (response.statusCode !== 200){
    // This shows a verbose output whenever the response status code is not 200
    console.log(response);
  }
  return next();
}


###
### payload.txt - This can be changed to your own JSON payload
###
{"data":"payload"}


###
### private.key
###
<< Append private key in PKCS8 format >>

```

## Commands to run load-test using Artillery

```
% npm install -g artillery@2.0.0-32
% npm i
% artillery run -k test_script.yaml
```

## Sample output when running load-test using Artillery

A short test run of phases Warm Up, Ramp Up Load and Sustained Load of durations 10, 20 and 10 seconds could produced a sample output below.

```
Test run id: xxxx
Phase started: Warm up (index: 0, duration: 10s) 16:47:15(+0800)

Phase completed: Warm up (index: 0, duration: 10s) 16:47:25(+0800)

Phase started: Ramp up load (index: 1, duration: 20s) 16:47:25(+0800)

--------------------------------------
Metrics for period to: 16:47:20(+0800) (width: 3.148s)
--------------------------------------


<<truncated>>


Phase completed: Ramp up load (index: 1, duration: 20s) 16:47:45(+0800)

Phase started: Sustained load (index: 2, duration: 10s) 16:47:45(+0800)


<<truncated>>


--------------------------------------
Metrics for period to: 16:48:00(+0800) (width: 6s)
--------------------------------------

http.codes.200: ................................................................ 121
http.downloaded_bytes: ......................................................... 1452
http.request_rate: ............................................................. 20/sec<<- This is the current TPS
http.requests: ................................................................. 118
http.response_time:
  min: ......................................................................... 83
  max: ......................................................................... 172
  median: ...................................................................... 90.9<<- Important statistic
  p95: ......................................................................... 104.6
  p99: ......................................................................... 130.3<<- Important statistic
http.responses: ................................................................ 121
vusers.completed: .............................................................. 121
vusers.created: ................................................................ 118
vusers.created_by_name.Regression Test: ........................................ 118
vusers.failed: ................................................................. 0
vusers.session_length:
  min: ......................................................................... 123.6
  max: ......................................................................... 237
  median: ...................................................................... 141.2
  p95: ......................................................................... 169
  p99: ......................................................................... 219.2


All VUs finished. Total time: 41 seconds

--------------------------------
Summary report @ 16:47:58(+0800)
--------------------------------

http.codes.200: ................................................................ 460
http.downloaded_bytes: ......................................................... 5520
http.request_rate: ............................................................. 15/sec
http.requests: ................................................................. 460
http.response_time:
  min: ......................................................................... 81
  max: ......................................................................... 190
  median: ...................................................................... 92.8
  p95: ......................................................................... 113.3
  p99: ......................................................................... 147
http.responses: ................................................................ 460
vusers.completed: .............................................................. 460
vusers.created: ................................................................ 460
vusers.created_by_name.Regression Test: ........................................ 460
vusers.failed: ................................................................. 0
vusers.session_length:
  min: ......................................................................... 123.6
  max: ......................................................................... 1218.9
  median: ...................................................................... 144
  p95: ......................................................................... 179.5
  p99: ......................................................................... 242.3

```

## Latency Statistics

Statistics can be seen in the last batch of testing at 20 TPS for the response time of request (in ms).

```
http.response_time:
min: ......................................................................... 83
max: ......................................................................... 172
median: ...................................................................... 90.9
p95: ......................................................................... 104.6
p99: ......................................................................... 130.3
```

From here we can see the important statistics for the response time:

- **Median response time: 90.9ms**
- **P99 response time: 130.3ms**

Hence effort could be used to troubleshoot and normalize this gap.

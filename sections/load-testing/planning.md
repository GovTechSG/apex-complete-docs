# Planning Your End-to-End Load-Test

## Background

End-to-end load-testing is important to establish an acceptable performance baseline for a new API or an existing API (when there are major version/backend changes).

The planning stage is divided as such:

- Planning When To Perform Load-Testing
- Inform APEX of Intention To Carry Out Load-Testing
- Determining APIs and Scope
- Determining Testing Transactions per Second
- Testing and Post-Testing Actions

## Planning When To Perform Load-Testing

It is recommended that your new and existing API be load-tested, especially if it is a new version release, undergone major changes such as infrastructure changes, refactoring, or when there is a major event approaching which may cause a spike in usage (eg. tax submission, elections).

## Inform APEX of Intention To Carry Out Load-Testing

It is mandatory to inform APEX of the intention to carry out any load test, in order to co-plan the scheduling, duration and scope of testing.

This will allow APEX to carry out resource planning and determine if there were any resource conflicts.

It is strongly recommended to test using the APEX staging environment, unless, permitted to do otherwise.

The scope of the test could include the APIs tested, the methods invoked, the maximum load to be tested and the steepness of the scale-up to maximum load.

## Determining APIs and Scope

It will be good to plan based on the most "heavy-hitting" APIs as well as APIs which cause the most computation on the backend to monitor the performance. Hence, appropriate request parameters and payload should be crafted to test the worst case scenario of heavy computation during.

Ideally the APIs load-tested should not exceed 3-5 seconds.

The availability (based on planned and unplanned downtime) and latency of the API must be published in the documentation or API description as per IM standards.
The latency determined during load-testing can be used as a reference to this.

## Determining Testing Transactions Per Second

The Real-time dashboard helps you monitor the status of your API requests in real time during the load-test. If there is a precendence as to the production usage level, the Real-time dashboard can be used to zoom to time of heavy usage and determine the peak transaction per second (tps).

The Load-test could be planned with the peak tps with a buffer of 20% or an appropriate percentage.

If there was no precedence and this is an API with limited use, 20tps could be considered. If it was an API used by several agencies often or by the general population, a higher TPS could be planned with APEX.

## Actual Testing & Post-Testing Actions

### Actual Testing

The scope of testing, Testing TPS, duration and other details should be coordinated with APEX.

The Testing would be completed with the median, P95, P99, mean latency times and status codes recorded and availability/latency benchmarked.

Some sample codes for the load-testing could be found [here](/sections/load-testing/executing).

### Post-Testing Actions

If there are unintended status codes, do troubleshoot the cause of issue, if it was caused by infrastructure, or application or database. A tracing tool or APM would greatly help in this aspect.

Do take note of the following status codes relating to errors commonly encountered in a load-test:

| Status Code | Cause of Error                 | Other Symptoms Observed     | Remediation Action Needed                                                          |
| ----------- | ------------------------------ | --------------------------- | ---------------------------------------------------------------------------------- |
| **429**     | Rate Limit in APEX Exceeded    | x-rate-limit header present | Reduce TPS or increase rate limit throttle                                         |
| **500**     | Backend or Gateway unavailable |                             | Check on load balancer, API gateway, backend if there are errors                   |
| **504**     | Backend Timeout                |                             | Check if backend resources are busy (eg. check CPU utilization of server/database) |

Do work on remediating the issue(s) and re-test until the API is producing 100% availability of an acceptable latency within the testing TPS.

The last best working TPS could be considered to be configured in APEX for the API configurations for rate limiting, and the **availability** and **P99 latency** documented in the API description, as per IM requirements. Availibility in this case would be the uptime SLA of the API taking into account the maintenance downtime.

?> **Note:** The Rate-Limiting configurations in APEX could be currently configured using a Service Request.

# K6 lab results
## Part 0
- Step 0.1, http_req_duration: 296.29 ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5:  75 requests, 3.599539 requests/s, p(95) = 363.31 ms.
  The request rate stays below 5 requests per second because each virtual user
  needs to wait for the response and sleep for 1 second before starting the next iteration.
- Step 0.6, exit code of the normal run and of the run with P95=300: Normal run: exit code 0.
P95=300 run: exit code 99 because the p(95) threshold failed (486.82 ms > 300 ms).
- Step 0.7, rules that passed, p(95), p(99), exit code after your change: All three rules passed. p(95) = 545.94 ms, p(99) = 702.73 ms, and the failure rate was 0.00%.
After changing the p(95) requirement from <1000 ms to <300 ms, the p(95) rule failed with p(95) = 430.26 ms. The p(99) and failure-rate rules still passed. The exit code was 99.
- Step 0.8, what was wrong, and how the checks showed it: The initial request used lowercase values, pepperoni and knife, which did not match the ingredient and tool names used by the API. The checks showed that no pepperoni failed 12 out of 74 times and no knife needed failed 24 out of 74 times. After changing them to Pepperoni and Knife, all checks passed (222/222, 100%), and the HTTP failure rate was 0%.
## Part A results
| Step | Test type | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1 | Smoke | 1 | 0.574215 | 558.72 ms | 567.04 ms | 0.00% | 100.00% |
| A2 | Average load | 3 | 1.289173 | 450.63 ms | 10.33 s | 0.42% | 99.67% |
| A3 | Stress | 5 | 2.161142 | 425.34 ms | 1.39 s | 0.00% | 100.00% |
| A4 | Spike | 5 | 1.024982 | 659.77 ms | 1.5 s | 0.00% | 100.00% |
| A5 | Soak	| 2 | 0.938704 | 569.11 ms | 10.33 s | 0.16% | 99.75% |

## Part B
1. The results are: A1 = 0.574215 req/s, p95 = 558.72 ms, A2 = 1.289173 req/s, p95 = 450.63 ms, A3 = 2.161142 req/s, p95 = 425.34 ms, A4 = 1.024982 req/s, p95 = 659.77 ms, and A5 = 0.938704 req/s, p95 = 569.11 ms.

2. The request rate is below 5 requests/s because each virtual user waits for the response and sleeps for 1 second before starting the next iteration.

3. No. The p95 response time decreased from 450.63 ms in the average-load test to 425.34 ms in the stress test. Therefore, the response time did not grow in these tests.

4. The response time increased during the spike and then gradually returned toward the previous level. The exact recovery time should be checked from the A4 report graph because the summary results do not show the exact recovery time.

5. The A5 test had a low HTTP failure rate of 0.16% and 99.75% checks passed. However, the summary results cannot confirm whether the response time or error rate worsened over time. A several-hour soak test could reveal problems such as memory leaks.

6. P95 is used because the average can hide slow requests. P95 shows the response time experienced by the slower 5% of requests.

7. The results can be different because internet network conditions may change, and the public site may have other users or traffic at the same time.

8. We should not run hundreds of users against a site we do not own because it can affect the site's performance and real users. It may also be considered unauthorized or abusive traffic.

9. A testable requirement is: “At least 95% of requests should complete within 1 second, and the HTTP failure rate should be below 1%.” The K6 thresholds are:
```javascript
thresholds: {
  http_req_duration: ['p(95)<1000'],
  http_req_failed: ['rate<0.01'],
}
```
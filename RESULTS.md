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
| A1 | Smoke | 1 | | | | | |
| A2 | Average load | 3 | | | | | |
| A3 | Stress | 5 | | | | | |
| A4 | Spike | 5 | | | | | |
| A5 | Soak | 2 | | | | | |
## Part B
1.
2.
3.
4.
5.
6.
7.
8.
9.
# API Service

| Category     | SLI | SLO                                                                                                         |
|--------------|-----|-------------------------------------------------------------------------------------------------------------|
| Availability | This is the total number of successful requests / the total number of requests | 99% |
| Latency      | This is a histogram of requests grouped showing the 90th percentile | 90% of requests below 100ms |
| Error Budget | This is the percentage of the remaining error budget: 1 - (1-(successful requests/total number of requests) / 20%)  | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   | This is the total number of successful requests over 5 minutes | 5 RPS indicates the application is functioning |

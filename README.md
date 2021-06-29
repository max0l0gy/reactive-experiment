# Case for experiment with reactive REST Services

Based on a real-life story and [Reactive Spring book by bootiful Josh Long](https://leanpub.com/reactive-spring)      

**Write and test REST services**

- report-collector Service  (Aggregate pattern)   
Service for the periodic generation of reports for customers.

- customer-report-calculation Service (Aggregate pattern)    
The service performs an expensive data aggregation operation and can respond up to 30 sec. 

- customer-report-storage service (Database adapter pattern)   

Imaging we have from 100,000 to 500,000 customers   
The "Report Collector" service is launched by the cron and bypasses all active customers, send the request to the service customer-report-calculation via the REST API (or GRPS), receives a report - puts the report into the customer-report-storage service, and then sends notification of the readiness of the report to the customer, if the customer subscribed for notifications about monthly reports.

All services are implemented in Java / Kotlin using a reactive backend development approach (Reactive Spring, Quarkus Reactive)   
The "report collector" should work smoothly and quickly without overloading the system services (customer-report-calculation, customer-report-storage, ..., customers, order... etc) 

The customer-report must remain healthy and report requests up to 1000 requests per second to consumers while the report build process is running.

It is necessary to carry out load testing of each service and provide RPS (requests per second) for each service in the limited resources (then less the better principal) of the cloud platform (for example kubernetes cluster @ Digital Ocean)   
256-512 (1/4, 1/2) CPU   
256-512 MB RAM   
It is necessary to determine the safety margin of RPS with such resources for each service.   
Based on the conditions:   
100,000 to 500,000 clients   
The report is calculated up to 30 sec   

And then write the same service with Quarkus and compare with the same service writed with Reactive Spring   

PostgreSQL operator 4 k8s   
https://github.com/zalando/postgres-operator

Analytics   
https://habr.com/ru/company/flant/blog/520616/

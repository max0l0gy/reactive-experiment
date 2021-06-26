# Case for experiment with reactive REST Services

**Write and test services**

- report-collector Service  (Aggregate)   
Service for the periodic generation of reports for customers.

- customer-report-calculation Service (Aggregate)    
The service performs an expensive data aggregation operation and can respond up to 30 sec. 

- customer-report-storage service (Database adapter)   

Let's say we have from 100,000 to 500,000 customers   
The "Report Collector" service is launched by the cron and bypasses all active customers, send request to the service customer-report-calculation via the REST API (or GRPS), receives a report - puts the report into the customer-report-storage service, and then sends notification of the readiness of the report to the customer, if the customer subscribed for notifications about monthly reports.

All services are implemented in Java / Kotlin using a reactive backend development approach (Reactive Spring, Quarkus Reactive)   
The "report collector" should work smoothly and quickly without overloading the system services (customer-report-calculation, customer-report-storage, ...) 

The customer-report must remain healthy and report requests up to 1000 requests per second to consumers while the report build process is running.

It is necessary to carry out load testing of each service and provide RPS (requests per second) for each service in the limited resources of the cloud platform (for example, Digital Ocean)   
250-500 (1/4, 1/2) CPU   
250-512 MB RAM   
It is necessary to determine the safety margin of RPS with such resources for each service.   
Based on the conditions:   
100,000 to 500,000 clients   
The report is calculated up to 30 sec    


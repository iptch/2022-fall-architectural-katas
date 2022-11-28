# Reporting Capability
The reporting capability covers the service landscape enabling **Hey, Blue!** staff to generate reports and share them with 
media companies.

![Order Capability](resources/hey-blue-report.drawio.svg)
<img width="750" src="resources/hey-blue-legend.drawio.svg">


## Services

### Reporting
- Provides API to generate various reports
- Distributes reports to Media Companies
- Only service with read-only-access to its database. See [ADR06 Read Replica Pattern](../ADRs/2022-11-06_06-read-replica-pattern.md)

## Infrastructure

### Periodical Batch Job / Change Data Capture
- Persists the data from other microservices into the read replica database.
- This is achieved by either a periodical batch job that runs when the load on the system is low or continuously with
CDC (Change Data Capture). The recommendation is to start with a periodical batch job with the option to change to CDC
if the reports require real-time data, since CDC is more complex and costly to set up.

### Read Replica
- Contains all relevant data for reports from other microservices.

## Related ADRs
- [ADR01 Microservice Architecture](../ADRs/2022-10-31_01-microservice-architecture.md)
- [ADR05 Backend-for-Frontend Pattern](../ADRs/2022-11-01_05-bff.md)
- [ADR06 Read Replica Pattern](../ADRs/2022-11-06_06-read-replica-pattern.md)


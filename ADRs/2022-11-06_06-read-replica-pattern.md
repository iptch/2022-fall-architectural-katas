# 2022-11-06 ADR06 Read Replica Pattern

## Title
Use of Read Replica Pattern

## Status
Proposed

## Context
In the [Functional requirements](../requirements/functional-requirements.md) we describe the need for comprehensive analytics and reporting.
These tasks can be very computationally expensive and are usually not run continuously. The reporting and analytics functionality should 
not put a strain on the core system.

## Decision
In order to fulfill the above-mentioned requirements, we decide to follow the read replica pattern. All microservice data is 
duplicated into its own database. Reports and Analytics can then use this database for their respective work items, thus avoiding 
straining the core system of the application. This also allows analytics teams to run computationally heavy loads on the data 
without having to worry about core system stability.

## Consequences
With this pattern, we find ourselves in the realm of *eventual consistency*. We find this an acceptable compromise, as reports 
usually aggregate data over longer time periods and missing a few hours will have a negligible effect on the report itself.
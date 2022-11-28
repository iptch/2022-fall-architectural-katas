# 2022-10-31 ADR02 Event-Driven Design

## Title
Making certain parts of our architecture event-driven

## Status
Proposed

## Context
Microservices can communicate synchronously (blocking) and asynchronously (non-blocking). The **Hey, Blue!** system has to implement several processes that contain consecutive and parallel steps.

## Decision
We use asynchronous calls between services whenever the calling service can proceed without waiting for the result of the called service. This is usually the case when the processing of the occurring event can be delayed without interrupting the process or hindering the user experience.

## Consequences
The solution architecture has to provide a way to queue messages. While this can be in the responsibility of event-driven services (e.g. the Notification service) we strongly recommend to integrate a dedicated event-streaming platform like [Kafka](https://kafka.apache.org/) or [Azure Event Hub](https://azure.microsoft.com/de-de/products/event-hubs/#features). This ensures a decoupling of services, which generally increases maintainability and expandability of the system. It also provides a solid foundation in case future enhancements plan to leverage event-driven communication.
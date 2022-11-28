# 2022-10-31 ADR04 Dispatcher Architecture

## Title
Design of the service responsible for dispatching orders to participating businesses

## Status
Proposed

## Context
In the [Non-functional requirements](../requirements/non-functional-requirements.md) we described why *ease of integration* is crucial to the success of **Hey, Blue!**. To fulfill this requirement we decided to let **Hey, Blue!** handle all the order stuff. Businesses are just required to post their offerings via a webform and to ship received orders to the customer. For sending the order to the Business we have a dedicated Dispatcher service. This service must be designed to easily integrate many different kinds of businesses.

## Decision
The Dispatcher service will follow the pattern of a Microkernel architecture. The core system is responsible for taking orders, updating their state and posting notifications about shipment state. It is connected asynchronously to the Order and Notification services. The Dispatcher service is expandable by Plugins where each Plugin represents one participating Business. The plugin is responsible to place **Hey, Blue!** orders at the respective Business.

## Consequences
This architecture makes onboarding of new Businesses easy and allows the system to integrate all thinkable kinds of interfaces. Larger Businesses may expose some kind of API to place Orders but smaller Businesses may take Orders by Mail or even Phone or Fax. For each new Business the **Hey, Blue!** team has to develop a new Plugin with customized code that supports the specialized needs of the Business. This way the participation in **Hey, Blue!** is very inclusive and no Business is excluded.
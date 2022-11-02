# 2022-fall-architectural-katas
Public repository to upload deliverables for O'Reillys 2022 Fall Architectural Katas

## Team IPT
Team IPT consists of [Matthäus Heer](https://ipt.ch/de/team/mitarbeiter/matthaus-heer), [Nicolas Mesot](https://ipt.ch/de/team/mitarbeiter/nicolas-mesot) and [Max Riedel](https://ipt.ch/de/team/mitarbeiter/max-riedel). We all are IT Consultants with [Innovation Process Technology AG](https://ipt.ch) in Zurich, Switzerland.

## Requirements
We grouped the requirements for the **Hey, Blue!** application into the following two sections.  
- [Functional requirements](requirements/functional-requirements.md)
- [Non-functional requirements](requirements/non-functional-requirements.md)  

While the former explains the desired functionalities of the application, describing possible user interactions, the former
represents a set of technical guidelines the system has to adhere to.

## Context
In the following we will describe what actors (user or system) might interact in what way with the **Hey, Blue!** system.
Those interactions are analyzed in the following two dimensions  

1) What is the intent of the interaction? What is the goal of an interaction?
2) What data is being exchanged between the actor and the system?

The actors are being divided into internal, i.e. actors which are internal to the **Hey, Blue!** ecosystem and external actors,
e.g., civilians and officers using the application. That way we receive a clear picture who profits from this ecosystem
and what the intends and desires of those actors might be.
![Context Diagram](context/resources/context-diagram-dummy.jpg)

## Domain Design
### Event-Storming process
How we did that stuff...

### Domain capabilities
The fields we came up with using event storming to form micro-service landscapes...

TODO: Overview of complete architecture.

Now we add the sub-architectures.

#### Connection capability
TODO: Put in the view and link to [Connection Capability](domain/connection-capability.md).

#### Reporting capability
TODO

#### Order capability
TODO

#### User
TODO

## Service Architecture
TODO: Describe clean architecture with Connection Service example.

## System Architecture
TODO: Add Azure Cloud Architecture with ADR why Azure und ADRs for decisions.

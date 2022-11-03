# 2022-fall-architectural-katas
Public repository to upload deliverables for O'Reillys 2022 Fall Architectural Katas

## Team IPT
Team IPT consists of [Matthäus Heer](https://ipt.ch/de/team/mitarbeiter/matthaus-heer), [Nicolas Mesot](https://ipt.ch/de/team/mitarbeiter/nicolas-mesot) and [Max Riedel](https://ipt.ch/de/team/mitarbeiter/max-riedel). We all are IT Consultants with [Innovation Process Technology AG](https://ipt.ch) in Zurich, Switzerland.

## Requirements
The following overview exhibits all actors participating in the system with their main intentions and capabilities.
More specifics can be found in the functional requirements section below.

<p align="center">
<img width="800" src="requirements/resources/hey-blue-actors-overview.drawio.svg">
</p>

We grouped the requirements for the **Hey, Blue!** application into the following two sections.  
- [Functional requirements](requirements/functional-requirements.md)
- [Non-functional requirements](requirements/non-functional-requirements.md)  

While the former explains the desired functionalities of the application, describing possible user interactions, the former
represents a set of technical guidelines the system has to adhere to.

## Context
In the following we will describe what actors (can be a user, participant or system) might interact in what way with the **Hey, Blue!** system.
For that matter, the diagram displays the main capabilities or intentions a user or system has to its disposal. 

The actors are being divided into internal, i.e. actors which are internal to the **Hey, Blue!** ecosystem and external actors,
e.g., civilians and officers using the application. That way we receive a clear picture who profits from this ecosystem
and what the intends and desires of those actors might be.

<p align="center">
<img width="800" src="context/resources/hey-blue-context-digram.drawio.svg">
</p>

## Domain Design
Now that all the actors and intents in the system are well understood, it's time to map these requirements into domain model which defines a common terminology and crystallizes out lower level components of the domain landscape. These components are grouped into so-called capabilities which form the basis for the final software architecture based on microservices. 
We tackle this challenge by employing [Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design). This approach enables us to subdivide the overarching problem statement into meaningful sub-domains based on business requirements and come up with a scalable, modularized and extensible software architecture.

### Event-Storming process
[Event Storming](https://www.eventstorming.com/) is a technique to develop a common understanding of all involved stakeholders, that is, domain experts, managers and the development team, of the domain at hand. Our approach was as follows:
- First we identified the major events taking place in the system due to some actor's interactions (orange stickers).
- We placed the actor (small yellow stickers) along with the intent for the action (blue stickers) next to the event.
- Those Actore-Command-Event groupings would be combined to meaningful Aggregates (large yellow stickers), which are logical entities making up the domain model.
- We identified hot spots (dark pink stickers) or open questions and resolved those during discussions.
- External systems (light pink stickers) have been added.
- We regrouped all the aggregates into logical system capabilities (green stickers). For each capabilitiy (e.g. "Connection", that is all the components involved in a civilian and an officer performing a digital handshake, a connection) a microservice landscape has been developed (see [Domain capabilities](#domain-capabilities)).

The following picture shows the final state of the Event Storming session after re-grouping Aggregates and extracting capabilities. The distilled Aggregates emerge prominently in the final architecture representing the commonly developed terminology and understanding of the domain.
<p align="center">
<img width="800" src="eventstorming/resources/event-storming-final-panel.png">
</p>


### Domain capabilities
Based on the output of the Event Storming, we defined the following capabilities for each of which we developed a microservice architecture.

- [Connection capability](#connection-capability)  
This is the heart piece of the Hey, Blue! ecosystem enabling civilians and officers to connect. This includes the possibility for officers to enroll in the look-up for civilians such that civilians can find officers online, the actual virutal handshake itself along with the respective notifications, rewards and awarding of points. Handshakes can only happen in proximity and connections might be shared over social media.

- [Reporting capability](#reporting-capability)  
TODO: Give description.

- [Order capability](#order-capability)  
TODO: Give description.

- [User capability](#user-capability)  
TODO: Give description.

For the following, if not stated otherwise in the diagram at hand, the symbols reflect the meaning as described in the following legend.
<p align="center">
<img width="800" src="domain/resources/hey-blue-legend.drawio.svg">
</p>

#### Connection capability
The connection capability describes the micro-service landscape enabling civilians and officers to make a 
connection incl. all the processes around this central happening. The capability is described in more detail
here: [Connection Capability](domain/connection-capability.md).

<p align="center">
<img width="800" src="domain/resources/hey-blue-connection.drawio.svg">
</p>

<p align="center">
<img width="600" src="domain/resources/hey-blue-connection-legend.drawio.svg">
</p>

#### Reporting capability
The reporting capability covers the service landscape enabling Hey, Blue! staff to generate reports and share them with 
media companies. More information can be found here:  [Reporting Capability](domain/reporting-capability.md)

<p align="center">
<img width="650" src="domain/resources/hey-blue-report.drawio.svg">
</p>


#### Order capability
The order capability describes the service landscape enabling Civilians or Charities to redeem their points. For more details see [Order Capability](domain/order-capability.md).

<p align="center">
<img width="900" src="domain/resources/hey-blue-order-capability.drawio.svg">
</p>

#### User capability
TODO

## Service Architecture
TODO: Describe clean architecture with Connection Service example.

## System Architecture
TODO: Add Azure Cloud Architecture with ADR why Azure und ADRs for decisions.

<!-- 
HTML IMG TEMPLATE FOR IMAGES

<p align="center">
<img width="800" src="relative/path/to">
</p>

-->
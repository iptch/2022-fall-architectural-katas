# ADR01 Microservice Architecture

## Title
Use the microservice architecture style

## Status
Proposed

## Context
Microservice architectures are commonplace nowadays. By itself, this is not a good enough reason to choose 
a microservice architecture. Below, we list some pros and cons of such a decision and why we still believe
that for **Hey, blue!**, a microservice architecture is the way to go.

## Decision
We will use the microservice architecture style for **Hey, blue!** backend systems.

## Consequences

### Flexibility
Micro architectures come with the great benefit of flexibility. When implemented correctly, teams can work 
independently on their respective services. All that is needed for inter-service communication
are clean service interfaces.

Deploying microservices also can be done independently, increasing flexibility.

For a startup with limited resources, flexibility to allocate these resources where the need is the greatest comes as
a great benefit.

### Scalability
The other big selling point of microservice architectures is of course scalability. Different services scale at 
different speeds and predicting which ones will scale by how much is a hard task. With a microservice architecture,
services can be scaled reactively.

### Referential Integrity
One of the big drawbacks of microservice architectures is the loss of referential integrity. Since each 
microservice owns their own storage, references carried across services can get out of sync. Here is where 
clean interfaces and processes come back into the picture. Good communication between teams can help alleviate 
these problems. In a startup environment, this can be as simple as a Slack channel.

### Duplication Overhead
It is inevitable that some code will get duplicated over time since each team operates independently. Finding the
right balance is where the challenge resides. A good reference architecture can help alleviate lots of these concerns.
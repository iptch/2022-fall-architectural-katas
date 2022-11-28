# 2022-11-08 ADR07 Azure as a Hyperscaler

## Title
Choosing a public cloud hyperscaler for the Hey, Blue! infrastructure

## Status
Proposed

## Context
Our architectural design has a [Cloud Native](https://www.cncf.io/) approach in mind. To support this mindset, we need a solid hyperscaler platform in place to provision infrastructure and services. Since there is a hand full of public cloud providers with a large enough service catalogue, reputation and thriving user base, a decision has to be taken as to which vendor will host **Hey, Blue!**. 

## Decision
We point out that any of the leading cloud providers such as [Google Cloud Platform](https://cloud.google.com/), [Amazon Web Services](https://aws.amazon.com/) or [Azure](https://azure.microsoft.com/) would have the capabilities to fit our needs. Most of the services used in our case can be provisioned with slight modifications on each and every platform. We regard a multi-cloud solution as significant overhead since this is a green-field project and the integration of multiple cloud providers would simply hinder progress. Thus the decision for a specific vendor boils down to preference, pre-existing know-how and interoperability with other tools in the stack.  
Due to our advanced knowledge of Microsoft Azure we propose a solution architecture based on Azure. Furthermore, Azure has great interoperability with Github (both Microsoft owned) through Github Actions which is the primary place to host open-source software. We think making **Hey, Blue!** an open-source project would be a great idea fostering trust and openness.

We would like to point out that all the services shown in the solution architecture are available in slight modification
for other cloud providers making the design, although focusing on Azure-specific components, portable to other
cloud providers. This might be a need if for example the devloper team has a mor profound knowledge on another platform.

## Consequences
There is a lock-in to a certain public cloud provider. Developers have to learn usign the Platform and services if the respective know-how is not available yet. Extending to multi-cloud setup is a possibility but requires additional efforts which, in the current state of the project, do not make sense.
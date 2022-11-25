# System Architecture

This in-depth design document elaborates on our proposal for the system design shown below, 
paying special attention to the decision-making process while discussing trade-offs concerning complexity, 
feasibility and usefulness of different approaches. For the reasons described in [ADR07 Azure as a Hyperscaler](ADRs/07-azure-hyperscaler.md), we opted for Microsoft Azure
as an exemplary public cloud vendor to host the **Hey, Blue!** application and the required infrastructure. This document acts as a decision log for 
any concerns regarding the system architecture solution proposed in the [System Architecture Overview](#0-system-architecture-overview).

## Table of Contents
**[0. System Architecture Overview](#0-system-archituctre-overview)**

**[1. Assumptions](#1-assumptions)**
* [1.1. No Restrictions on Public Cloud](#11-no-restrictions-on-public-cloud)
* [1.2. Available Resources](#12-available-resources)

**[2. Requirements](#2-requirements)**
* [2.1 General](#21-general-requirements-to-system-architecture)
    * [2.1.1 Rapid development for DevOps teams](#211-rapid-development-for-devops-teams)
    * [2.1.2 Security](#212-security)
    * [2.1.3 Quality and Staging](#213-quality-and-staging)
    * [2.1.4 Observability](#214-observability)
* [2.2 From Domain-driven Design ](#22-requirements-due-to-domain-driven-design-solution)
  * [2.2.1 Container Orchestration](#221-container-orchestration)
  * [2.2.2 Messaging System](#222-messaging-system)

**[3. Components](#3-components)**
* [3.1. Client-side Applications](#31-client-side-applications)
* [3.2. Web Application Firewall](#32-web-application-firewall)
* [3.3. API Gateway & Management](#33-api-gateway--management)
* [3.4. Backend Services Orchestration](#34-backend-services-orchestration)
* [3.5. Streaming Service](#35-streaming-service)
* [3.6. Frontend Hosting](#36-frontend-hosting)
* [3.7. Analytics](#37-analytics)
* [3.8. Administration Cockpit (Portal, Loggin, Monitoring, IAM)](#38-administration-cockpit-portal-logging-monitoring-iam)
* [3.9. DevOps Tooling (GitHub)](#39-devops-tooling-github)
* [3.10. GitOps Tooling (Terraform)](#310-gitops-tooling-terraform)


## 0. System Architecture Overview

<p align="center">
<img width="1000" src="cloud_architecture-azure-overview-simple.drawio.png">
</p>

## 1. Assumptions
### 1.1 No Restrictions on Public Cloud
Software architectures building on the foundation of public cloud capabilities enable a rapid ramp-up and a wealth of 
[IaaS](https://www.redhat.com/en/topics/cloud-computing/iaas-vs-paas-vs-saas) and 
[PaaS](https://www.redhat.com/en/topics/cloud-computing/iaas-vs-paas-vs-saas) offerings without the need to provision 
and run self-hosted compute infrastructure. Since we deal with sensitive data of civilians and, more importantly, police 
officers, there might be concerns regarding privacy of data hosted by public cloud providers. We assume that those 
concerns are put out of the way with the appropriate legal frameworks. If not, on-premise hosting solutions or private / 
hybrid cloud solutions might have to be considered which could lead to considerably higher complexity.

### 1.2 Available Resources
A rapid time-to-market with limited resources is to be assumed of great importance. We assume the developer count to
be limited to around 15 people with fond knowledge in modern software development, such as cloud-native approaches,
containerization, orchestration and CI/CD principles. Our design reflects on those limitations by using as many
components of the shelf as possible, limiting development to where it matters - the domain logic and user experience.

## 2. Requirements and Reactions
### 2.1. General Requirements to System Architecture
#### 2.1.1 Rapid development for DevOps teams
We envision a startup-like development environment with limited resource both monetarily and in personnel. The proposed
solution is well attainable for a capable team of about say 10-15 developers, even for production-grade development. 
While an on-prem setup like the one presented above might not be feasible at all, the public cloud abstracts away 
much of the tedious infrastructure work and offers great means of automation. Furthermore, many services like 
monitoring, log analytics or Identity and Access Management (IAM) are at ones disposal by a few clicks.
That way, the team can start development quick and iterate fast without concentrating on low-level technicalities 
or infrastructure concerns.

#### 2.1.2 Security
Hey, Blue! hosts sensitive data. It must under all circumstances prevent data leakage and access by unauthorized parties.
For that reason, we employ battle-tested state-of-the-art components from public cloud providers such as 
Azure Web Application Firewall and Azure Front Door. Our security concerns are rather standard (mainly authentication / authorization)
and can be better handled by standard libraries compared to custom-made solutions.  
Since human beings are often the weakest link in the security equation, the system architecture embodies security and
access control as a first class citizen, displaying and describing access from external system to the Hey, Blue! ecosystem.

##### 2.1.2.1 The Zero-Trust Philosophy
[Zero Trust](https://en.wikipedia.org/wiki/Zero_trust_security_model) is a security model which can be seen as a succession 
or expansion of the good old network security approaches like heavily protected demilitarized zones. While there are 
many aspects in Zero Trust, such as granting the least access privileges needed for any identities, we would like to highlight one particular aspect
- "never trust, always verify": Always perform authentication / authorization, also within a protected zone, e.g., from one service to another.
These capabilities are strongly built-in into our solution via managed identities and OAuth authorization in service-to-service communication in Azure.
 
#### 2.1.3 Quality and Staging
Quality in such a sensible application is of utmost importance. It is important to have a test staging concept, such as
development, staging environments. Especially so since there are many external players (business, media) accessing the 
system that might need to integrate Hey, Blue! interfaces into their own systems. Thus, replication of infrastructure
and deployments is important. By leveraging Terraform and CI/CD principles, we can replicate and configure both 
infrastructure and deployments on various stages, some of which might be used internally only (dev) and some of which
might be accessible from the outside (staging) before finally deploying into production. Without the use of Infrastructure
as Code and CI/CD pipelines this setup becomes very hard to achieve but is feasible if done right from the beginning on.

#### 2.1.4 Observability
Microservice-based architectures possess an inherent complexity when it comes to observability and understanding issues
in a distributed deployment landscape. To remain in control of the situation we leverage industry-leading cloud-based
solutions such as Dapr-sidecars for request-tracing which are inherently inbuilt into Azure Container Apps.  
By using deploying Infrastructure as Code (via Terraform and CI/CD pipelines), it is possible to track deployment states
and make quick adjustments when things go wrong.

### 2.2. Requirements due to Domain-driven Design Solution
#### 2.2.1 Container Orchestration
We based our evaluation of the domain and the functional requirements we opted for a microservice-based architecture
in combination with a messaging system. While this approach might offer great decoupling and scaling capabilities
when done right, it poses great challenges of complexity in the actual implementation. For that reason, we leverage
out-of-the-box cloud-hosted PaaS services for container orchestration such as Azure Container Apps.
It's not reasonable to run a complex native Kubernetes platform, e.g., Azure Kubernetes Service, for the sole 
benefit of flexibility when there exist orchestration tools like Container Apps which offer essential capabilities 
like service-to-service invocation, tracing, secrets management and auto-scaling.

#### 2.2.2 Messaging System
To decouple individual services and enable greater scaling capabilities, we opted for an event-driven architecture. 
That means a sophisticated messaging platform has to be put in place. Luckily we can leverage existing cloud services
like Azure Event Hub for our needs.

## 3. Components
### 3.1. Client-side Applications
We expect the main usage of Hey, Blue! to be via mobile applications. Thus, there should be a dedicated mobile application
in place. However, if development of a mobile application and a web application might pose too much load on development, 
a progressive web application could be considered.

### 3.2. Web Application Firewall
This component vital component secures the backend services from attacks via the HTTP protocol such as SQL injection, 
Cross-Site Scripting or Cookie Poisoning. Azure offers an out-of-the-box solution with many pre-configured policies
to cope with the most-common attacks.

### 3.3. API Gateway & Management
The API Gateway is the entry point for the outer world into the Hey, Blue! service landscape. Client applications 
contact this single point of entry which hosts the backend API specification, performs load balancing and does the routing
next to other tasks like rate limiting and auditing of requests.

### 3.4. Backend Services Orchestration
Our choice for Azure Container Apps has been described in the section on [Container Orchestration](#221-container-orchestration).
Note that the backend service orchestration will host all the services described in the domain architecture before.

### 3.5. Streaming Service
Our choice for Azure Event Hub has been described in the section on [Messaging System](#222-messaging-system).

### 3.6. Frontend Hosting
In order to host the javascript bundles for the Front-End application, we propose Azure Static Web Apps. This service
offers an easy-to-use interface for development. It does one job and does it well. Also, it takes the burden away from 
the developers for complex Cors-configuration which is handled internally.

### 3.7. Analytics
To analyze user behaviour in the front-end or mobile applications, we propose an industry-standard analytics tool such as
[Matomo](https://matomo.org/).

### 3.8. Administration Cockpit (Portal, Logging, Monitoring, IAM)
These components are essential for the development team for security reasons (role-based access control) as well as 
for monitoring the infrastructure, deployments and applications. Most of these tools are in-built and readily available
for all cloud providers.

### 3.9. DevOps Tooling (GitHub)
GitHub is well integrated through so-called Actions with Microsoft Azure, thus being an obvious choice. CI/CD principles
must be embraced from the very beginning to ensure quality and reproducibility of software.

### 3.10. GitOps Tooling (Terraform)
We suggest embracing GitOps principles (Infrastructure as Code incl. versioning and reviewing) from the get-go. While
this might seem overwhelming at first it eases development in later stages considerably and can even account for a
software project to be successful or fail completely at later stages. It enables rapid development and reproducible
environments which is absolutely essential.

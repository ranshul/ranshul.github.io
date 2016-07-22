---
title:  "From OOP to Microservices: Part 2"
date:   2016-06-24 22:37:00
categories: ['Software Architecture', 'Microservices', 'Cloud']
primary: Cloud
---

The alternative for a monolithic architecture is the _microservices architecture_. As the name suggests, the idea is to build a set of smaller, interconnected services. 

The abstraction here is quite similar to modularity - but at the scale of components. Each distinct component (which implements some set of  functionalities) is a microservice - a mini-monolith by itself. 

![Source: Martin Fowler's Blog](http://martinfowler.com/articles/microservices/images/sketch.png)
Source: [Martin Fowler's blog](http://martinfowler.com/articles/microservices.html)

The next step is to make these microservices communicate. The communication can be synchronous or asynchronous[^1]. This is done by exposing the APIs[^2] of services to each other. One popular and scalable way of doing this is the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architecture.

Quick review of REST (gist of wiki):

1. **Client-Server Model** - Clients and Servers[^3] have clearly delineated functionalities.

2. **Stateless** - Server does not store any context. When a client asks a server for help, it sends all the information the server would need to make that decision. State can be held by an external database (another service) for persistency of state (like login sessions).

3. Service is accessed and manipulated using HTTP Methods (GET, PUT, POST, DELETE) URLs. Ex. A HTTP GET on `http://rest-example.com/resources/` can return all the results from the service's database.

Back to microservices, a popular way of communication is exposing REST endpoints in each microservice. Each of these microservice could be running as a Docker container on the cloud - possibly multiple instances for HA. And, there can be a load balancer in front of these containers.

Another significant change required is the way the databases and the application interact. For loose coupling, each microservice should have its own database. An added advantage here is that each of these databases can be different - one can be a NoSQL db optimized for a specific purpose and another one could be a traditional MySQL database.

![Source: Martin Fowler's Blog](http://martinfowler.com/articles/microservices/images/decentralised-data.png)
Source: [Martin Fowler's blog](http://martinfowler.com/articles/microservices.html)

Like every other architecture, microservices have their own drawbacks. The primary point of concern is the complexity arising from the distributed nature of microservices. [CAP theorem](http://ksat.me/a-plain-english-introduction-to-cap-theorem/) comes into play here. From a developer's perspective, debugging microservices is no easy task. And, the rollout of changes in a set of interdependent microservices has to be carefully planned. [Kubernetes](http://kubernetes.io/) with Docker can bring some organization and automation into the chaos that is deploying microservices.

This barely does justice to the complexities in the microservices architecture - but is a good starting point. The complexities, in general, slowly start cropping up in natural progression as the microservice is built.

[^1]: A sends message to B and doesn't wait for B to respond.
[^2]: A set of functions in the component
[^3]: clients seek services ("tell me the IP address of google.com"), servers provide them (DNS Server).

*[APIs]: Application Programming Interfaces
*[HA]: High Availability
---
title:  "From OOP to Microservices: Part 3"
date:   2016-07-26 22:37:00
categories: ['Microservices', 'Service Registry', 'Cloud']
primary: Cloud
---

### Microservices - The selfish, spoilt brat you can't ignore

Microservices are **loosely-coupled** functional units that concern themselves with one primary set of functions. They're independent of other microservices - 

1. You don't have to spend eons tracking down the source of that one weird variable that didn't make sense; only to find out that it was carried over from a different project. [^1]

2. You can choose a different stack or language for this particular functionality with - one service can be written in Python because `scipy` and `numpy` are good fits and another service can be written in Java because it's critical and battle-tested.

3. If you've let a bug slip by (shh), you can always correct it immediately without a lot of effort.

TL;DR, microservices are independent services that do small things well by their own free will. 

Like any other selfish brat, microservices are bad at sharing. They need their own databases - loosely coupled architecture. So, if more than one microservice needs the same data, we have to take care of concurrency in database transactions. This is a bottleneck for scalability as well.

More importantly, they take their own sweet time to do things and sometimes, microservices can be moody. So, you need to deal with microservices that rudely say _"I need some time off"_  and go away. To address this, we use a _service discovery and registry_ pattern/service. More on this soon.

And how do microservices talk to each other? Through _API proxying_ or _API gateways_. This is fancy speak for `http://microservice1:8010/apps/app1/gimmeResult`. `microservice2` can ask `microservice1` for data by sending a GET result to that URL. Similarly, actions can be defined for each HTTP method.

Complications like dependencies, data sharing, graceful failure and so on can be resolved with some software design patterns or using existing solutions with small modifications. The remainder of this post deals with one such complication (availability of a service) and dealing with it through **service discovery and registration**.

### A Rollcall for Microservices

Here's the question: how will you know the IP and port for a service that can reside in multiple hosts?

If you're tempted to say that I'll fix a dedicated set of IPs and ports, you're not thinking about scale. This is a __static configuration__ and once you start scaling your service, you will run into a complication - if you auto/manually scale your service frequently, the locations and ports of the services will be changing and you can't expect the client (can be a lazy guy like me) to keep track of your proclivity for scaling. 

In simple words, `http://microservice1:8010/apps/app1/gimmeResult` will no longer exist; it could become `http://uglyRandomIPAddress:1234/apps/app1/gimmeResult` when one of your machines go down and you bring up a new one. When can this happen? When you're on the cloud and use AWS to bring up new machines or ask machines to go gentle into that good night... [^3]

So what do we do? We create a database which keeps track of IP addresses and the services they provide. If the system/microservice goes down, the corresponding entry in the database is removed. The exact details of how this is done depends on a particular system's design. So how is this done?

**Service Registration** - Every service is to be added to the central database (called the service registry henceforth). Otherwise, we wouldn't know that it exists. What is added? A tuple like `(IP Address, Port, Service Type, Metadata)` could be one possible schema for the service registry. A heartbeat protocol is a popular way of doing things. The service tells the central service registry to register itself and the service registry says something along the lines of "Send you status to me every 1 minute. If you don't send one for 3 entire minutes, you're dead to me." When the service registry thinks a service is dead, the entry is gone from the list.

**Service Discovery** - A random creature seeking service will ask the central service registry for info. The service registry will provide info about the service it's looking for. For example, let

| IP Address | Port | Service | Status|
| - | - | - | - |
|106.105.104.3| 9911 | Spoil GoT | UP |
|106.105.104.4| 9911 | Spoil GoT | DOWN |
|106.105.104.5| 9911 | Spoil GoT | UP |
|101.102.103.4| 11 | Suggest book | UP |
|101.102.103.1| 11 | Suggest book | DOWN |
|210.168.126.84| 42 | Answer to Life | UP |
|134.12.36.81| 41 | Pokemon Go | DOWN |

be the service registry. When you're bored and want the service that spoils GoT for a friend, you ask the service registry. It returns one of `106.105.104.3` and `106.105.104.5` because common sense says a `DOWN` status probably means the service in `106.105.104.4` isn't working. When you ask for Pokemon Go, it will eternally say no service available.

Yay, all done? No. There could be millions of people who want the answer to life. If Answer to Life belongs to an ASG [^4] and the demand/load increases, more nodes with the Answer to Life service can spawn. These new nodes will register themselves in the service registry to deal with more and more Answer to Life requests.

Two questions. 

1. How does the service registry pick an IP address for a service request (if there are multiple nodes offering the same service)?

2. When do these nodes go away and who tells AWS to increase the desired capacity of the ASG when needed?

The first question is the same as asking "What is your load balancing strategy?". The second question can be solved by writing a service (yes, another one) that monitors the service registry and takes these decisions. Or better yet, integrate the registry into this service and call it a discovery server. [^5]

Another complication here is the discovery server failing - a single point failure. If the discovery server breaks down, all functioning comes to a halt - new clients will not know where the services are. If the registry is in-memory (like it is in Netflix's Eureka), design patterns for High Availability (HA) can be used - if one discovery server goes down, there will be a backup which will take its place without affecting overall system functionality. Once again, CAP theorem plays an important role in choosing the right open source service discovery framework.

[^1]: Yes, this rant may have been inspired by a personal incident. 

[^2]: A random (nit: logical) computer for hire from Amazon. It has its own IP address and all.

[^3]: Hint: Interstellar (2015)

[^4]: AWS jargon. AutoScaling Group (ASG) is a dynamic list of instances. The list will always have a minimum, maximum and the desired number of AWS instances that should be present in the ASG.

[^5]: There are other approaches - etcd, Doozer, Zookeper, Spotify DNS, Airbnb SmartStack and so on. I worked with Netflix's Eureka.

*[SaaS]: Software as a Service
*[AWS]: Amazon Web Services
*[GoT]: Game of Thrones
*[ASG]: Autoscaling Group
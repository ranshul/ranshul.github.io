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

Like any other selfish brat, microservices are bad at sharing. They need their own databases - loosely couple architecture. So, if more than one microservice needs the same data, we have to take care of concurrency in database transactions. This is a bottleneck for scalability as well.

More importantly, they take their own sweet time to do things and sometimes, microservices can be moody. So, you need to deal with microservices that rudely say _"I need some time off"_  and go away. For this, there's another piece of software which deals with _service discovery and registry_. More on this soon.

And how do microservices talk to each other? Through _API proxying_ or _API gateways_. This is fancy speak for `http://microservice1:8010/apps/app1/gimmeResult`. `microservice2` can ask `microservice1` for data by sending a GET result to that URL. Similarly, actions can be defined for each HTTP method.

Complications like dependencies, data sharing, graceful failure and so on can be resolved with some software design patterns or using existing solutions with small modifications. The remainder of this post deals with one such complication (availability of a service) and dealing with it through **service discovery and registration**.

### A Rollcall for Microservices

Here's the question: how will you know the IP and port for a service that can be on multiple hosts?

If you're tempted to say that I'll fix a dedicated set of IPs and ports, you're not thinking about scale. This is a __static configuration__ and once you start scaling your service, you will run into a complication - if you auto/manually scale your service frequently, the locations and ports of the services will be changing and you can't expect the client (can be a lazy guy like me) to keep track of your proclivity for scaling. 

In simple words, `http://microservice1:8010/apps/app1/gimmeResult` will no longer exist. It could become `http://uglyRandomIPAddress:1234/apps/app1/gimmeResult` when one of your machines go down and you bring up a new one. When can this happen? When you're on the cloud and use AWS to bring up new machines or ask machines to go gentle into that good night... [^3]


[^1]: Yes, this rant may have been inspired by a personal incident. 

[^2]: A random (nit: logical) computer for hire from Amazon. It has its own IP address and all.

[^3]: Hint: Interstellar (2015)

*[SaaS]: Software as a Service
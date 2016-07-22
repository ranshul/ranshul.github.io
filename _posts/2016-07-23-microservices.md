---
title:  "From OOP to Microservices #1"
date:   2016-07-23 22:37:00
categories: ['Software Architecture', 'Microservices', 'Cloud']
primary: Cloud
---

In my Object Oriented Programming class, I learnt about the importance of making things modular, highly cohesive [^1] and less coupled [^2]. It sounded like common sense - why wouldn't you want to organize your functions and blackbox them? 

Traditionally, all the functionalities are bundled into one package and run as a process. In the context of Java, a single JAR file can be built and run. For example, [Qubole](https://www.qubole.com/)[^3] runs the in-house [Hive](https://en.wikipedia.org/wiki/Apache_Hive) jar on AWS instances to offer Hive as a Service. This is an example of _monolothic architecture_ - all functionality is in a single process.

Naturally, the same idea was extended to building websites offering different services. A website like Flipkart can be thought of as a single website/platform with multiple components like Products, Users, Authentication and so on. For now, let's assume all of these are different classes bundled into one jar file.

It's simple to

1. *develop* - IDEs are built for development of monolithic applications/
2. *deploy* - to get the website running, all you have to do is put the gigantic jar file in some server and run it.
3. *scale* (horizontally) - get more servers (AWS or otherwise), put the jar file in all of them and put the servers behind a load balancer[^4]. (then break your head about concurrency issues)

Unless you've dealt with really large codebases, the drawbacks of this simple approach to software development are not apparent. 

1. It's nearly impossible (and not worth the effort) to know the entire codebase to know how it works - critical for making modifications and bug fixes.
2. OOP principles are mostly adhered to. There are some instances where it's just not worth it - results in breakdown of modularity. That decreases quality of code which reduces understandability. So, new modifications and bug fixes will make the code even less modular and you end up in a downward spiral.
3. Extremely high startup and deployment time.
4. If there is a memory leak or some bug in say, the Authentication component of Flipkart,
	1. It affects the entire application - reliability lost
	2. All the servers will be plagued by the same bug.
	3. Fix is to redeploy - people won't be able to view the Products the page for a while.
5. In the context of SaaS and cloud, different components may be suited for different types of AWS instances. A monolith forces us to stick with one type of instance.
6. You can't simply shift to a shiny new language or framework that's tailor-made for one of your components or a new feature.

Effectively, a big business-critical monolith that's understood well by few to none is a flight risk. And it doesn't play well with agile development or the DevOps culture. Continuous deployment and hotfixes are next to impossible to achieve without downtime. 

[^1]: a class performs a highly specific set of functions
[^2]: dependency between classes is less - a major change in one class should not affect the other
[^3]: I interned there :P
[^4]: a load balancer is your point of contact which redirects you to one of the servers. It shields you from having to choose servers and works to serve a lot of requests to the applications. 
*[RoR]: Ruby on Rails
*[SaaS]: Software as a Service
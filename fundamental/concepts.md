## Maintaining Model Integrity 


>Cells can exist because their membranes define what is in and out and determine what can pass - Eric S Evans, Domain Driven Design

> A model has to be internally consistent;

A model is said to be internally conistent, such that it's terms always have the same meaning, and that it contains no contradictory rules.

Infering from the quote above, this not an option rather an essential fundamental requirement of the model.

We seldom think about this essentiality of the model that makes it a `model`.

> The internal consistency of a model, such that each term is unambiguous and
no rules contradict, is called <b>unification</b> - Eric S Evans, DDD


Ideally we would just have single model across the complete enterprise domain, this could be are `unified` Domain Model, without any contradictory or overlapping definitions of terms and every logical statement about the product domain will be consitent.

However, when it comes to engineering large scale complex software development, we can be certain that this is not the ideal world.
Having single `unified` model across the complex bussiness domain we are trying to tackle, it's completely out of the eqution.
In order to maintain the `internally consistent` and `unified` model outweighs the benifits it will provide.

<!-- It is necessary to allow multiple models to develop in different parts of the system, but we need to make careful choices
about which parts of the system will be allowed to diverge and what their relationship to each
other will be. We need ways of keeping crucial parts of the model tightly unified. None of this
happens by itself or through good intentions. It happens only through conscious design decisions
and institution of specific processes. Total unification of the domain model for a large
system will not be feasible or cost-effective.

In an ideal world, we would have a single model spanning the whole domain of the enterprise. This
model would be unified, without any contradictory or overlapping definitions of terms. Every logical
statement about the domain would be consistent.
But the world of large systems development is not the ideal world. To maintain that level of
unification in an entire enterprise system is more trouble than it is worth. It is necessary to allow
multiple models to develop in different parts of the system, but we need to make careful choices
about which parts of the system will be allowed to diverge and what their relationship to each
other will be. We need ways of keeping crucial parts of the model tightly unified. None of this
happens by itself or through good intentions. It happens only through conscious design decisions
and institution of specific processes. Total unification of the domain model for a large
system will not be feasible or cost-effective.
Sometimes people fight this fact. Most people see the price that multiple models exact by limiting
integration and making communication cumbersome. On top of that, having more than one model
somehow seems inelegant. This resistance to multiple models sometimes leads to very ambitious
attempts to unify all the software in a large project under a single model. I know I've been guilty
of this kind of overreaching. But consider the risks.
1. Too many legacy replacements may be attempted at once.
2. Large projects may bog down because the coordination over-head exceeds their abilities.
Applications with specialized requirements may have to use models that don't fully satisfy
their needs, forcing them to put behavior elsewhere.
3.
Conversely, attempting to satisfy everyone with a single model may lead to complex options
that make the model difficult to use.
4.
What's more, model divergences are as likely to come from political fragmentation and differing
management priorities as from technical concerns. And the emergence of different models can be
a result of team organization and development process. So even when no technical factor prevents
full integration, the project may still face multiple models.
Given that it isn't feasible to maintain a unified model for an entire enterprise, we don't have to
leave ourselves at the mercy of events. Through a combination of proactive decisions about what
should be unified and pragmatic recognition of what is not unified, we can create a clear, shared
picture of the situation. With that in hand, we can set about making sure that the parts we want to
unify stay that way, and the parts that are not unified don't cause confusion or corruption.
We need a way to mark the boundaries and relationships between different models. We need to
choose our strategy consciously and then follow our strategy consistently.
This chapter lays out techniques for recognizing, communicating, and choosing the limits of a
model and its relationships to others. It all starts with mapping the current terrain of the project. A
BOUNDED CONTEXT defines the range of applicability of each model, while a CONTEXT MAP gives a
global overview of the project's contexts and the relationships between them. This reduction of
ambiguity will, in and of itself, change the way things happen on the project, but it isn't necessarily
enough. Once we have a CONTEXT BOUNDED, a process of CONTINUOUS INTEGRATION will keep the
model unified.
Then, starting from this stable situation, we can start to migrate toward more effective strategies
for BOUNDING CONTEXTS and relating them, ranging from closely allied contexts with SHARED
KERNELS to loosely coupled models that go their SEPARATE WAYS. -->



## Bounded Context

>A BOUNDED CONTEXT defines the range of applicability of each model, while a CONTEXT MAP gives a
global overview of the project's contexts and the relationships between them. This reduction of
ambiguity will, in and of itself, change the way things happen on the project, but it isn't necessarily
enough. Once we have a CONTEXT BOUNDED, a process of CONTINUOUS INTEGRATION will keep the
model unified. - Eric S Evans , Domain Driven Design




## Anti Corruption Layer


## Context Map


## Inversion of Control


## Dependency Injection

## Api Gateway

### Micro Services and Consumers
Microservices are a service oriented architecture where teams can design, develop and ship their applications independently. Taking this approach allows technology diversity on various levels of the system, where teams can benefit from using the best language, database, protocol, and transportation layer for the given technical challenge.

For example, one team can use JSON over HTTP REST while the other team can use gRPC over HTTP/2 or a messaging broker like RabbitMQ.

Using different data serialization and protocols can be powerful in certain situations, but clients that want to consume our product may have different requirements.

The problem can also occur in systems with homogeneous technology stack as consumers can vary from a desktop browser through mobile devices and gaming consoles to legacy systems. One client may expect XML format while the other one wants JSON. In many cases, you need to support both.

Another challenge that you can face when clients want to consume your microservices comes from generic shared logic like authentication, as you don't want to re-implement the same thing in all of your services.

> To summarize: we don't want to implement our internal services in our microservices architecture in a way to support multiple clients and re-implement the same logic all over. This is where the API Gateway comes into the picture and provides a shared layer to handle differences between service protocols and fulfills the requirements of specific clients.

### What is an API Gateway?
API Gateway is a type of service in a microservices architecture which provides a shared layer and API for clients to communicate with internal services. The API Gateway can route requests, transform protocols, aggregate data and implement shared logic like authentication and rate-limiters.

You can think about API Gateway as the entry point to our microservices world.
Our system can have one or multiple API Gateways, depending on the clients' requirements. For example, we can have a separate gateway for desktop browsers, mobile applications and public API(s) as well.

[![gateway](/micro-frontends/api-gateway-1.png)](#)

As API Gateway provides functionality for client applications like browsers - it can be implemented and managed by the team who is responsible for the frontend application.

It also means that the language API Gateway is implemented in language should be chosen by the team who is responsible for the particular client. As JavaScript is the primary language to develop applications for the browser, Node.js can be an excellent choice to implement an API Gateway even if our micro services architecture is will be developed in a different language.

Netflix successfully uses Node.js API Gateways with their Java backend to support a broad range of clients - to learn more about their approach read The "Paved Road" PaaS for Microservices at Netflix article.

Setting up an independent gateway for frontend was not required in the the case of Innovaccer Healthcare Platform. Even though we are doing Micro Services Approach for frontend , but we are doing development time integration to compose Innovaccer Healthcare Platform with independent Frontend Applications.

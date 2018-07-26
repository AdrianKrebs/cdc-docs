---
title: Introduction
---

# Introduction to CDC Testing

In order to countiniously and independently deploy software, the microservice architecture became state of the art for large scale enterprise applications.

![architecture](microservice.jpg)
- <b>Monolith: </b>detect breaking changes at <b>compile time</b>
- <b>Micorservice: </b>detect breaking changes at <b>runtime</b>

**Imagine the following situation:**

The service provider changes his interface, refactors code, refactors tests as well, everything green</br>
In the providers CI/CD pipline, all tests are passing</br>
GO for Production!</br>
Monitoring shows no Errors for the ProviderService</br>

But suddenly the ConsumerService fails on every request...</br>
Blaming starts...</br>
Maybe we should have tested this...</br>
But how?</br>

**Obvious solution: End-to-End Tests**

Start the frontend. Start other consumed services. We need test data. In addition, we need tokens, certificates, authentication, ...

What a nightmare! This results in a full End-to-End Tests even though we wanted to test the interaction in between the two services only.
We want to know very early and fast that we break something.

**Contracts come into play**

![teams](teams.jpg)
 

The <a href="https://docs.pact.io">Pact Framework</a> offers CDC testing implementations for most languages (Java, Javascript, Ruby, Swift, Android, Go). 
Generally this includes a Domain Specific Language for generating JSON based contracts and a framework for executing tests against it.


## Provider Workflow
**Goal**: Don't deploy breaking changes
- Implements Changes
- Get contracts from all consumers 
- Replay and verify interactions - Stop introduction of breaking change very early
- Deploy service

## Consumer Workflow
<b>Goal</b>: Dont consume resources which are not provided
- Implement changes
- Generate contract
- Push contract to provider
- Each provider verifies contracts
- Deploy service 

::: tip
Use the <a href="https://martinfowler.com/bliki/TolerantReader.html">Tolerant Reader Pattern</a> as a verification philosophy (unnecessary fields are ignored)
:::
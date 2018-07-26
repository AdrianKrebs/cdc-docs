---
title: Introduction
---

# Introduction to CDC Testing

In order to countiniously and independently deploy software, the microservice architecture became state of the art for large scale enterprise applications.

![architecture](microservice.jpg)
- <b>Monolith: </b>detect breaking changes at <b>compile time</b>
- <b>Micorservice: </b>detect breaking changes at <b>runtime</b>

Imagine the following situation: </br>
Provider changes his interface, refactors, refactors tests as well, everything green</br>
In the providers CI/CD pipline, all tests are passing</br>
GO for Production! </br>
Monitoring shows no Errors for the ProviderService</br>

But suddenly the ConsumerService fails every request...</br>
Blaming starts...</br>
Maybe we should have tested this...</br>
But how?</br>

Obvious solution: Service Tests  </br>
Start the frontend. Start other consumed services. We need test data. In addition, we need tokens, certificates, authentication, ...</br>
What a nightmare! This results in a full E2E test. But we only wanted test the interaction between the two services.</br>
We want to know very early and fast that we break something.

<b>Contracts come into play</b>

![teams](teams.jpg)
 

The <a href="https://docs.pact.io">Pact Framework</a> offers CDC testing implementations for most languages (Java, Javascript, Ruby, Swift, Android, Go). <br>


## Provider Workflow
<b>Goal</b>: Don't deploy breaking changes
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
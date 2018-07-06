---
title: Introduction
---

# Introduction to CDC Testing

- Monolith -> detect breaking changes at compile time
- Micorservice -> detect breaking changes at runtime

Provider changes interface, refactors, refactors tests as well, everything green
CI/CD Pipline all tests are passing
GO FOR Production!
Monitoring shows no Errors in UserService

But suddenly the LoginService fails every requst...
Blaming starts...
Maybe we should have tested this!
Test what?

Obvious Solution: Service Tests -> Start the frontend, oh it needs IP-Check-Service, oh we need test-data... oh we need tokens, certificates, authentication
Nightmare! Results in a full E2E test. We only wanted test the interaction between the two services!
We want to know very early that we break something, AND we want to know it fast!

Contracts come into play.

Pact Framework (no real alternative). Implementations for most languages (IOS App as Consumer, Java or Ruby as Provider -> no problem)
Verification philosophy: Tolerant Reader! (more fields are ignored)

## Provider Workflow
- <b>Goal</b>: Don't deploy breaking changes
- Implements Changes
- Get Pacts from Broker 
- Replay and Verify Interactions -> Stop introduction of breaking change very early
- deploy service

## Consumer Workflow
- <b>Goal</b>: Dont consume resources which are not provided
- Implement changes
- Generate Pacts (Build)
- Push Pacts to Broker
- Each Provider verifies Pact
- deploy service 

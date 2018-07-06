---
title: Implementation
---

## DSL
Pact provides an easy to use DSL to write your customer expectations.


### Headers
The Pact DSL offers a convinient way to do a matching on request and response headers.

```
.given("default")
	.uponReceiving("retrieving goal data")
	.path("/goals")
	.headers(requestHeaders)
```

### Authentication
Sometimes you may need to add things to the requests that can't be persisted in a pact file. Examples of these would be authentication tokens, which have a small life span.

```
@TargetRequestFilter
public void exampleRequestFilter(HttpRequest request) {
	request.addHeader("Authorization", "OAUTH hdsagasjhgdjashgdah...");
}
```

## Pact Broker
How do could we share Pacts?
Sharing contracts by email works pretty well. But as soon as you have more and more services using pact, you need some kind of versioning for your pacts.
The soulution is called <a href="https://github.com/pact-foundation/pact_broker">Pact Broker</a>.

A Pact Broker isn't just a file storage:

- The Broker provides Versioning, Tagging, Rest-API
- Dependency Graph 
- Living documentation for free - For every new pact
- Build Pipeline integration

![broker](broker.jpg)

<a href="https://pact-broker.cdc.panter.biz/">Example Broker</a>


### How to setup a pact broker:
- ``docker pull dius/pact-broker``
- Setup a postgres database
- Ready to go

### How to publish pact files to the broker:
- Configure brokerUrl and pactDirectory
- Run mvn pact:publish to push your contracts to the broker

### How to pull pact files from the broker:
- Run mvn pact:verify to pull all the contracts and verify your provider against it
- Publish results of a provider verification with setting the following system property:
``-Dpact.verifier.publishResults=true``

## How to add new interactions without breaking everything

In summary: keep the CI running against a stable version of the pact, while simultaneously providing a new version for the provider team so they can update their code and provider states.
Once both the stable and the new versions of the pact are green, the new version can be published as the stable version.


- Tags can be used to enable you to add new interactions without breaking all the builds. This approach works well if you use feature branches for development, and release from a production branch.
- Tags can be used to enable you to test consumer and provider head and prod versions against each other. 

Tagging approaches
- Automatically tag with branch name when pact is published
- Manually tag production or feature pacts (no tagging for each environment needed since pacts only run on integration)

 Source: <a href="https://github.com/pact-foundation/pact_broker/wiki/Using-tags">Pact Foundation - Using tags</a>


 ## Pact in comparison to other API testing/documentation frameworks

| Feature       					| Pact          | Restdocs  | Swagger 	|
| --------------------------------- | ------------- | --------- | ---------	|
| Live Documentation      			| yes	 		| yes		| no		|
| HATEOAS Support      				| yes		    | yes 		| no 		|
| Integration/ Maintainance effort 	| high      	| high 		| low		|
| Contracts							| no			| no		| no		|
| Versioning	 					| yes			| partly	| partly	|
| Universallity 					| yes			| partly	| no		|



## Experiences
::: tip Experiences
- Increases confidence when coding & deploying (CD)
- Collaborative API design
- Living API documentation
:::

## Pitfalls
::: warning Pitfalls
- Provider state setup for each interaction (Pre conditions, state  - even if it should be stateless)
- Getting used to DSL is hard
- Don't limit to happy path (Response code when user from UserService doesn't exist)
- Automation isn't trivial (Pipeline)
:::


## Limitations
- Limited Media Type support (no XML)
- No Websocket support



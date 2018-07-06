---
title: Implementation
---

## Why pact framework?

| Feature       					| Pact          | Restdocs  | Swagger 	|
| --------------------------------- | ------------- | --------- | ---------	|
| Live Doku      					| ja	 		| ja		| nein		|
| HATEOAS Support      				| ja		    | ja 		| nein 		|
| Integration/ Maintainance effort 	| gross      	| gross 	| gross		|
| Contracts							| nein			| nein		| nein		|
| Versionierung 					| ja			| jein		| jein		|
| Universelles Konzept 				| ja			| jein		| nein		|


## DSL



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
How do we share these Pacts?
- Email works pretty well (even at Mobi)
- But no versioning, more and more services

Solution: Pact Broker!
- Not just a file storage
- Dependency Graph 
- Living documentation for free - For every new pact
- Build Pipeline integration -> The Broker provides Versioning, Tagging, Rest-API

How to public pact files:
- configure maven plugin https://github.com/DiUS/pact-jvm/tree/master/pact-jvm-provider-maven
- Configure brokerUrl and pactDirectory
- run mvn pact:publish to push your contracts to the broker
- run mvn pact:verify to pull all the contracts and verify your provider against it

https://github.com/pact-foundation/pact_broker

How to setup a pact broker:
- docker pull dius/pact-broker
- setup a postgres database
- Set environment variables in order to connect to a postgres db
- https://github.com/DiUS/pact_broker-docker

Publish results of a provider verificatio with setting the following system property:
``-Dpact.verifier.publishResults=true``



## How to add new interactions without breaking everything

In summary: keep the CI running against a stable version of the pact, while simultaneously providing a new version for the provider team so they can update their code and provider states.
Once both the stable and the new versions of the pact are green, the new version can be published as the stable version.

- from https://github.com/pact-foundation/pact_broker/wiki/Using-tags
- Tags can be used to enable you to add new interactions without breaking all the builds. This approach works well if you use feature branches for development, and release from a production branch.
- Tags can be used to enable you to test consumer and provider head and prod versions against each other. 

Tagging approaches
- Automatically tag with branch name when pact is published (`git rev-parse --abbrev-ref HEAD`)
- Manually tag production or feature pacts (no tagging for each environment needed since pacts only run on integration)


## Experiences
- Increases confidence when coding & deploying (CD)
- Collaborative API design
- Living API documentation

## Pitfalls
- Provider state setup for each interaction (Pre conditions, state (even if it should be stateless)
- Getting used to DSL is hard
- Don't limit to happy path (Response code when user from UserService doesn't exist)
- Automation isn't trivial (Pipeline)


## Limitations
- Limited Media Type support
- No Websocket Support
- HTTP2 differences?



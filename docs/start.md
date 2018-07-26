---
title: Getting Started
---

# Getting Started

The following samples are written in Java with JUnit5. See <a href="https://docs.pact.io/implementation-guides">implementation guide</a> for other languages.

## Service Consumer

Add the following dependencies to your pom.xml:

```
<dependency>
      <groupId>au.com.dius</groupId>
      <artifactId>pact-jvm-consumer-junit5_2.12</artifactId>
      <version>3.5.17</version>
      <scope>test</scope>
</dependency>
<dependency>
      <groupId>au.com.dius</groupId>
      <artifactId>pact-jvm-consumer-java8_2.12</artifactId>
      <version>3.5.18</version>
</dependency>
```

Write your expectations to the provider in form of a unit test:

```
@Pact(provider="GoalProvider", consumer="GoalConsumer")
public RequestResponsePact getGoalPact(PactDslWithProvider builder) {

final DslPart body = LambdaDsl.newJsonArray((rootArray) ->{
	rootArray.object((goal) -> {
		goal.stringType("description","stop smoking");
		goal.decimalType("amount",new Double("10.00"));
		goal.date("deadline");
		goal.object("person", (personObj -> {
			personObj.stringType("firstName","Herbert");
			personObj.stringType("lastName","Panter");
			personObj.stringType("paymentToken","xyz");
			personObj.stringType("email","example@example.com");
		}));
	});
}).build();

	return builder
		.given("default")
		.uponReceiving("retrieving goal data")
		.path("/goals")
		.headers(requestHeaders)
		.method("GET")
		.willRespondWith()
		.status(200)
		.body(body)
		.toPact();
}

```



<a href="https://git.panter.ch/panter/pan-103-consumer">Cosnumer Example</a>



## Service Provider

Add the following dependencies to your pom.xml:

```
<dependency>
    <groupId>au.com.dius</groupId>
    <artifactId>pact-jvm-provider-junit5_2.11</artifactId>
    <version>3.5.18</version>
    <scope>test</scope>
</dependency>

```

Verify the expectations of all consumers against your service:


```
@TestTemplate
@ExtendWith(PactVerificationInvocationContextProvider.class)
void pactVerificationTestTemplate(PactVerificationContext context) {
	context.verifyInteraction();
}

```



<a href="https://git.panter.ch/panter/pan-103-provider">Provider Example</a>
# Overview

I would create a microservice (in case the current architecture allows it) or service (in case the current product is monolith) named **NewsFeeder**.

The service itself could be separated in two parts:

- **DataProcessor** - would be started as a background job (.NET/Java background jobs/Hangfire) or AWS/Azure trigger queues if possible:
  - Client – Entry point of the processor. This is where the work starts after an event has been received or time period has passed.

  - Business (Service/Logic/Domain) - It would consume news from any provider [configuration] (it could be list of providers with different service/logic implementations) and enrich the data with any information you can think of (domain rules)

  - DAL (Repository) – writes the enriched data in the storage: non-sql db (MongoDB) or cloud provided one – CosmosDB/DynamoDB with considerable TTL period (configuration).

- **NewsApi** – normal web/console API application
  - View (Client) – API which should be used by consumers/clients of the feeder.

    API Design:

    Expose one [authorized] endpoint for querying news data with filter (N days, default values, text search)

    Expose one [authorized] endpoint for subscribing.

    Expose one [non-authorized] public endpoint for getting TOP 5 latest news for conversion tool.

  - Business (Service/Logic/Domain) – It would do some domain specific translation (if needed) and communicate with the DAL.

  - DAL (Repository) – fetch the data from the storage.

# Real solutions:

1. AWS/Azure functions with DynamoDB/CosmosDB

  Pros:

  - Easy to implement
  - Easy to scale
  - Reliable
  - No deep infrastructure design concern/skills needed (in the current case)
  - Cost efficient - pay per usage (it depends on the usage)

  Cons:

  - Lack of deep control over the whole flow
  - Newer tech stack that might not fit with the current technology
  - Security and privacy risk
  - Cost concerns (it depends on the usage)

2. .NET core apps with MongoDB

  Pros:

  - Already familiar with the stack
  - Deep control of the environments/configuration/runtime
  - Great community and support
  - Fast and reliable framework

  Cons:

  - Harder to scale
  - Infrastructure skills needed

# Task lists:

- [US] Create DB design and scheme – 1-2 md

- [US] Create common DAL layer with all repos needed – 3 md
  - Implement repositories
  - Add ORM if needed
  - Unit tests
  - Configrations

- [US] Create **DataProcessor** business logic – 3 md
  - Different providers implementation
  - Enrich data logic/classes
  - Unit tests

- [US] **DataProcessor** client (entry point) – 1 md
  - Subscribe/configure logic
  - Connection with services layer
  - Unit tests??

- [US] **NewsApi** business logic – 1 md
  - Translation logic
  - Unit tests

- [US] **NewsApi** API design/implementation – 2md
  - API design
  - Authorization
  - Unit tests

- [US] Infrastructure – (depends from the choice – cloud or on-premise) – 2-3 md
  - Structure
  - Configuration
  - Environment

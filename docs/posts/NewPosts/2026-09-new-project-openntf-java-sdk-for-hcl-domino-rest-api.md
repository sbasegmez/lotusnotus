---
title: "New Project: OpenNTF Java SDK for HCL Domino REST API"
slug: project-openntf-java-sdk-hcl-domino-rest-api
date: 2026-09-11T09:29:59.143Z
draft: false
tags:
    - announcement
    - community
    - java
    - open-source
    - openntf
    - rest-apis
    - drapi
categories:
    - OpenNTF
description: OpenNTF Java SDK for HCL Domino REST API — early preview simplifying Java access to Domino REST; JWT auth, async/sync API. Get it on GitHub.
---

# A Java SDK for HCL Domino REST API

I've released an early preview of the [OpenNTF Java SDK for HCL Domino REST API](https://github.com/sbasegmez/openntf-drapi-java-sdk).

The idea is quite simple: I wanted to use the Domino REST API from Java without dealing with HTTP requests, JSON parsing and all the other plumbing every time. The SDK wraps some of the common operations, such as working with documents and lists, behind a Java API.

It is still early days. Only part of the Domino REST API is implemented, and I'm still experimenting with some parts of the API design. That's actually one of the reasons I'm publishing it now rather than waiting until everything is finished.

This is an OpenNTF project, so contributions are welcome. If you're interested in using it, I'd be particularly interested in feedback about the API design before too much of it becomes difficult to change.

The source is available on [GitHub](https://github.com/sbasegmez/openntf-drapi-java-sdk) under the Apache 2.0 licence.

<!-- more -->

## Why a Java SDK for HCL Domino REST API?

HCL Domino REST API gives us a standard HTTP interface to Domino data. That also means you can already use it from pretty much any Java application using an HTTP client.

Of course, "you can call the REST API" and "it is pleasant to call the REST API" are slightly different things.

For anything beyond a few requests, you quickly start dealing with authentication, URLs, query parameters, JSON serialisation, error handling and mapping responses into something useful in Java.

HCL already provides a [Golang SDK](https://github.com/HCL-TECH-SOFTWARE/domino-rest-sdk-go) and a [Node.js SDK](https://github.com/HCL-TECH-SOFTWARE/domino-rest-sdk-node), but there is no official Java SDK at the moment.

So, there was a gap.

There is also a second, slightly sneaky reason for this project.

I've been working with Java for many years, but much of my Domino Java work has inevitably been shaped by the Java versions available in Domino. Meanwhile, Java has changed quite a lot.

I wanted a project where I could spend some time with newer Java versions, revisit some of my old habits, try different design patterns and generally catch up with parts of the Java ecosystem I haven't had much reason to use.

Writing an SDK from scratch seemed like a suitably complicated way of doing that :)

## Where are we now?

This is still a preview.

The main goal at this point is to get enough of the SDK working to test whether the overall design makes sense. Some pieces are usable, some are incomplete, and quite a few APIs are still missing.

Here is what is there so far.

### Authentication

The SDK currently supports JWT authentication.

You can either provide an existing token or let the SDK obtain one using a username and password. Token expiration and renewal are handled by the SDK.

I plan to add other authentication options, including OAuth, later.&#x20;

### Configuration

Configuration can come from Java code, a properties file or environment variables.

For example:

```java
DrapiConfig config = DrapiConfig.builder()
                                .baseUrl("https://my.drapi.server:8880")
                                .basic("Doctor Notes", "password")
                                .connectTimeout(Duration.ofSeconds(10))
                                .authScope("$DATA")
                                .build();
```

For applications where you don't want configuration in the code, you can load it from environment variables or a properties file.

For example, to load configuration from environment variables. starting with a prefix:

```java
DrapiConfig config = DrapiConfig.builder()
                                .applyEnvironmentVariables("DRAPI_")
                                .build();
```

You can also load it from a resource file:

```java
DrapiConfig config = DrapiConfig.builder()
                                .applyResourceFile("drapi.properties")
                                .build();
```

Or from a different file:

```java
DrapiConfig config = DrapiConfig.builder()
                                .applyFile("/Users/dalek/invasion-plans.properties")
                                .build();
```

Yes, apparently Daleks use properties files too.

### HTTP Transport

Underneath the SDK, HTTP requests are handled using the Java `HttpClient` API.

Normally you shouldn't need to care much about this part. However, the transport layer is abstracted behind an `HttpTransport` interface, so it is possible to provide another implementation if an application has different requirements.

The default transport uses asynchronous HTTP requests.

This led me to another design question.

### Asynchronous or Synchronous?

I spent some time deciding how much of the public API should be asynchronous.

There is an ongoing discussion in the Java world about how ["virtual threads" from Project Loom affect asynchronous programming](https://www.infoq.com/articles/java-virtual-threads/).

`CompletableFuture` is useful, particularly when an application is making several concurrent requests. But let's be honest, it isn't always fun to work with.

Virtual threads make non-blocking code much cheaper and let us write code in the familiar synchronous style without dedicating a traditional platform thread to every blocked operation. The catch for an SDK is compatibility: virtual threads require Java 21, and I don't want to require Java 21 at this stage.

For now, I'm going with both styles.

The underlying API is asynchronous, returning `CompletableFuture` objects, while synchronous alternatives can wrap those calls for developers who don't need or want asynchronous code.

In fact, authentication uses synchronous calls. This keeps the implementation simpler, and there isn't much benefit to making these calls non-blocking.

I'm planning to follow the same pattern across the SDK.

### JSON Serialisation and Deserialisation

Then there is JSON.

Picking a JSON library for a Java SDK is surprisingly awkward because different platforms already have their favourites. A Spring application may already use Jackson, while a Jakarta EE servers come with Jakarta JSON.

I didn't want the SDK API to depend too heavily on one of them.

So there is a JSON abstraction inside the SDK, with Jakarta JSON-B and JSON-P as the default implementation. The downside is that testing becomes a bit more complicated, and I can't use JSON annotations in my POJOs.

In the future, other implementations could be added for Jackson, Gson or something else without changing the rest of the SDK.

Whether I've found the right level of abstraction here is one of the areas where feedback would be useful.

## API Surface

Another question was how closely the Java API should resemble traditional Domino programming.

It would have been easy to build everything around familiar Domino concepts such as databases, views and documents. Instead, I've tried to stay reasonably close to the terminology and structure of Domino REST API itself.

That should make it easier to understand what the SDK is doing underneath, and also avoid inventing another completely separate abstraction on top of DRAPI.

The main entry point is `DrapiClient`:

```java
DrapiClient client = DrapiClient.builder(config).build();
```

DRAPI doesn't really work with "databases" in the traditional Domino API sense. Instead, we have data sources, also known as scopes.

A data source gives us access to a schema and the different APIs available for that data.

For example, let's use a data source called `contacts`:

```java
DrapiDataSource contacts = client.dataSource("contacts");
```

From there, operations are divided into smaller APIs such as Lists, Documents and Query.

To get to the Lists API:

```java
ListsApi lists = contacts.lists();
```

Now we can retrieve entries from a list:

```java
lists.get("AllContacts", ListsGetOptions.create().count(20).meta(false))
        .thenAccept(this::consumeListStream)
        .exceptionally(this::handleError)
        .join();
```

This is the asynchronous version. The call returns a `CompletableFuture`, so the application can process the result without blocking while the request is running.&#x20;

For operations that aren't tied to a particular data source, the API will be available directly from `DrapiClient`.

For example, a future Server API might look something like this:

```java
client.server() // Not implemented yet...
        .getInfo()
        .thenAccept(this::consumeServerInfo)
        .exceptionally(this::handleError)
        .join();
```

That one isn't implemented yet, hence the comment. But it shows the direction I'm taking with the API. The API semantics I'm planning to implement are described [here](https://github.com/sbasegmez/openntf-drapi-java-sdk/blob/main/docs/api-semantics.md).

The API surface is probably the part I'm least interested in getting wrong at this stage. Adding another endpoint later is relatively easy. Changing the basic way applications interact with the SDK after people start using it is considerably less fun.

So comments on this part are especially welcome.

## Data Surface

I also wanted to avoid handing raw JSON back to the application and making every developer parse the same responses again.

The SDK therefore has data objects corresponding to DRAPI/OpenAPI structures such as `Document` and `ListEntry`.

At the same time, Domino data doesn't always fit neatly into strongly typed Java objects. Fields and columns can vary between applications, so these objects need to stay fairly flexible.

For that reason, I've been experimenting with a fluent API for accessing values.

For example:

```java
document.field("firstName")
        .asString()
        .ifPresent(firstName -> System.out.println("First Name: " + firstName));
```

`field()` finds the field and `asString()` attempts to read its value as a string, returning an `Optional<String>`.

So a missing field doesn't immediately turn into another encounter with `NullPointerException`.

Another decision point is how strictly values should be mapped. The mappings are currently strongly typed, so there is no implicit conversion. If the incoming value is `"123"`, for example, `asInteger()` will not return an integer.

The assumption is that the data schema defines a contract, and that developers should not have to deal with unexpected conversions or exceptions. That said, I'm also considering a lenient mode that would give applications some room to handle values that don't match the expected type exactly.

There are also methods such as `exists()` and `isMultiValue()`, and values can be retrieved as lists:

```java
document.field("someField").asList(String.class);
```

`ListEntry` uses the same idea for columns:

```java
listEntry.column("colName");
```

There are more examples in the [sample code](https://github.com/sbasegmez/openntf-drapi-java-sdk/blob/main/drapi-sdk-samples/README.md) in the GitHub repository.

## What's next?

There is still plenty missing.

I'll continue adding parts of the Domino REST API, and authentication needs more work. Documentation and samples also need attention.

But before filling in all the endpoints, I'd like to get some feedback on the foundation: configuration, the synchronous/asynchronous API, JSON abstraction, resource hierarchy and the way data values are represented.

You can find the project on [GitHub](https://github.com/sbasegmez/openntf-drapi-java-sdk).

I've also opened [a discussion thread on OpenNTF Discord](https://discord.com/channels/953760981241200721/1541566228555112550) for questions, ideas and, quite possibly, arguments about API design.

Contributions don't have to be code either. Documentation, examples, use cases and feature requests would all be useful at this stage.

Let's see where it goes.

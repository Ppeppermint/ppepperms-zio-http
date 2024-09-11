[//]: # (This file was autogenerated using `zio-sbt-website` plugin via `sbt generateReadme` command.)
[//]: # (So please do not edit it manually. Instead, change "docs/index.md" file or sbt setting keys)
[//]: # (e.g. "readmeDocumentation" and "readmeSupport".)

# ZIO Http

ZIO HTTP is a scala library for building http apps. It is powered by ZIO and [Netty](https://netty.io/) and aims at being the defacto solution for writing, highly scalable and performant web applications using idiomatic Scala.

ZIO HTTP is designed in terms of **HTTP as function**, where both server and client are a function from a request to a response, with a focus on type safety, composability, and testability.

[![Development](https://img.shields.io/badge/Project%20Stage-Development-green.svg)](https://github.com/zio/zio/wiki/Project-Stages) ![CI Badge](https://github.com/zio/zio-http/workflows/Continuous%20Integration/badge.svg) [![Sonatype Releases](https://img.shields.io/nexus/r/https/oss.sonatype.org/dev.zio/zio-http_2.13.svg?label=Sonatype%20Release)](https://oss.sonatype.org/content/repositories/releases/dev/zio/zio-http_2.13/) [![Sonatype Snapshots](https://img.shields.io/nexus/s/https/oss.sonatype.org/dev.zio/zio-http_2.13.svg?label=Sonatype%20Snapshot)](https://oss.sonatype.org/content/repositories/snapshots/dev/zio/zio-http_2.13/) [![javadoc](https://javadoc.io/badge2/dev.zio/zio-http-docs_2.13/javadoc.svg)](https://javadoc.io/doc/dev.zio/zio-http-docs_2.13) [![ZIO Http](https://img.shields.io/github/stars/zio/zio-http?style=social)](https://github.com/zio/zio-http)

Some of the key features of ZIO HTTP are:

**ZIO Native**: ZIO HTTP is built atop ZIO, a type-safe, composable, and asynchronous effect system for Scala. It inherits all the benefits of ZIO, including testability, composability, and type safety.

**Cloud-Native**: ZIO HTTP is designed for cloud-native environments and supports building highly scalable and performant web applications. Built atop ZIO, it features built-in support for concurrency, parallelism, resource management, error handling, structured logging, configuration management, and metrics instrumentation.

**Imperative and Declarative Endpoints**: ZIO HTTP provides a declarative API for defining HTTP endpoints besides the imperative API. With imperative endpoints, both the shape of the endpoint and the logic are defined together, while with declarative endpoints, the description of the endpoint is separated from its logic. Developers can choose the style that best fit their needs.

**Type-Driven API Design**: Beside the fact that ZIO HTTP supports declarative endpoint descriptions, it also provides a type-driven API design that leverages Scala's type system to ensure correctness and safety at compile time. So the implementation of the endpoint is type-checked against the description of the endpoint.

**Middleware Support**: ZIO HTTP offers middleware support for incorporating cross-cutting concerns such as logging, metrics, authentication, and more into your services.

**Error Handling**: Built-in support exists for handling errors at the HTTP layer, distinguishing between handled and unhandled errors.

**WebSockets**: Built-in support for WebSockets allows for the creation of real-time applications using ZIO HTTP.

**Testkit**: ZIO HTTP provides first-class testing utilities that facilitate test writing without requiring a live server instance.

**Interoperability**: Interoperability with existing Scala/Java libraries is provided, enabling seamless integration with functionality from the Scala/Java ecosystem through the importation of blocking and non-blocking operations.

**JSON and Binary Codecs**: Built-in support for ZIO Schema enables encoding and decoding of request/response bodies, supporting various data types including JSON, Protobuf, Avro, and Thrift.

**Template System**: A built-in DSL facilitates writing HTML templates using Scala code.

**OpenAPI Support**: Built-in support is available for generating OpenAPI documentation for HTTP applications, and conversely, for generating HTTP endpoints from OpenAPI documentation.

**ZIO HTTP CLI**: Command-line applications can be built to interact with HTTP APIs by leveraging the power of [ZIO CLI](https://zio.dev/zio-cli) and ZIO HTTP.

## Installation

Setup via `build.sbt`:

```scala
libraryDependencies += "dev.zio" %% "zio-http" % "3.0.0"
```

**NOTES ON VERSIONING:**

- Older library versions `1.x` or `2.x` with organization `io.d11` of ZIO HTTP are derived from Dream11, the organization that donated ZIO HTTP to the ZIO organization in 2022.
- Newer library versions, starting in 2023 and resulting from the [ZIO organization](https://dev.zio) started with `0.0.x`, reaching `1.0.0` release candidates in April of 2023

## Getting Started

ZIO HTTP provides a simple and expressive API for building HTTP applications. It supports both server and client-side APIs. Let's see how it is simple to build a greeting server and call it using the client API.

### Greeting Server

The following example demonstrates how to build a simple greeting server. It contains 2 routes: one on the root
path, it responds with a fixed string, and one route on the path `/greet` that responds with a greeting message
based on the query parameter `name`.

```scala
import zio._
import zio.http._

object GreetingServer extends ZIOAppDefault {
  val routes =
    Routes(
      Method.GET / Root -> handler(Response.text("Greetings at your service")),
      Method.GET / "greet" -> handler { (req: Request) =>
        val name = req.queryParamToOrElse("name", "World")
        Response.text(s"Hello $name!")
      }
    )

  def run = Server.serve(routes).provide(Server.default)
}
```

### Greeting Client

The following example demonstrates how to call the greeting server using the ZIO HTTP client:

```scala
import zio._
import zio.http._

object GreetingClient extends ZIOAppDefault {

  val app =
    for {
      client   <- ZIO.serviceWith[Client](_.host("localhost").port(8080))
      request  =  Request.get("greet").addQueryParam("name", "John")
      response <- client.batched(request)
      _        <- response.body.asString.debug("Response")
    } yield ()

  def run = app.provide(Client.default)
}
```

## Documentation

Learn more on the [ZIO Http homepage](https://zio.dev/zio-http)!

## Contributing

For the general guidelines, see ZIO [contributor's guide](https://zio.dev/contributor-guidelines).

## Code of Conduct

See the [Code of Conduct](https://zio.dev/code-of-conduct)

## Support

Come chat with us on [![Badge-Discord]][Link-Discord].

[Badge-Discord]: https://img.shields.io/discord/629491597070827530?logo=discord "chat on discord"
[Link-Discord]: https://discord.gg/2ccFBr4 "Discord"

## License

[License](LICENSE)

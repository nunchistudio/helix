---
location: "/documentation/index.md"
title: "What is helix?"
---

# {% $markdoc.frontmatter.title %}

helix is a framework for building cloud-native, consistent, reliable, and
high-performance (micro) services. It allows back-end engineers to simplify
development of complex problems through a thin layer of abstraction that handles
automatic logging, tracing, observability, and event propagation across services
and integrations.

helix acts as a package you can import in your code. Implementations:
- [Go](https://github.com/nunchistudio/helix.go): for building micro-services
  with all the benefits listed above.
- [TypeScript](https://github.com/nunchistudio/helix.ts): for consuming public
  types on the front-end exposed by micro-services.

## Use cases

helix was developed with the following use cases in mind:

- Engineers must be able to write and maintain cloud-native, consistent, reliable,
  and high-performance (micro) services with no hassle.
- Engineers must have the power to transparently leverage end-to-end observability
  and event propagation across services and integrations.
- Engineering and product teams must be able to understand the user journey, fix
  customer issues, and improve user/platform/product experience together via
  end-to-end observability.
- Engineers must focus on the business logic. Technical challenges such as
  consistency and observability must live outside the business logic of the
  services.

![Event propagation with helix](https://nunchi.studio/helix/screenshots/trace-distributed.png)

## Features and benefits

helix provides an opinionated way to develop (micro) services. At its core, it
relies on specifications and concepts such as OpenTelemetry and event propagation
across services and integrations.

- **Automatic distributed tracing and error recording:** All integrations implement
  distributed tracing and error recording automatically, following OpenTelemetry
  standards. Which means developers can leverage integrations' capabilities as
  they are used to, except that all those functions transparently leverage traces
  and error recording with no additional efforts on the application side.
- **Consistent event propagation:** At its core, helix exposes an "*Event*" object.
  This object is automatically passed from service to service via distributed
  tracing, allowing to have observability of an event from end-to-end with no
  effort.
- **Consistent error handling:** Error handling and recording share the same
  schemas and behaviours across helix core and all integrations for strong
  consistency in the whole system.
- **OpenAPI & AsyncAPI support:** When it makes sense, OpenAPI and AsyncAPI
  specifications can optionally be leveraged for data validation. This can happen
  in the REST router for request/response validation against an OpenAPI description,
  or in other integrations for message validation against an AsyncAPI description.

## Goals

To understand why helix is designed this way, it's important to understand the
following goals in mind:

- The solution must be deployable anywhere and everywhere.
- The solution must be easy to configure and operate.
- The solution could exist in any and every language.
- The solution must allow smooth onboarding and no lock-in.
- The solution must provide a transparent layer of abstraction for end-to-end
  observability.
- The solution must be as light as possible, while still supporting out of the
  box capabilities via integrations.
  - Integrations capabilities must not overlap with one another.
  - Integrations must be as consistent as possible across languages.
  - Integrations internal lifecycle must not be exposed to end-users.
  - Configuration across integrations must be as easy and consistent as possible.
  - Custom integrations must be possible in case built-in ones don't fit the
    requirements of a company.

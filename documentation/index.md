---
location: "/documentation/index.md"
title: "What is helix?"
---

# {% $markdoc.frontmatter.title %}

helix is how I build and maintain reliable, cloud-native, high-performance
microservices. After years of working with different organizations and various
technical stacks, I've decided to put my knowledge and experiences into helix,
so I can deliver a consistent and high quality of work. I hope it makes your
development a little bit easier!

## Goals

helix was developed with the following goals in mind:

- The solution must be deployable anywhere and everywhere.
- The solution must be easy to configure and operate.
- The solution could exist in any and every language.
- The solution must allow smooth onboarding and no lock-in.
- The solution must provide a transparent layer of abstraction for end-to-end
  observability.
- The solution must be as light as possible, while still supporting out of the
  box capabilities via integrations.
  - Integrations capabilities must not overlap with one another.
  - Integrations must be implemented in all languages supported. 
  - Integrations must be as consistent as possible across languages.
  - Integrations internal lifecycle must not be exposed to end-users.
  - Configuration across integrations must be as easy and consistent as possible.
  - Custom integrations must be possible in case built-in ones don't fit the
    requirements of a company.

## Features and benefits

helix provides an opinionated way to develop microservices. At its core, it relies
on specifications and concepts such as OpenTelemetry and event propagation across
services and integrations.

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

By using helix, organizations can benefit from automatic distributed tracing,
error recording, and event propagation across their stack with no additional
lines of code!

![Event propagation with helix](/helix/screenshots/trace-distributed.png)

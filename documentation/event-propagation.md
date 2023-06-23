---
location: "/documentation/event-propagation.md"
title: "Event propagation"
---

# {% $markdoc.frontmatter.title %}

helix comes with a built-in `Event` object. It provides useful context about an
event and brings consistency across integrations and services when propagating
events information.

helix automatically propagates an `Event` across traces/spans and services, with
no additional development required on the application.

{% callout level="warning" iconType="securitySignalDetected" title="Security warning" %}
  `Event` should be used for data that you’re okay with potentially exposing to
  anyone who inspects your network traffic. This is because it’s stored in HTTP
  headers for distributed tracing. If your relevant network traffic is entirely
  within your own network, then this caveat may not apply.
{% /callout %}

{% tab name="Go" %}
  {% partial file="helix-go/event-propagation.md" /%} 
{% /tab %}

By having a consistent event propagation across all services and integrations, an
organization can benefit end-to-end observability much more easily:

![Your second trace with helix](/helix/screenshots/trace-distributed.png)

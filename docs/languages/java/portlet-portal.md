---
tags: [java, portal, portlet, JSR186, JSR286]
---
# PORTLET - PORTAL

*Last update: 7 Oct 2022*

### Versions

* Portlet 1.0 (JSR 168) does not provide a native solution to let portlets communicate with each other. One alternative is to use shared data in the web session.

* Portlet 2.0 (JSR 286) defines events and public render parameters

### Events

* Events are defined in the `portlet.xml` file, in the `event-definition` section
* Every portlet defines:
    - the emitted (generated) events (supported-publishing-event)
    - the listened events (supported-processing-event)
* The events are generated with the `setEvent` method
* The `EventPortlet.processEvent` method handles the listened events
* The event names are `QNames`

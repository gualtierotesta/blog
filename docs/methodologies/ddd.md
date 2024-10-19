---
tags: [software, methodologies, ddd, domain driven design]
---
# Domain Driven Design (DDD)

*Last update: 19 Oct 2024*

Domain-Driven Design (DDD) is a software development approach that emphasizes modeling a software system around the business domain it serves.

It focuses on understanding the core business concepts and processes, and then designing the software to reflect those concepts accurately.

Key principles of DDD include:

* `Ubiquitous Language`: A shared vocabulary used by developers, business analysts, and domain experts to communicate about the business domain.
* `Bounded Contexts`: Defining boundaries around different parts of the system that have distinct responsibilities and can be modeled independently.
* `Entities`: Entities represent the fundamental building blocks of your business domain. They have a unique identity that persists over time, even if their attributes change. This means that an entity remains the same object, even if its properties are updated. Entities have a state and a behavior. Their life cycle (creation, update, deletion) is managed by the application.
* `Aggregates`: Grouping related entities together into a cohesive unit that can be treated as a single object.
* `Value Objects`: Objects that represent values rather than entities, such as money or color.
* `Domain Events`: Capturing significant occurrences within the domain that can be used to trigger actions or updates in other parts of the system.

By following these principles, DDD aims to create software that is more aligned with the business, easier to understand, and more maintainable.

## Books and references

* "Domain-Driven Design: Tackling Complexity in the Heart of Software Systems" (Eric Evans)
* "Implementing Domain-Driven Design" (Vaughn Vernon)
* "Domain-Driven Design Distilled" (Vaughn Vernon)

## Internal structure

The suggested internal structure for a DDD project is the following:

### Core Domain Layer

* Entities: These represent the fundamental building blocks of your business domain. They should be immutable and encapsulate the domain logic.
* Value Objects: Represent values rather than entities. They are often immutable and should be compared by value, not reference.
* Aggregates: Group related entities together into a cohesive unit. They have a root entity that is responsible for ensuring the data consistency of the aggregate.
* Domain Services: Handle complex business rules that don't fit naturally within an entity or value object.
* Domain Events: Capture significant occurrences within the domain that can be used to trigger actions or updates in other parts of the system.

### Application Layer

* Application Services: Facilitate interactions between the user interface or other external systems and the domain layer. They orchestrate the flow of data and business logic.

### Infrastructure Layer

* Persistence: Handles the storage and retrieval of domain objects. In a Java project, this can be implemented using frameworks like JPA or Hibernate.
* External Services: Interfaces with external systems or APIs.
* Utilities: Provides reusable utility classes for common tasks.

### User/API Interface Layer

* Presentation Layer (optional): Handles the user interface, such as web or mobile applications.
* API layer (optional): APIs and web-services exposed by the application.

An example of a Java application package structure:

```text
    src/main/java/com/yourcompany/
        domain/
            entities/
            valueobjects/
            aggregates/
            services/
            events/
        application/
            services/
        infrastructure/
            persistence/
            externalservices/
            utils/
        ui/
            web/
            mobile/
        api/
```

---
tags: [software, methodologies, model, c4]
---
# C4 Model

*Last update: 22 Nov 2024*

## Links

* [c4model.com](https://c4model.com/)
* [Visualizing software architecture with the C4 Model (Video)](https://www.youtube.com/watch?v=x2-rSnhpw0g) 

## Introduction

The C4 model has been created by S. Brown in 2006. It is a graphical and hierarchical representation of a software system. AT the lower levels, the C4 model use UML diagrams.

The C4 in the name means Context, Containers, Components and Code.

The upper level (1) is the `Context` view that shows how the software system interacts with the users and the external systems.

Below, there is the level 2, the `Container` diagram. Here, container means an application, a data store, an API. It is not related to the Docker containers.

A container can be a relational database, a Spring Boot application.

Below, there is the level 3, the `Component` diagram. The components are the parts of a container. For example, the main web controllers in a Spring Boot application.

The lower level is level 4, the `Code`, where UML class and ER diagrams can describe the elements inside a component.

S. Brown does not suggest drawing level 4 diagrams. IDEs can do it from the code. See the video in the links.


The C4 model can be considered as the static structure for other models and diagrams:

* UML deployment diagrams (to connect C4 containers with the infrastructure)
* Infrastructure diagrams (physical, virtual, hardware, firewalls, routers, load balancers...)
* UML Entity relationship diagrams (ERD) for data
* UML sequence and collaboration diagrams to describe runtime and behavior
* Business process and workflow

## Connections

S. Brown suggest:

* using one direction only lines (with only one arrow head) to make more evident the relations between the two connected elements;
* describing with a short phrase the intent of the connection. Ex. "Make API calls using [JSON/HTTPS]" (instead of "uses")

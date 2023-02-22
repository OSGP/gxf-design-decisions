# Business case GXF Upgrade
We want to strengthen GXF's position as an opensource IOT device management and data collection platform.
This will require us to rethink the way GXF is developed and designed.

This document will focus on the new way we want to work and how it differs from what we are used to.
We won't go into specific tooling or frameworks until we are sure we want to go ahead with the project.



## Core principals of new design
One of the most important changes to the design is the independent building and releasing of functional modules.
The current design has large components that are hard to test and configure with a large set of responsibilities.


### Functional services
The new design works with smaller, functional modules.
The modules should have clear responsibilities and work independently of other modules.
A clear description will be provided of the incoming and outgoing data.

Each module gets its own repo with a clear description of what it does.
The different versions of the modules will be aggregated in a deployment project, this project should not have code only
environment specific variables.
It will be up to the different implementors to choose which modules they need for their project.

We will still have shared libraries, for example for authentication. But these libraries should not have product
functionality.


### Scalable
GXF is intended for large amounts of IOT devices. We need to ensure that all the components perform and are scalable.
Using async communication ensures that services keep working when another service is down for maintenance.

The services need to be loosely coupled. A change in one should not need to result in an immediate change in another service.
Data transfer objects (DTO) can be published by a service in the form of avro schema's so other services can create their own
implementation in a language agnostic way.
Database objects should never be shared between services, use DTO's!! If the same data is needed in another service the data should
be published by the service responsible for this data and consumed by the other service(s).

It might be needed in the future to create a new iteration of a service that branches off into its own repository.
For instance during the pilot phase of a project we create a new service and later create another service with the production code.
Both should be able to run until the pilot devices are phased out.


### Keep it simple, stupid!
It is important that the new code uses easy to read descriptions and code.
This can mean using less optimal code if we can keep the code simpler.

Composition over inheritance, The current setup has a lot of inherited classes and code making it hard to understand what a classes responsibilities are.
Making simple services that have clear input and output will keep the code easier to maintain.


### Standard tooling
We want to focus on implementing logic and not implementing infrastructure code.
This means using standard database connection, metrics endpoints, and authentication logic.
Possible frameworks we can use are: Spring boot, Quarkus, Micronaut. If we want to go forward with the redesign we have
to investigate which framework to use.

This will also make it easier to onboard developers and get open source contribution. 
Also, security updates will be handled by the framework maintainers which saves us time. 
This requires us to prefer the libs included in the framework over 3rd (4th?) party libraries.


### Easy to run
The services should be easy to run locally and on servers.
This includes proper monitoring tools which indicate the state of the service.

The image build files will be stored with the project as well as the kubernetes configuration.


### Easy to test
Testing the code locally should be easy and possible on laptop hardware in a production like manner.
If local tests require other systems they will need to be mocked so the service can be tested independently.

The tests should be identical to the tests than run on the CI server.


### Quick to build
The modules should stay lean. This will allow for rapid development cycles.


### Continuous delivery
The services will be automatically built, tested and deployed to acceptance but final production deployment will stay manual.
This will ensure that we are in control of the release processes.


## Transition

The plan is to transition to a new design without stopping development on the current application.
Because we are choosing to go to a modular application we can create new services for new functionality 
without having to make big changes to the current application.

The team will decide when functionality makes sense to develop in the new style or create a small fix in the old style.

When a part of the gxf functionality is extracted the data needed from the other components should be posted to Kafka.
We use Kafka for this because the platform already supports it and the new components don't need to support REST or ActiveMQ.



## Proof of concept
After we have choosing which language, framework and tooling we want to use we will start with a proof of concept.
This can be new functionality or extracting a small piece of functionality from the gxf platform.


The new application should help us see what tooling is missing and if the learning curve is bigger than anticipated.


## Return on Investment

### Cost
The main cost will be choosing the new tooling and making an initial application template. After we have the template we
can develop new code in a new way and start reaping the rewards.

If the new approach is too our liking we can start looking at the current components and estimate how much it will cost to refactor those.
The main cost of refactoring the existing components is replacing infrastructure code. Logic can be copied to the new services.

| Item                                    | Hours   |
|-----------------------------------------|---------|
| Framework Selection                     | 20      |
| Template Development                    | 10      |
| New framework configuration             | 20      |
| Alliander specific library development  | 40      |
| Implement bridging mechanic in old code | 20      |
| **Sum**                                 | **110** |

After the 110 hours of development we should have a solid base after which there is no big additional 
cost to developing new functionality compared to developing in the current setup.


### Return
Local development will be faster with lower build times and easier to run tests.
Time to production will be shorter with faster CI builds and deployment pipelines.

| Item             | Hours | Times per month | Developers | Total Hours |
|------------------|-------|-----------------|------------|-------------|
| Local build time | 0,10  | 60              | 5          | 30          |
| CI Build Time    | 1,5   | 10              | 5          | 150         |
| Deployment time  | 0,5   | 4               | 5          | 10          |

#### Intangible Benefits

- More focus on business logic
- Easier onboarding of developers
- Easier opensource contribution
- Developer happiness
- More secure software

### Summary

By gradually migrating the code we should be able to recover costs per module fairly easily.
The faster development and ease of use should allow us to recover the initial costs within a year.



## Risks

- Choosing the wrong tooling
- Performance issues
- Reduced productivity
- Falling back to old habits
- Unnecessary complexity



## Followup actions

- Select new tooling
    - Language (Java, Kotlin)
    - Framework (Spring boot, Quarkus, Micronaut)
    - Build Tool (GitHub actions, Jenkins, Tekton)
    - Deployment Tool (Argocd, Argo Pipeline, Tekton, Jenkins)
- Define target architecture
- Create transition plan
- Define release strategy



## Stakeholders

| Stakeholder    | Role                   | Informed level |
|----------------|------------------------|----------------|
| Maarten Mulder | Product Owner          |                |
| Robert Tusveld | Solution Architect     |                |
| GXF Developers | Developer              |                |
| LF Energy      | Open source Foundation |                |
| Alliander      | Supplier               |                |

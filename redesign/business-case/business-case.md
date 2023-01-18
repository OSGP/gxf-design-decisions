# Business case redesign GXF
We want to strengthen GXF's position as an opensource IOT device management and data collection platform.
This will require us to rethink the way GXF is developed and designed.

Here, we will describe the new ways we want to work and the reasoning behind it.

---
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
Data transfer objects can be published by a service in the form of avro schema's so other services can create their own implementation in a language agnostic way.
Database objects should never be shared between services!! If the same data is needed in another service the data should be published and consumed by the other service.


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


---
## Transition
The plan is to transition to a new design without stopping development on the current application.
This means implementing new functionality in the new style and extracting logic from the current application when needed. 

The team will decide when functionality makes sense to develop in the new style or create a small fix in the old style.

TODO How de we handle communication between old and new setup (Kafka, REST, SOAP, ActiveMQ)

---
## Return on Investment

### Cost
The main cost will be choosing the new tooling and making an initial application template.
By extracting more and more functionality from the current components we don't have to invest a lot into recreating
components.
If in the end we remain with large chunks of functionality, we need to make a more in depth analysis on redeveloping
those components.

| Item                                    | Hours   |
|-----------------------------------------|---------|
| Framework Selection                     | 20      |
| Template Development                    | 10      |
| New framework configuration             | 20      |
| Alliander specific library development  | 40      |
| Implement bridging mechanic in old code | 20      |
| **Sum**                                 | **120** |

### Return
Local development will be faster with lower build times and easier to run tests.
Time to production will be shorter with faster CI builds and deployment pipelines.

| Item             | Hours | per month | Total Hours |
|------------------|-------|-----------|-------------|
| Local build time | 0,25  | 60        | 15          |
| CI Build Time    | 1,5   | 10        | 30          |
| Deployment time  | 0,5   | 4         | 2           |

### Summary
The faster development and ease of use should allow us to recover the initial costs within a year.

---
### Intangible Benefits

- More focus on business logic
- Easier onboarding of developers
- Easier opensource contribution
- Developer happiness

---
## Risks

- Choosing the wrong tooling
- Performance issues
- Reduced productivity
- Falling back to old habits

---
## Followup actions

- Select new tooling
    - Language (Java, Kotlin)
    - Framework (Spring boot, Quarkus, Micronaut)
    - Build Tool (GitHub actions, Jenkins, Tekton)
    - Deployment Tool (Argocd, Argo Pipeline, Tekton, Jenkins)
- Define target architecture
- Create transition plan
- Define release strategy

---
## Stakeholders

| Stakeholder    | Role                   | Informed level |
|----------------|------------------------|----------------|
| Maarten Mulder | Product Owner          |                |
| Robert Tusveld | Solution Architect     |                |
| GXF Developers | Developer              |                |
| LF Energy      | Open source Foundation |                |
| Alliander      | Supplier               |                |

# Business case redesign GXF
We want to strengthen GXF's position as an opensource IOT device management and data collection platform.
This will require us to rethink the way GXF is developed and designed. 

Here, we will describe the new ways we want to work and the reasoning behind it. 

---
## Core principals of new design
One of the most important changes to the design is the independent building and releasing of functional modules.
The current design has large components that are hard to test and configure with a large set of responsibilties.

### Functional modules / services
The new design works with smaller, functional modules.
The modules should have clear responsibilities and work independently of other modules.
A clear description will be provided of the incoming and outgoing data.

Each module gets its own repo with a clear description of what it does.
The different versions of the modules will be aggregated in a deployment project this module should not have code only environment specific variables. 
It will be up to the different implementors to choose which modules they need for their project.

We will still have shared libraries, for example for authentication. But these libraries should not have product functionality.


### Standard tooling
We want to focus on implementing logic and not implementing infrastructure code.
This means using standard database connection, metrics endpoints, and authentication logic.

### Easy to run
The services should be easy to run locally and on servers.
The image build files will be stored with the project as well as the kubernetes configuration.

### Easy to test
Testing the code locally should be easy and possible on laptop hardware in a production like manner.
The tests should be identical to the tests than run on the CI server.


### Quick to build
The modules should stay lean. This will allow for rapid development cycles.

### Continuous delivery
The services will be automatically built, tested and deployed to acceptance but final production deployment will stay manual.
This will ensure that we are in control of the release processes.

---
## Transition
Gradual transition.
The current GXF iteration should keep working while we transition to new modules.

---
## Return on Investment
The faster development and ease of use should allow us to recover cost within a year.

--- 
## Benefits

### More focus on business logic

### Faster Development cycles

### Easier onboarding of developers

### Easier opensource contribution


### Developer happiness

---
## Risks

- Choosing the wrong tooling
- Performance issues
- Reduced productivity

---
## Followup actions

- Select new tooling
  - Language
  - Framework
  - Build Tool
  - CI Tool
  - Deployment Tool
- Define target architecture
- Create transition plan

---
## Stakeholders

| Stakeholder    | Role                   | Informed level |
|----------------|------------------------|----------------|
| Maarten Mulder | Product Owner          |                |
| Robert Tusveld | Solution Architect     |                |
| GXF Developer  | Developer              |                |
| LF Energy      | Open source Foundation |                |
| Alliander      | Supplier               |                |

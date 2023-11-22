# API Gateway (Currently Studying)
- Centralize all the endpoints in one API Gateway
- Unique entry point of the whole architecture (to a system or a group of systems)
- It has to be Resilient/scalable. Because it is a unique point of failure.
- Good for security (A firewall commonly acts in it

- Some functions:
  * rate limiting
  * authentication / authorization
  * log control
  * routing
  * metrics
  * tracing

- Types:
  * Enterprise Gateway:
     - APIs that are for business areas.
     - It allows you to control the life cycle of the development (develop, test, production, deprecated, etc).
     - There is a portal interface that controls it.
     - Take care with these gateways vendor "lock-in" (when you get dependent on this vendor).
     - Some of them have databases and caches (but take care, because it increases the risk of unavailability).
     - Usually it stays out of your infrastructure.
  * Micro/Microservice Gateway:
     - Route the traffic to the micro services.
     - Does not offer support for APIs life cycle and the team has to create a separate process to do it (like a pipeline).
     - Usually they do not have external dependency, so the k8s manage the state of the application.
     - Purpose: exposure, observability and monitoring.
     - Maintain: Declarative configuration for updates, done by the services teams
     - Support for environments. Support for canary deployment (debugging).
     - Environment: One API Gateway Instance for each environment.
     - Management: through pipelines.

- OBS: Inside your network, when you have a lot of microservices, avoid the "death star", and put an API Gateway to each "context". This way things get more organized.
- OBS: Always automate your API Gateway configuration! Through pipelines, as an example.

- Advantages:
   * Logging, security, unique entry point, metrics
 
- Disadvantages:
   * complexity, needs extra care (availability), needs to maintain/update (to avoid vulnerability), unique point of failure

- When do we choose each API Gateway?
  * When we have a team focuses in manage the API Gateway: Enterprise Gateway
  * When each team has its own capacity to manage their endpoints in API Gateway: Micro/Microservice Gateway

- Example:
  * Kong API Gateway:
    - A Open Source API Gateway, for free. (there is an enterprise version, too, that is paid)
    - Micro API Gateway (it does not manage the life cycle of your API)
    - Flexible Deployment (can be internally or in a edge)
    - Ready for Kubernetes
    - There are plugins for it
    - Deployment models: you can work with declarative YML/Json or with Database
    - Konga: It is a visual interface to manage everything (show metrics, control users, etc.)

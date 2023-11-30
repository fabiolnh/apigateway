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
      * OBS:
        - Downstream: User or Service that is accessing the API Gateway
        - Proxy: Kong
        - Upstream: Backend (Services behind API Gateway)
      * Create Service, than create the route (ex: api.test.com -> api/test)
      * Plugins: Can be configured in Routes, Consumers or globally. Exemples:
        - Plugin Correlation ID: Good to have. It puts an ID to the Request and to the Response (header: Request-ID), passing this value to the services. ( https://docs.konghq.com/hub/kong-inc/correlation-id/ )
        - Plugin Rate Limiting: if you want to limit X requests per seconds/minutes. You can choose the limit by consumer, credential, ip, service, header or path). There are a lot of configurations, one example is to use redis to better consistency.
        - Plugin Response Transformer or Request Transformer: You can insert or remove json elements (body), headers, etc.  (Good to know: do not transform heavy operations to not overload the API Gateway)
        - Plugin Basic Auth: When you send in the header the authorization value in Base64 (Authorization: Basic ...). It is the initial concept of Authentication. The beginner level. Low Security. But it works if you do not have anything else. (if some attacker has the header value, he will know the password). Currently we have to use OAuth or OpenID Connect
           * You can work with Authentication in the Kong API Gateway (but remember, in some cases, having the base of users in the API Gateway is not good because you create a "lock-in"). It is good to have an Identity Provider in the Company to have a central controller of the users.
        - Plugin Key Authentication: For services authentication (not users). Those consumers (services) that are accessing the API Gateway. It is only a value on the header. (Ex: apiKey: ...). This authentication is not recommended, either. It is better to use OAuth or OpenID Connect
   
- Infrastructure Types / Deployment Models:
  * Location:
    1) Edge: External. Entry point for consumers.
    2) Between Contexts (to minimize the Death Star).
  * Data:
    1) DB-Less: Declarative YAML/JSON with the configuration (remember to replicate the file to all nodes). You have more availability here.
    2) With a Database: More consistency but less availability. (If the database goes down, a problem may occur.) However, there are mechanisms in Kong that keep some configurations cached for performance.
    * Note: Some plugins work only in the Database Model (Ex: OAuth2 Authentication). Therefore, it is important to verify at the beginning the Data Model you will choose.
  * Teams:
    1) A Team that is specific for the management of the whole API Gateway for all entries in squads. The Specialized API Gateway Team is responsible.
    2) A Squad that manages your own apps Api Gateway. The Squad is responsible. Important to have a Pipeline (Automation)

- Observability:
 - Metrics: Install the Prometheus Plugin (Globally. However, you can put in another granularity). In Grafana there is an official dashboard for it. You can see: Request Rate (Requests per second, by route, by service and by status code), Latencies, Bandwidth, Caching, Upstream and Nginx.
 - Logging: Install The TCP Logging Plugin (in Service). This way you send the logs to FluentBit (elastic stack). So, in Kibana you can see all the logs, such as: Index, latency time, Headers, Port, URL, IP origin, etc. This way you can do a lot of things, like: "How many times we received the header X", etc. OBS: There is no Body Logging. (could cause a performance issue, or a security issue, like sensible information)
 - 

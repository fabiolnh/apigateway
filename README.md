# API Gateway (Currently Studying)
- Centralize all the endpoints in one API Gateway
- Unique entry point of the whole architecture (to a system or a group of systems)
- It has to be Resilient/scalable. Because it is a unique point of failure.
- Good for security (A firewall commonly acts in it)
- Some functions:
  * rate limiting
  * authentication / authorization
  * log control
  * routing
  * metrics
  * tracing
- Types:
  * Enterprise Gateway: APIs that are for business areas. It allows you to control the life cycle of the development (develop, test, production, deprecated, etc). There is a portal interface that controls it. Take care with these gateways vendor "lock-in" (when you get dependent on this vendor). Some of them have databases and caches (but take care, because it increases the risk of unavailability). Usually it stays out of your infrastructure.
  * Micro/Microservice Gateway: 

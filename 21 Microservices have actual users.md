# Microservices have actual users?

## Author
@stilkov

## Definition
"Microservices" are building blocks of an architectural style that uses deployment boundaries as first-class software architecture principle.

## Assumptions
* Orchestration is cheap
  * false: huge difference between "almost local" and "remote"
  * interaction from a client-side app with many remote services is very costly
* back-end for front-end: channels matter
  * false: users expect a seamless experience across channels
  * everything should be accessible everywhere at any time
* services matter most
  * UIs matter more
* front-end technology don't matter
  * false: has crucial implications on architectural possibilities

## UIs and microservices
UIs of a specific domain part (bounded context) should be along with the microservice (back-end part)
* not all microservices require a UI

Parts of the business service are split up into multiple microservices.

## Microservices back-end platform goals
* as few assumptions as possible
* no implementation dependencies
* small interface surface
* based on standards
* parallel development
* parallel operations
  * independent deployments
  * autonomous operations

## Front-end types
* Web app
  * Rendered on client
  * Rendered on server
* Native app
  * many platforms
* Hybrid

Web:
* links
* redirections
* transclusion
* web components

## SOAP Web services have nothing to do with the Web
* use HTTP as transport
* ignore verbs
* ignore URIs
* expose a single endpoint
* fail to embrace the Web

## ROCA style
ROCA: Resource Oriented Client Architecture
Link: http://roca-style.org
# Efficient service API design

## APIs
Local APIs vs remote APIs: different stories.

A network service (almost always a server) by which programs communicate with a bundle of functionality provided by code owned by someone else, running on their computer (not yours).

## Interface
An interface provides an additional layer of indirection

## Differences between local library APIs and remote service APIs
* service APIs are unreliable
  * network failures, server crashes, vendor changes data formats, endpoints or simply shut them down
* service APIs are slow
* interactive applications need to invoke asynchronously

## Start with the documentation
* focus on the raw HTTP: URL paths, query strings and response formats
* begin with sample requests, responses and use cases
* not schemas!
* Java, C#, etc come later, if at all
* don't try to autogenerate an API by sprinkling existing code with a few annotations mapping methods to URLs

## What to put in an API
* minimum viable product: start small and grow
* consider read-only as a first step: much easier and covers a lot of valuable use cases
* YAGNI
  * much easier to add later than remove
  * much cheaper to add one neglected feature than ten unused features

## Design considerations
* single request, single response with no side effects or ordering
* aim for idempotency
* noun-oriented (resource oriented)
* standard verbs only: GET, PUT, DELETE, POST*
* custom verbs and non-standard POST usually aren't necessary

# Define your endpoints (URLs)
* be literal in what you use of URL paths, avoid a single endpoint that addresses by parameters
* each resourcehas a URL that independently identifies the resource
* the URL of a resource is fixed and never changes
* meaningful path components make development easier

## Define your data formats
* JSON, XML
* all text in UTF-8
* the storage format is not (necessarily) the exchange format
* date formats should rule things in, not rule them out
  * allow for future extensibility
* schemas are documentation, not part of your processing chain
* allow optional data

## Define your trust level
* what's your SLA?
* what's the deprecation schedule (e.g., 2 years)
* how much can developers rely on your API?
* plan for versioning (easier without schemas and with optional fields)
* can someone else implement your API? Are clients locked in?

## Client libraries
* installed local bundle of code (library) that interfaces to the remote API while hiding some details of response parsing and network transport
* useful, not required
* never assume your clients will only use your library to access the service
* cannot hide the fundamental unreliability of a network library
* hand-crafted is better than auto-generated

## Decouple the API from the client
The server should assume nothing about the client

## Authentication and Authorization
* authentication and authorization don't tell which resource you want
* resource ID belongs in the URL, not the auth

## Authentication schemes
* if you don't need 2FA, basic over HTTPS still works
* consider piggybacking on existing logins (Google, Facebook, Twitter, etc)
* three-legged oAuth, OpenID, JWT
* do not ask for more permissions than you need
* leverage existing flows in the browser, do not try to roll your own UX for this
* provide for service and robot accounts

## Developer keys
* not the same as authentication; identifies the app, not the user
* not really a solved problem, easily stealable
* helps identify developers with good intentions; does not stop malicious users
* another bump in the road for API adoption

## Performance
* not as important as on a web page since a program doesn't get bored
* few seconds is perfectly acceptable (unless used by a Web front-end)
* for tens of second, apply async patterns
* paginate with continuation tokens
* don't optimize around the transport protocol
  * work with HTTP, not against it
    * gzip instead of custom binary
    * keep-alive in HTTP/1, batch methods in SPDY/HTTP/2, conditional GET

## Rate limiting
* HTTP status code 429 (dedicated status code)
* strongly prefer by user if at all possible
* use developer key if you must, but remember this is almost trivially forgeable

## Specs and tools
* swagger and open APIs are worth a look if JSON serves you well
* easier/cleaner to design URL structure outside of Swagger and describe it post facto
  * focus on the use cases first
  * treat specs as documentation, not as design

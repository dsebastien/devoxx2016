# Authentication and authorization in a cloud native world

## SAMLv2

* intended for Web browser single sign-on
* developed since 2001 by OASIS
* widely supported
* complex XML-based protocol that builds upon other XML specs like XML sig and XML encryption
* no direct connection needed between identity provider (idP) and service provider (SP)

## OAuth
* provides delegated authorization to access server resources on behalf of a resource owner
* began in 2006 at Twitter
* RFC 5849: OAuth v1
* RFC 6749: OAuth v2

## OpenId Connect
* standardized "social" login

## Today's requirements for Authentication
* fault tolerant
* scale independently
* support delegation
* support federation (even within an organization)
* based on a standard
* support multi-factor

## Today's requirements for Authorization
* role based
* fine-grained permissions
* no tight coupling
* central overview and management

## Solution
* store for users and credentials
* central authentication system 
  * sign-up
  * password policies
  * change password
  * forgot password
  * captcha
  * ...
* browser-based login
* token-based authentication
  * user credentials validated by an authorization service
  * tokens provide a level of indirection
    * can be presented to resource providers (e.g., microservices, cloud-based services, ...)
  * valid for a limited time
  * opaque or structured (or structured but rendered opaque)

## State of the art tokens
* stateless
* support local validation by the receiver
* lightweight (small, easy to parse, ...)
* short lived
* can be part of each request

Current best alternative: JWT

## OpenId Connect (OIDC)
* standard protocol defining
  * process to obtain identity token for authentication
  * validation of token
  * token format
* does NOT define login process
* based on OAuth 2
* uses JWT as token format
* supports different flows

## OIDC flow
* access the app
* redirect to authorization server
* present login screen
* provide credentials
* issue token
* redirect to the app
* app can now provide the token on new requests

## Single sign-on
Supports single-sign-on (tokens can provide authorizations to use multiple apps)
* different strategies
  * reuse the same tokens (e.g., storing a code within the token)
  * keep the tokens private to one application but allow to retrieve a valid token for another application without re-authentication
    * more secure

## Single sign-out
Central logout at the authentication service
* invalidate tokens
* notify all apps

In OIDC, a logout token compatible with Security Event Token (SET) is used for propagating the logout

Alternative: Front-Channel Logout
* logout handled by the browser (e.g., by destroying cookies, things in local storage, ...)

## Federation
* integration of third-party user bases
  * social login
  * enterprise B2B
* can be implemented using OIDC or SAMLv2

SAMLv2
* still widely used in enterprises
* available out of the box in Active Directory
* modern authentication servers support federation based on SAMLv2

## Central vs local
* central authentication
  * identity with roles
  * consistent across systems
  * simplifies management
* local authorization
  * each service maps roles to permissions
  * promotes loose coupling
* central authorization management
  
## Delegation of authorization
* allow external parties to access resources on behalf or a user
* B2B scenario
  * SaaS
  * API as a service
* often implemented using OAuth 2

## Technologies for authorization
* on-premise
  * Keycloak: open source
  * IdentityServer
  * WSO2, Forgerock, OpenAM/OpenIDM
* as a service
  * Auth0
  * Stormpath
  * ...

## Technologies for applications
* Java
  * JAAS
  * client certificates
  * spring security: support for OAuth 2 & OIDC
  * apache Shiro
  * vendor-specific: keycloak, auth0, stormpath, ... (tight coupling but easy integration)

## Client certificates
* client certificates allow offloading of authentication to infrastructure
  * can be used as substitute for token based solution
  * or complement it to model trust relationship for machine to machine communication
* no single point of failure
* certificate rollout/management is complex (better leave this only for machine to machine communications)
* no solution for federation

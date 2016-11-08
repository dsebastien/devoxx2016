# Keycloak
Delegate security to Keycloak

* Open source
* OpenID / SAML / Kerberos
* Adapters for wildfly, eap, fuse, tomcat, jetty, spring boot, node.js
* Social login brokering
* Authz
* OTP
* client libraries for
  * Native iOS
  * Native Android
  * JS (including Cordova)
  * REST and SPI (JSON Web Tokens)

# Web.xml
Added a security constraint

```
<security-constraint>
    <web-resource-collection>
        <web-resource-name>Products</web-resource-name>
        ...
    </web-resource-collection>
    ...
</security-constraint>
```

keycloak.json: file containing all keycloak related properties.
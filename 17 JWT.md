# JSON WEB TOKENS

## About
Only wrote down the stuff I found most interesting (i.e., not a summary!)

## Recommendations
* use white list of tokens, not a blacklist!
* use JWTs internally (behind the API gateway) and session IDs for the outside world
* add application specific information inside the JWT (e.g., app name, api version, etc) to constrain the tokens

## JWT & Cookies
To protect against both CSRF and XSS attacks we can:
* after a successful authentication on the server side
  * generate a random id for the user's session
  * create a JWT for the user
    * put the random id in the token's data (e.g., JWT.CSRF = ...)
  * set a cookie for the client, containing the JWT token
    * HttpOnly & Secure flags!
    * domain as specific as possible
* client stores the JWT.CSRF value in local storage

With the above
* the client now has a CSRF synchronizer token in local storage and a secure/httOnly cookie with the JWT
* JavaScript code on the page cannot access the cookie (HttpOnly flag)
* the browser will automatically send the cookie when making requests for that domain
* the client-side app must manually get the CSRF token and add it to the request (specific header)

The back-end can then verify that the CSRF token is present and valid and it can also verify the JWT.

## Cool use cases
* mail confirmation (did this for my own project a while ago)

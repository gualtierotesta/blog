---
tags: [software, security, web, authentication, authorization]
---
# Web security

*Last update: 01 Aug 2023*

## References

1. [Web security by MDN](https://developer.mozilla.org/en-US/docs/Web/Security)
2. [Web security by the Chrome team](https://web.dev/secure/)
3. [Information security on Wikipedia](https://en.wikipedia.org/wiki/Information_security)
4. [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)

## Introduction

Web security is the intersection of the following:

- Network security --> TLS, firewall, load-balancer
- Information security (InfoSec): how we handle the data -> cryptography, access control...
- Application security (AppSec): how we use the applications --> vulnerability, credentials storage...

Security-related tools:

- WAF, Web Application Firewall, HTTP level firewalls
- IDS, Intrusion Detection System
- IPS, Intrusion Prevention System
- HSH, Hardware Security Module

The security goals are non-functional requirements (NFR) for all applications.

Information security's (see reference 3) primary focus is data *Confidentiality*, *Integrity* and *Availability* (the CIA triad).

[STRIDE](https://en.wikipedia.org/wiki/STRIDE_(security)) is a security model which describes the possible threads.

- Spoofing: when a person or a program assumes another person's identity
- Tampering: changing data and messages
- Repudiation: when someone disputes to be the author or an action
- Information disclosure (data leak, privacy breach): acquiring and publishing private info
- Denial of service (DoS)
- elevation of privilege


## HTTP codes

Security-related HTTP answers:

* **401** User is not authenticated.
* **403** The user is not allowed to perform the requested operation, even if they are authenticated.


## SOP, ORIGIN

ORIGIN: in any URL, the origin is the union of the protocol, the server and the port (if specified). For example the origin in the URL https://www.example.com/pages/index.html is https://www.example.com. (port is 443 here).

The same-origin policy (SOP) is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

SOP usually block read calls while writing calls (for ex. form submits) are usually allowed.

For more details [MDN on SOP](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)



## COOKIE

Cookie contents:

* `path` = "/" is the path used by the browser to establish if the cookie should be sent to the server or not depending on the current page
* `expiration` (expires and max-age) = cookie expiration date. If not specified, the cookie is a session cookie and it is deleted by the browser at the end of the session. If specified, the cookie is persistent. If the date is in the past, the cookie is deleted.
* `attributes` = Secure (send only on HTTPS),  HttpOnly (not readable from JS code), SameSite
* `Domain`: the domain of the cookie. If not specified, the cookie is "host-only"
* `name` = the name of the cookie


The suggested cookies attributes:

* Session cookie: HttpOnly, Secure, Path=/
* CSRF token cookie: Secure, SameSite=strict, Path=/



## CSRF (Cross-Site Request Forgery) and SameSite cookie attribute

Cross-site Request Forgery (CSRF) is a web attack where actions are performed using the user's identity without them knowing what is happening.

See [Wikipedia](https://en.wikipedia.org/wiki/Cross-site_request_forgery) for more details.

The user is logged in the website A and opens a new browser tab on Website B (the attacker). Website B calls website A back-end services; the browser sends the session cookie to the website A back-end which accepts the request. Even if the browser implements the same-origin policy, still many requests can be done from website B pages to website A server. 

The first defence line is to not have GET endpoints that perform create or update or delete actions. Use POST, UPDATE or DELETE methods as appropriate.

The second defence line is to use the `SameSite` attribute for the session cookie; when set, the browser will not send the cookie if the page does not have the same site (domain) as the cookie.

The SameSite attribute can have the following values:

* strict: the cookie is not sent in all cross-site requests
* lax: the cookie is sent on navigation requests only
* none: the cookie is always sent

Alternative to the SameSite attribute is the `double-submit cookie` mechanism. At the first request, the server creates a special CSRF token cookie with a value that the client (the page) should include at every request using the X-CSRF-Token header. The server compares the value with the one that has been generated and allows the request accordingly.


## CORS

Cross-Origin Resource Sharing (CORS) is a security mechanism that allows websites and APIs to control and overcome the same origin policy implemented by the browser.

See [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) for more details.

The browser performs a pre-flight request, using the HTTP method OPTIONS, to evaluate if the cross-origin request is possible or not. 

The pre-flight response from the server includes several CORS headers:

    Access-Control-Allow-Origin
    Access-Control-Allow-Headers
    Access-Control-Allow-Methods
    Access-Control-Allow-Credentials (should be true if we use session cookies)
    Access-Control-Max-Age
    Access-Control-Expose-Headers
    
The pre-flight response status code can be:

* 403: the request cannot be performed because not allowed by the resource server
* 204 (no-contents): the request can be done



---
tags: [software, security, web, authentication, authorization, token, jwt]
---
# Tokens

*Last update: 01 Aug 2023*

## Introduction

The security of web applications is often based on tokens.


## JWT

The JSON Web Token (JWT), pronounced "JOT", is a popular kind of security token and it is based upon the JSON format.

The typical JWT usage is as an authentication token:

    Authentication: Bearer <jwt>

The JWT is divided into three base64 encoded sections, separated by a ".".
 
    header "." payload "." signature

Standard HEADER claims:

* typ: the type of the token (how the payload can be interpreted)
* alg: the name of the algorithm used to make the signature

Header example :

		{
			typ: 'JWT', 
			alg: 'HS256'
		}

Standard PAYLOAD claims:

* iss: the issuer of the token
* sub: the subject of the token
* aud: the audience of the token
* exp: this will probably be the registered claim most often used. This will define the expiration as a NumericDate value. The expiration MUST be after the current date/time.
* nbf: (not before) defines the time before which the JWT MUST NOT be accepted for processing
* iat: (issued at) the time the JWT was issued. Can be used to determine the age of the JWT
* jti: unique identifier for the JWT. Can be used to prevent the JWT from being replayed. This is helpful for a one-time use token.

Example:

        {  
          "iss": "http://example.org",  
          "aud": "http://example.com",  
          "iat": 1356999524,  
          "nbf": 1357000000,  
          "exp": 1407019629,  
          "jti": "id123456",  
          "typ": "https://example.com/register",  
          "custom-property": "foo",  
          "name": "Rob McLarty",  
          "id": 78  
        }

Having an expiration date (exp claim) allows the emitting server to not handle the token disablement process. The expiration date is usually within 24 hours but it can be 1 or 2 hours long.

JWT advantages:

* There is no need for server-side sessions
* the same token can be used for different endpoints/applications if they share the same secret or the same token validation endpoint.
* Cookies are not required so no CSRF issues.
* Compatible with CORS
* Compatible with non-browser-based applications

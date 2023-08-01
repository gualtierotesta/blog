---
tags: [software, security, api]
---
# API security

*Last update: 01 Aug 2023*

## Introduction

A [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy) or, better, an API gateway, is usually placed in front of an API server/service to:

- handle TLS/SSL terminations
- validate the credentials (user authentication, tokens validation)
- facade back-end services

## The security pipeline

Every API service should implement a **security pipeline**. The steps in the pipeline, in the order they process the upcoming requests, are the following:

1) Rate limiting / throttling
2) Auditing/logging: track and log all requests and responses, assigning a unique ID to all inbound requests to enable inter-service tracing.
3) Implement/ verify the CORS mechanism
4) Check generic (non-endpoint specific) validation rules, like the "Content-Type" header value should be "application/json" for all endpoints.
5) Identification and authentication: user authentication and/or token validation. User sessions can be created after authentication.
6) Access control/authorization. If the user is authenticated (previous step), user permissions should be checked against the request requirements. See also the [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege
7) Endpoint-specific validation rules, input data validation


## API KEY

The API key is an opaque secret generated server-side and shared with a confidential client.

Every client request should include the API key in an HTTP header.

API Key vs tokens:

1. Using tokens, it is easier to have more granular and variable authorizations while API key authorizations are fixed and usually inherited by the creator of the key.
2. Tokens (like JWT) can also be used for other purposes like the user profile (see the OIDC's ID Token)
3. Tokens are also used in other authentication/authorization mechanisms like OAuth/OIDC.
4. The API key does not have an explicit expiration date
5. The API key should not be used when the client is public (not confidential) because the secret cannot be protected. 
6. The API key is opaque so it cannot be inspected.
7. It's not easy to rotate API keys.

On the other side, API keys are easy to use

## JWT Tokens verification

The application that receives a token should verify it before consumption.

The first check to do is token integrity. The signature should be calculated again and compared with the signature included in the token.

The second check is JWT validity: not expired (exp claim), not already used (jti claim).
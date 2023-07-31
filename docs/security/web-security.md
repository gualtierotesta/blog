---
tags: [software, security, web, authentication, authorization]
---
# Web security

*Last update: 31 Jul 2023*

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

## API security

A reverse proxy or, better, an API gateway, is usually placed in front of an API server/service to:

- handle TLS/SSL terminations
- validate the credentials (user authentication, tokens validation)
- facade back-end services

Every API service should implement a **security pipeline**. The steps in the pipeline, in the order they process the upcoming requests, are the following:

1) Rate limiting / throttling
2) Auditing/logging: track and log all requests and responses, assigning a unique ID to all inbound requests to enable inter-service tracing.
3) CORS
4) Generic (non-endpoint specific) validation rules check like "Content-Type" should be "application/json"
5) Identification and authentication: user authentication and/or token validation. User sessions can be created after authentication.
6) Access control/authorization. If the user is authenticated (previous step), user permissions should be checked against the request requirements. See below for related HTTP response codes. See also reference 4.
7) Endpoint-specific validation rules, input data validation

Security-related HTTP answers:

    401 User is not authenticated.
    403 The user is not allowed to perform the requested operation, even if they are authenticated.


## SOP, ORIGIN

ORIGIN: in any URL, the origin is the union of the protocol, the server and the port (if specified). For example the origin in the URL https://www.example.com/pages/index.html is https://www.example.com. (port is 443 here).

The same-origin policy (SOP) is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

SOP usually block read calls while writing calls (for ex. form submits) are usually allowed.

For more details [MDN on SOP](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)



## AUTHENTICATION

The web supports several kinds of authentication mechanisms:

**Basic**: user credentials are provided on every HTTP requests.

Token base: chiamo prima un endpoint con le credenziali che poi mi fornisce un token a scadenza che uso per fare le altre chiamate.
Questo token è spesso chiamato session token o cookie token.

Cookie per salvare il token: l'endpoint di login genera il token ed risponde con l'header Set-Cookie. Tutte le successive chiamate avranno il token nel cookie.
 
Cookie:
	path = "/" indica il percorso usato per determinare se il cookie deve essere inviato al server
	scadenza (expires e max-age) = scadenza del cookie. Se non indicata il cookie è cancellato alla chiusura del browser (session cookie).
        Se indicata, il cookie è persistent. Se è una data nel passato, il cookie è cancellato dal browser
	attributi = 
        Secure (solo su HTTPS, non HTTP)
        HttpOnly (non leggibile da codice JS)
        SameSite
    Domain: se non indicato, il cookie (host-only) è inviato a tutte le chiamate al server che ha creato il cookie
        se indicato: il cookie è inviato a quel dominio e a tutti i suoi sotto-domini
	nome tipico = JSESSIONID

Alternativa ai cookie è salvare il token sull'URL.

## CSRF (Cross Site Request Forgery)

Il sito A chiama le API di back-end del sito B sfruttando il fatto che il browser invia i cookie relativi al sito B quindi facendo credere al back-end del sito B che la chiamata arriva dal suo front-end. SOP impedisce al sito A di vedere i risultati della chiamata ma intanto la chiamata è stata fatta.
Prima difesa è assicurarsi che le chiamate GET non siano dispositive perchè SOP permette le chiamate, bloccando solo le risposte.
Seconda protezione è marcare con "SameSite" i cookie di autorizzazione: il browser non manda al back-end del sito B i cookie marcati come samesite se le chiamate arrivano dal sito A per cui le chiamate falliscono perché non autorizzate. 
Per site si intende il dominio registrato tipo "example.com"
SameSite=strict: i cookie non sono inviate in tutte le chiamate "illegali"
SameSite=lax: i cookie sono inviate solo quando l'utente ha premuto su un link. Tutto il resto è bloccato.
I cookie SameSite non sono ancora uno standard ufficiale e i vecchi browser potrebbero non implementarlo.
Altra protezione: double-submit cookie. Il server crea un cookie per CSRF (senza attributo HttpOnly). 
Tutte le chiamate del client devono includere il valore di questo cookie nell'header X-CSRF-Token. 
Il server controlla che il valore del cookie e dell'header coincidano.
Per ulteriore protezione il token CSRF deve essere derivato dal security token (session cookie) ad esempio con un hash tipo SHA-256 in modo che nessuno possa cambiare sia il cookie CSRF sia l'header X-CSRF-Token per cercare di bypassare le protezioni.

Non autenticato: 401
Autenticato ma senza permessi: 403
Richiesta con content type non corretto: 415

Cookie di sessione (JSESSIONID) : HttpOnly, Secure, Path=/
Cookie di CSRF                  : Secure, SameSite=strict, Path=/

Logout:
 - invalidare la sessione
 - cancellare il cookie (mettere la sua expire date nel passato)


## CORS

Il browser effettua una prima chiamata di pre-fligth (HTTP OPTIONS) per determinare se è possibile chiamare l'endpoint. In questa chiamata l'header Origin riporta l'origine della chiamata.

La response alla chiamata di pre-flight contiene i seguenti headers:

    Access-Control-Allow-Origin
    Access-Control-Allow-Headers
    Access-Control-Allow-Methods
    Access-Control-Allow-Credentials: deve essere a true se vogliamo usare i cookie di sessione
    Access-Control-Max-Age
    Access-Control-Expose-Headers
    
    Pre-flight: si risponde con 403 se fallisce, con 204 (no contents) se ok
    
Non usare cookie SameSite in presenza di CORS perché incompatibili.


## JWT (Jason Web Token, "JOT")

Struttura:

    header "." payload "." signature

    Tutti i tre base64 encoded in maniera distinta

    Uso tipico:
    "Authentication: Bearer xxxx.yyyy.zzzz"
    ma può essere passato anche sull'URL, altro header o anche in un cookie (se proprio si vuole)

Standard HEADER claims:
	typ: the type of the token (how the payload can be interpreted)
        alg: the name of the algorithm used to make the signature

	Esempio:
		{
			typ: 'JWT', 
			alg: 'HS256'
		}

Standard PAYLOAD claims:

    iss: the issuer of the token
    sub: the subject of the token
    aud: the audience of the token
    exp: this will probably be the registered claim most often used. 
	    This will define the expiration as a NumericDate value. 
	    The expiration MUST be after the current date/time.
    nbf: (not before) defines the time before which the JWT MUST NOT be accepted for processing
    iat: (issued at) the time the JWT was issued. 
	    Can be used to determine the age of the JWT
    jti: unique identifier for the JWT. 
	    Can be used to prevent the JWT from being replayed. 
	    This is helpful for a one time use token.

    Esempio:
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

Compiti della API che riceve una richiesta con JWT

    1. Verificare che il JWT non sia stato manipolato (forged).
       Ricalcola la firma partendo dall'header e dal payload ricevuti e la confronta con quella ricevuta.
       Per fare questo deve sapere il secret
       Non si può usare l'algoritmo indicato nell'header ricevuto perché potrebbe essere stato messo a null.

    2. Verificare che il JWT sia valido: non scaduto (exp), non già usato (jti) o inserito in blacklist.


Il fatto di avere una scadenza (claim exp) permette al server di rilasciare dei token senza preoccuparsi di disabilitarli.

La scadenza tipica è di 24 ore ma può essere anche di 1 o 2 ore.

Vantaggi del JWT:
    * Niente sessioni lato server. Non serve un session storage
    * Lo stesso token può essere usato su diversi endpoint/applicazion se queste condividono il secret (o l'endpoint di validazione)
    * Niente cookie quindi niente prolemi di CSRF (i token non sono inseriti automaticamente dal browser)
    * Tutte le applicazioni, anche quello non basate su browser, possono gestire gli header
    * Scadenza inclusa
    * Compatibile con CORS


## API KEY

Secret generato server side e condiviso con i client.

Confronto API Key vs JWT:
     
    * Sicurezza granulare: con JWT si può distinguere le autorizzazioni sulle richieste. 
      Con un API key tutti possono fare tutto a meno di creare, se possibile, una API key specifica per 
      l'insieme di operazioni indicate. Vedi cosa fa adesso GitHub.
    * Omogeneità della architettura: usare i JWT per tutto permette di avere una soluzione più omogenea.
    * I JWT possono essere self-emitted permettendo soluzioni decentralizzate. 
    * Compatibile con OAuth2. Invece di restituire un token opaco, il server può usare un JWT.
    * I token opachi non sono debuggabili.
    * Le API key non hanno una scadenza esplicita.
    * Le API key non vanno usato in app per mobile dove il dispositivo potrebbe essere rubato.

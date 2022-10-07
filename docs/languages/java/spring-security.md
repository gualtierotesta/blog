---
tags: [java, spring, security]
---
# Spring Security

*Last update: 7 Oct 2022*

Spring security:

* is a set of servlet filters that help you add authentication and authorization to your web application.
* auto-generates login/logout pages
* protects against common exploits like CSRF.

[Spring Security Guide](https://www.marcobehler.com/guides/spring-security)


### Main modules

Without Spring Boot
    
```xml    
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
</dependency>
```

With Spring Boot
    
```xml    
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

    
### Web security

Example

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
        .authorizeRequests()
        
            // unauthenticated
            .mvcMatchers("/", "/home").permitAll()
            
            // authenticated with specific authority
            .mvcMatchers("/admin").hasAuthority("ROLE_ADMIN")
            .mvcMatchers("/callcenter").hasAnyAuthority("ROLE_ADMIN", "ROLE_CALLCENTER")
            
            // authenticated with specific role
            .mvcMatchers("/admin").hasRole("ADMIN")
            .mvcMatchers("/callcenter").hasAnyRole("ADMIN", "CALLCENTER")
    
            // using SpEL
            .mvcMatchers("/admin")
                .access("hasRole('admin') and hasIpAddress('192.168.1.0/24') and @myCustomBean.checkAccess(authentication,request)")
            
            // all other request requires authentication without specific role
            .anyRequest().authenticated()
            
            .and()
        .formLogin()  // enable the login page
            .loginPage("/login") // custom login page
            .permitAll()
            .and()
        .logout() // enable the Spring generated logout page
            .permitAll()
        .and()
        .httpBasic(); // enable basic authentication
    }
}
```

For further info on `SpEL` [here](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#el-access)
    
Matchers:
    
    mvcMatchers: (preferred)
        '/a'    : match '/a' and '/a/'
        '/a/*'  : match '/a/xx' but not '/a' and not '/a/b/c'
        '/a/**' : match '/a', '/a/b', '/a/b/c'
    antMatchers: old
        The main difference is on the '/a' matching: the mvcMatcher match '/a/' while the antMatcher not.
    regexMacthers: for more complex cases
        
    
## Authentication

There are three possible cases.

**Case 1**

The user directory is handled by the application, using a database for example.

Two beans should be implemented:
* UserDetailsService which finds the user from their username
* PasswordEncoder which defines how the password are encrypted
    
The `UserDetails` interface is the user profile. The class `org.springframework.security.core.userdetails.User` is a concrete implementation that can be used.

Some concrete implementations of the `UserDetailsService` interface are the classes `InMemoryUserDetailsManager` and `JdbcUserDetailsManager`.
    
The most used concrete implementation of the `PasswordEncoder` interface is the `BCryptPasswordEncoder` class. The class `PasswordEncoderFactories.createDelegatingPasswordEncoder()` use a prefix to understand which hashing algorithm should be used.

**Case 2**

The user directory is handled by an external application, a LDAP server for example.

In this case, we should implement the `AuthenticationProvider` interface.
    
Example:
    
```java    
public class CustomAuthenticationProvider implements AuthenticationProvider {

    Authentication authenticate(Authentication authentication) throws AuthenticationException {
    
        String username = authentication.getPrincipal().toString();
        String password = authentication.getCredentials().toString();

        // Call the external service to authenticate the user and return the profile
        User user = callAnotherAutheticationService(username, password);
        
        if (user == null) {
            throw new AuthenticationException("could not login");
        }
        return new UserNamePasswordAuthenticationToken(
            user.getUsername(), 
            user.getPassword(), 
            user.getAuthorities()); 
    }
        // other method ignored
}
```
**Case 3**

When we use OAuth2 / OpenID 

    
### Authorization

Definitions:

* Authority: it is usually a string like "USER", "ADMIN", or "ROLE_GUEST"
* Role: it is an authority with the `ROLE_` prefix

Authority can be also a role if it has the ROLE_ prefix.

In general, the role is a high-level concept, collecting several authorizations described as authorities. The authorities are mapped to specific actions the user can perform.

Example:
    
    Authority: read, write, update, delete
    Role Reader  --> authority read
    Role Manager --> authority read, write, update
    Role Admin   --> authority read, write, update, delete

Authority:

    Interface   GrantedAuthority 
    Class       SimpleGrantedAuthority 

The authorization checks usually start at the web security level, where each URL is mapped to roles and authorities. 

In addition, the authorization checks can be also implemented at the method level (who can call the method), at every level of the application. This is required for non-web applications.

The security at the method level should be enabled:

```java
@Configuration
@EnableGlobalMethodSecurity(
    prePostEnabled = true, // enable the Spring @PreAuthorize e @PostAuthorize annotations
    securedEnabled = true, // enable the Spring @Secured annotation
    jsr250Enabled = true) // enable the Java standard @RolesAllowed annotation   
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    ....
}
```

The `@Secured` and `@RolesAllowed` annotations are equivalent and they require roles.
    
The `@PreAuthorize` and `@PostAuthorize` annotations can express more complex conditions thanks to the SpEL.

Examples:

```java    
@Secured("ROLE_ADMIN")

@RolesAllowed("ADMIN")

@PreAuthorize("isAnonymous()")

@PreAuthorize("#contact.name == principal.name")
void aMethod(Contact contact) {...}

@PreAuthorize("#name == authentication.principal.username")
void aMethod(String name) {...}

@PreAuthorize("ROLE_ADMIN")
```

The `@PostAuthorize` annotation can be used when the access control depends on the value returned by the method. The method is always executed but its returned value is transmitted only if the PostAuthorize succeed.

Example:

```java
@PostAuthorize("returnObject.roles.contains('reader')")
```

If the authorization condition is too complex to be easily expressed with a SpEL string, the condition can be implemented by a class that extends from the `PermissionEvaluator` interface. The class should be registered before using it.

```java
@PostAuthorize("hasPermission(returnObject, 'ROLE_admin')")
```
    
    
### Cross-Site-Request-Forgery: CSRF

The `CSRFFilter` filter looks for a CSRF token in every POST/PUT/DELETE request.

The CSRF token is usually a hidden form field or a cookie or an HTTP header.

The CSRF token value is generated by Spring Security and saved in the HTTP session.

If you are only providing a stateless REST API where CSRF protection does not make any sense, 
you would completely disable CSRF protection

```java
http.csrf().disable();
```      

## Spring Security with Spring MVC

When using Spring Security and Spring MVC:

1. We can use the mvcMatchers instead of the andMatchers.
1. We can inject the user (principal) in the MVC controllers:

        @RequestMapping("/xxx")
        public String xxxx(@AuthenticationPrincipal CustomUser customUser
    
1. Inject the CSRF token

    
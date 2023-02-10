---
tags: [application server, glassfish, oracle]
---

#  GlassFish

*Last update: 10 Feb 2023*

## Passwords handling

Examples:

    asadmin --host localhost --port 4848 login
    asadmin --host localhost --port 4848 --user admin change-admin-password

## Domains

The `asadmin` tool is located in the  `<appserver-installation-directory>/bin` dir.

Start the default domain:

    asadmin start-domain

Start the domain xxxx:

    asadmin start-domain xxxx

Stop the default domain:

    asadmin stop-domain

Delete a domain:

    asadmin delete-domain xxxx

Restart the domain:

    asadmin restart-domain xxxx

List available domains:

    asadmin list-domains

Create a new domain xxxx:

    asadmin create-domain --user admin --nopassword --instanceport 8100 xxxx

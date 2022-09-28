---
tags: [tools, postgresql, dbms]
---

# PostgreSQL

*Last update: 28 Sep 2022*

### Command line

Connect client to an instance

    psql --dbname=<db-name> \
       --host=<host> \
       --port=<port> \
       --username=<username>

### Client

Exit from the client

    \q
    
Enable verbose messages

    \set VERBOSITY verbose
    
    
### BACKUP

Dump database schema to file

    pg_dump <db-name> --format=p --username=<username> --verbose  --schema-only --file=<filename>.ddl

Dump database data to file    

    pg_dump <db-name> --format=p --username=<username> --verbose  --data-only --column-inserts --file=<filename>.ddl

Dump everything

    pg_dump <db-name> --format=c --username=<username> --verbose  --file=<filename>.backup

Restore everything

    pg_restore -U <username> --dbname=<db-name> <filename>.backup

Restore data    

    pg_restore <db-name> --username=<username> --data_only --verbose --file=<filename>.ddl



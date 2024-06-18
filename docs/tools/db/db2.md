---
tags: [tools, ibm, db2, udb, dbms]
---

# IBM DB2

*Last update: 26 Sep 2022*

### General info

* On Unix/Linux, the operating system user is also the DB instance administrator.
* On Windows, the DB administrator is not the Windows user.
* Every DB2 instance can contain one or more databases
* Every database can contain one or more schemas.

Row size limit:

*  4005 bytes in a table space with a 4K page size
*  8101 bytes in a table space with an 8K page size
* 16293 bytes in a table space with a 16K page size
* 32677 bytes in a table space with a 32K page size 

### Tables and indexes maintenance

Show statistics about tables and indexes.

    RUNSTATS ON TABLE <table-name> AND SAMPLED DETAILED INDEXES ALL
    RUNSTATS ON TABLE <table-name> WITH DISTRIBUTION AND DETAILED INDEXES ALL ALLOW WRITE ACCESS

To verify which tables require a reorg:

    select tabschema, tabname from sysibmadm.admintabinfo where reorg_pending = 'Y'

Executing the reorg of the tables/indexes

    REORG TABLE <table-name>
    REORG INDEXES ALL FOR TABLE <table-name>

Check the current tables and indexes status

    reorgchk current statistics on table all
    reorgchk update statistics on table all
    reorgchk update statistics on table user


### Command line

How to start DB2

    db2start

Connect

    db2 connect to <database-name> user <user-name> using password

Dump a database contents into a file

    db2look -d <database-name> -a -e -o <sql file name>
    
Show DB instance Status

    db2pd -everything

Global registry

    db2greg -dump

Run the internal monitoring

    db2top -d <database-name>

Backup

    db2 backup database <database-name> online to <file-name> with 2 buffers buffer 1024 parallelism 1 compress include logs without prompting;

Restore

    db2 restore db <database-name> from <file-name> taken at <timestamp>
    db2 restore db <database-name> continue
    db2 rollforward db <database-name> to end of logs and complete


### Queries performances analysis

Design Advisor:

    db2advis -d <database-name> -n <schema-name> -s 'select * from ..'
    db2advis -d <database-name> -i <file with one or more queries>

Query explains:

    db2expln -d <database-name> -t -q 'select * from .. '
    db2expln -d <database-name> -t -stmtfile <file with one or more queries> -terminator \;


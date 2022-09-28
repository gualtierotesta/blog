---
tags: [tools, oracle, mysql, dbms]
---

# MySQL

*Last update: 28 Sep 2022*


### Command line commands

How to login

    mysql -u <user-name> -p


### Users and grants

Create new user:

    grant all privileges on <db-name>.* to '<user-name>'@'localhost' IDENTIFIED BY '<user-password>' WITH GRANT OPTION;

Add grants:

    grant all on <db-name>.* to '<user-name>'@'localhost' 

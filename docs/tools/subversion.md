---
tags:
  - tools
  - subversion
  - svn
---

# SubVersion (svn)

*Last update: 16 Aug 2022*


Checkout

    svn co --username <user> --password <password> svn://<host>/<repo>

Status

    svn status

Local copy cleanup

    svn cleanup

Branch/tag creation

    svn copy <trunk> branches/<branch> -m "..."
    svn copy https://<host>/<repo>/<proj>  https://<host>/<repo>/tags/<proj>/<tag> -m "..."

Branch checkout

    cd branches
    svn co http://<host>/svn/<repo>/branches/<branch>

List repo files

    svn ls  https://<host>/<repo>/tags

Update branch with trunk updates

    cd branches/<branch>
    svn merge ^/calc/trunk

Update trunk with branch contents

    cd trunk
    svn update
    svn merge --reintegrate branches/<branch>

Ignore current dir

    svn propedit svn:ignore .

Global ignores

    svn propedit svn:global-ignores .
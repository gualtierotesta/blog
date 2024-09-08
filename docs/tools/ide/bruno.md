---
tags: [tools, ide, client, bruno, api, postman]
---

# Bruno

*Last update: 8 Sep 2024*

Article on [Medium](https://medium.com/@gualtierotesta/bruno-an-ide-for-apis-8f0257394e41)

Links:

* [Bruno website](https://www.usebruno.com/)
* [Bruno command line tools](https://docs.usebruno.com/bru-cli/overview)
* [GitHub](https://github.com/usebruno/bruno)
* [Documentation](https://docs.usebruno.com/introduction/what-is-bruno)
* [Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=bruno-api-client.bruno)
* [Migrating from Postman](https://docs.usebruno.com/get-started/import-export-data/postman-migration)

## How to use Bruno

The suggested directory structure is like the following:

```text
├── .env                     optional, for secrets and passwords
├── API 1                    dir to group requests belonging to the same API
│   ├── request-1.bru
│   ├── request-2.bru
├── API 2
│   ├── request-1.bru
├── README.md
├── bruno.json              the main Bruno file
├── environments            the Bruno environments
│   ├── prod.bru
│   ├── test.bru
├── node_modules
├── package-lock.json
└── package.json            BRUNO CLI and additional node.js dependencies
```

API 1 and API 2 are folders grouping the requests for each API.

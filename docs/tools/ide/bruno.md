---
tags: [tools, ide, client, bruno, api, postman]
---

# Bruno

*Last update: 7 Sep 2024*

## Introduction

[Bruno](https://www.usebruno.com/) is an API requests editor similar to Postman.

It is a desktop application that should be installed on the local machine.
There are no cloud/browser version and no server side components.
Every request execution is from the local machine.

Requests are stored in text files on the local machines. They are in BRU format and can be committed to a VCS (like git) repository.

With Bruno, we can build the HTTP requests, specifying the URL, the query parameters,
the headers and the payload (if needed), then run the request and observes the response from the server.

We can also:

* Add assertions to automatically validate the responses; for example, this request should return 200.
* Add JavaScript code to test the response; for example, the response body should contain a field named 'state'
* Pass data from one request to another; for example to use an access_token from an authorization request as
  Authentication header for another request
* Use [Bruno command line tools](https://docs.usebruno.com/bru-cli/overview) to run a set of requests, useful in a CI/CD pipeline
* Use Node.js modules and code to perform custom actions before and after the requests.

Bruno is a free to use tool with personal and enterprise commercial versions with additional features.
See the [pricing](https://www.usebruno.com/pricing) section on the website.

Other links:

* [GitHub](https://github.com/usebruno/bruno)
* [Documentation](https://docs.usebruno.com/introduction/what-is-bruno)

## When to use Bruno

* To use external APIs you plan to use in your applications.
  The purpose is to learn and document how to use them.
* To test your APIs for integration test purposes, maybe with a pipeline

## How to use Bruno

The suggested way is to create a dedicated directory or repository for Bruno files.
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

API 1 and API 2 are folders to group together the requests for each API.

Useful is the [Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=bruno-api-client.bruno)
if you use this IDE.

We can use Node.js libraries to process requests and responses.
Few libraries are embedded in Bruno (see list [here](https://docs.usebruno.com/scripting/inbuilt-libraries)).
Others can be imported via `npm as documented [here](https://docs.usebruno.com/scripting/external-libraries).

## Bruno vs Postman

Compared with Postman, Bruno has the following advantages:

1. It is based on local files oriented that are easily shared and versioned via git, and
we do not expose our secrets in the cloud (as Postman requires).
2. It is free of charge
3. Sensitive requests are more protected because they don't need to come from an external site.

The only downsize is that you need to have Bruno locally installed on the machines to have an UI.
We can use the Bruno CLI node.js module in the CI/CD pipelines.

We can easily migrate collections from Postman; the flow is documented [here](https://docs.usebruno.com/get-started/import-export-data/postman-migration).
After the migration, I suggest reviewing and updating the directory structure.

Postman environments can be also migrate to Bruno with a similar flow. See the Secrets Management section.

## Secrets management

Secrets (API keys, passwords...) can be saved as:

1. Bruno's variables in the environments BRU files
2. environment variables in the .env file. The .env file is automatically read by Bruno if it is in the same folder as the `bruno.json` file.

If you follow the [.env solution](https://docs.usebruno.com/secrets-management/dotenv-file), its contents should be read via Node.js `process.env` array.
It is also suggested to create a .env.template file to document its contents.

IMPORTANT: In both cases, secrets (API keys, passwords...) should not be stored in clear in the Bruno repository.

We have several options:

1. Use tools like [git-crypt](https://github.com/AGWA/git-crypt) to crypt the environments files or the .env file
2. Exclude the environment files and the .env file from the VCS (= add them in the .gitignore file) and share them in other way.
3. Ask Bruno to internally store the environment files vars values when marked as secret.
  See [here](https://docs.usebruno.com/secrets-management/secret-variables) for details.
4. Use an external secret manager like HashiCorp Vault. This feature is documented [here](https://docs.usebruno.com/secrets-management/hashicorp-vault/overview),
but it is not included in the free edition.

---
tags: [ide, api, postman, bruno]
---

*Last update: 24 Nov 2024*

## Introduction

[Bruno](https://www.usebruno.com/) is a web API requests editor like Postman.

It is a desktop application that should be installed on the local machine. There are no cloud/browser versions and no server-side components.
Every request execution is from the local machine.

The web requests are stored in text files on the local machines. They are in a Bruno proprietary format (named `bru`) and can be committed to a VCS (like Git) repository.

With Bruno, we can build the HTTP requests, specifying the URL, the query parameters, the headers, and the payload (if needed), then run the request and observe the response from the server.

We can also:

* Add assertions to validate the responses automatically; for example, this request should return 200.
* Add JavaScript code to test the response; for example, the response body should contain a field named `state`
* Pass data from one request to another; for example, to use an access_token from an authorization request as Authentication header for another request
* Use [Bruno command line](https://docs.usebruno.com/bru-cli/overview) tools to run a set of requests, useful in a CI/CD pipeline
* Use Node.js modules and code to perform custom actions before and after the requests.

Bruno is a free-to-use tool with personal and enterprise commercial versions and additional features.

See the [pricing](https://www.usebruno.com/pricing) section on the website.

Other links:

* [GitHub repo](https://github.com/usebruno/bruno)
* [Documentation](https://docs.usebruno.com/introduction/what-is-bruno)

## When to use Bruno

There are two main use cases:

* To use external APIs you plan to use in your applications. The purpose is to learn and document how to use them.
* To test your APIs for integration test purposes, maybe with a pipeline

The first use case is when our application uses external APIs from a vendor, another team in the company, or even from ourselves.

To use an external API, we usually need:

* the secrets to access the API: user credentials, API keys, tokens…
* the documentation of the API endpoints we need to use
* examples of how to use them
* test data to pass to the endpoints
* response examples

The best approach to collect and verify this data is creating an “external APIs” requests collection and using Bruno to verify that credentials are correct, endpoints accept the data we are sending to them and return the data we expect from them.

The second use case is to validate our own APIs. We can create a “project API” requests collection with the purpose of:

* show our API users how to use the API, together with Open API documentation
* test our API in functional and smoke tests

## How to use Bruno

The suggested way is to create a dedicated directory in our project repository or a dedicated repository for Bruno files.

The suggested directory structure is like the following:

```text
├── .env optional, for secrets and passwords
├── API 1 dir to group requests belonging to the same API
│ ├── request-1.bru
│ ├── request-2.bru
├── API 2
│ ├── request-1.bru
├── README.md
├── bruno.json the main Bruno file
├── environments the Bruno environments
│ ├── prod.bru
│ ├── test.bru
├── node_modules
├── package-lock.json
└── package.json BRUNO CLI and additional node.js dependencies
```

API 1 and API 2 are folders grouping the requests for each API.

The Bruno team is sharing a project on GitHub as an example.

Useful is the [Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=bruno-api-client.bruno) if you use this IDE.

We can use Node.js libraries to process requests and responses. Few libraries are embedded in Bruno; see the complete list here. Others libraries can be imported via `npm` as documented here.

## Bruno vs Postman

Compared with Postman, Bruno has the following advantages:

* It is based on local files that are easily shared and versioned via Git, and we do not expose our secrets in the cloud (as Postman requires).
* It is free of charge
* Sensitive requests are more protected because they don’t need to come from an external site.

The only downside is that you need to have Bruno installed locally on the machines to have a UI.

We can use the Bruno CLI node.js module in the CI/CD pipelines.

We can easily migrate collections from Postman; the flow is documented here.
After the migration, I suggest reviewing and updating the directory structure.

Postman environments can also be migrated to Bruno with a similar flow. 

## Secrets management

Secrets (API keys, passwords…) can be saved as:

1. Bruno’s variables in the environments BRU files

2. environment variables in the `.env` file. The .env file is read by Bruno if it is in the same folder as the `bruno.json` file.

If you follow the [.env solution](https://docs.usebruno.com/secrets-management/dotenv-file), its contents should be read via Node.js `process.env` array. I also recommend creating a `.env.template` file to document its contents.

**IMPORTANT**: In both cases, secrets (API keys, passwords…) should not be stored in clear in the Bruno repository.

We have several options:

* Use tools like git-crypt to crypt the environments files or the .env file
*   Exclude the environment files and the .env file from the VCS (= add them in the `.gitignore` file) and share them in another way.
* Ask Bruno to store the var values internally when marked as secret. See [here](https://docs.usebruno.com/secrets-management/secret-variables) for details.
* Use an external secret manager like HashiCorp Vault. This feature is documented [here](https://docs.usebruno.com/secrets-management/secret-variables), but it is not included in the free edition. This solution is the most secure.

I hope you will enjoy using Bruno as I do. Thanks to the Bruno team for sharing this tool.

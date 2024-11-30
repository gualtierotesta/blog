---
tags:
  - tools
  - git
  - git-crypt
  - encryption
---

# GIT CRYPT

*Last update: 30 Nov 2024*

## Introduction

[git-crypt](https://github.com/AGWA/git-crypt) is an encryption/decryption tool integrated with git.

It works under two modes:

1. Symmetric key
2. GPG key

## Installation

git-crypt binaries for Windows and Linux are available from the [project repository on GitHub](https://github.com/AGWA/git-crypt/releases).

On macOS, you can use brew:

    brew install git-crypt

For other installation instructions, you can refer to the [official documentation](https://github.com/AGWA/git-crypt/blob/master/INSTALL.md).

## Configuration

We use the `.gitattributes` file to define which files in the repo should be encrypted:

    secretfile filter=git-crypt diff=git-crypt
    *.key filter=git-crypt diff=git-crypt
    secretdir/** filter=git-crypt diff=git-crypt

## Symmetric key

To initialize the repository to use git-crypt

    git-crypt init

This command will generate a symmetric key. To save the key to a file

    git-crypt export-key /path/to/key

The file containing the key should be stored in a safe position and shared with everybody who need to access the encrypted files.

IMPORTANT: the key file should not be committed in the repo.

The other users of the repository should clone the project and run the following command:

    git-crypt unlock /path/to/key

## GPG key

To initialize the repository to use git-crypt

    git-crypt init

To add a GPP key to the repository git-crypt keys list:

    git-crypt add-gpg-user key_id | GPG fingerprint | email

The other users of the repository should clone the project and run the following command:

    git-crypt unlock

git-crypt will use the user GPG key(s) to unlock the repo.

## Other commands

To check if git-crypt has been configured correctly and all sensitive files are encrypted:

    git-crypt status

---
tags: [tools, security, privacy, gpg]
---

# GNU Privacy Guard (GnuPG or GPG)

*Last update: 02 Oct 2022*

### GPG key

To create a new GPG key, just follow [these](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key) instructions.

### Command line commands

Export a public GPG key in binary format:

    gpg --output public.gpg --export <email>

Export a public GPG key in text format:

    gpg --output public.txt --export --armor <email>

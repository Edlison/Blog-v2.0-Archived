---
title: Github SSH-key
categories:
- Notebook
tags:
- git
---

# Assign private key

modify config in `~/.ssh/config`.

```shell
# Github
Host *
AddKeysToAgent yes
UseKeychain yes
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_common
```

# Switch protocol

Switch url from `http` type to `ssh` type in each repo.

```shell
# deprecated
url = http://github.com/Edlison/xxx.git
# approved
url = git@github.com:Edlison/xxx.git
```



----

Reference

https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent


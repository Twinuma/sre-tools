---
title: "SSH Setting"
date: 2018-10-25T15:21:49+09:00
draft: false
Tags: ["linux", "ssh"]
Categories: ["linux"]
---

## `~/.ssh/config`
```
Include */config
ServerAliveInterval 30

Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_github_rsa
  User git

Host heroku.com
  User git
  port 22
  Hostname heroku.com
  IdentityFile ~/.ssh/heroku_rsa
  TCPKeepAlive yes
  IdentitiesOnly yes

Host git-codecommit.*.amazonaws.com
  User xxxx
  IdentityFile ~/.ssh/aws_codecommit
```
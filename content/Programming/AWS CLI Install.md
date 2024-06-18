---
title: Ubuntu AWS CLI Install
date: 2023-11-10 15:10
Slug: aws_cli_install
summary: How to install AWS CLI on Ubuntu
tags: programming, aws, ubuntu
---
This is a little piece of code that can be hard to find sometimes - how to install AWS CLI via the Ubuntu terminal:


```shell
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" 
unzip awscliv2.zip 
sudo ./aws/install
```

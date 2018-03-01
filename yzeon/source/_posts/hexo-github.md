---
title: github pages
date: 2017-12-28 12:35:36
tags:
---
ssh -T git@github.com

# start the ssh-agent in the background
eval $(ssh-agent -s)
Agent pid 59566

ssh-add ~/.ssh/id_rsa
---
layout: post
title: "Comment découper un commit"
date: 2016-09-03 18:25:06 UTC
comments: true
---

# Comment découper un commit ?

On code on code, on commit, et bientôt on va merger
sa branche sur master.

Oui mais en faite, un des commits est vraiment enorme et pourrait etre splitté
avant le merge final.

J'utilise ce mode genial `git rebase -i`

D'autre facons son possible.

## Ce qu'il faut :

+ Ne pas avoir déjà pushé
ou
+ Ne pas être sur master

## Au début

Je suis sur la branche `fake-branch`, en retard ou pas par rapport à
`master` je lance le rebase :

```
git rebase -i master

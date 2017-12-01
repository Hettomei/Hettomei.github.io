---
layout: post
title: "Workflow Gerrit"
date: 2017-10-02 09:16:06 UTC
comments: true
---

# Mise en situation

En ce moment, on m'impose d'utiliser Gerrit.
Pire, on m'impose, pour résoudre un bug ou une feature de ne faire qu'un commit.

# TLDR

```bash
git checkout -b ma-feature
git commit -m "foo"
git commit -m "Splendid commit message"
# ....

git checkout --detach
git rebase -i master # use pick, squash, fixup
git push origin HEAD:refs/for/master
git checkout -
```

# Solution

Commiter autant que je veux sur ma branche. Tout squash en un sur une branch anonyme.

# Process

On veut la feature BONJOUR

## On se met bien

```bash
git checkout master
git pull
git checkout -b feature-bonjour
```

## On code

```bash
git commit -m 'first commit'
# git commit -m
# ...
```

## La feature est prete

On edit le dernier commit !

Pourquoi ? Pour avoir un joli message, sur notre branche, au cas où des modifs sont demandées après la review.

```bash
git commit --amend
```

## On squash, loin de notre branche préférée

Donc en gros,

On se met sur une branche anonyme,

on rebase en :

+ gardant le premier commit,
+ fixup tous les suivants,
+ squash le dernier (pour récupérer le joli message bien formatté)

```bash
# Branch anonyme
git checkout --detach

git rebase -i master

# On edite de la sorte
pick e21512bd881e9eb183eafc81738e0e817ddcaa52 first commit
fixup dbd709da1e78fcd4c3a700583c6fed514bb55011 second commit
fixup 8e7a606fd6797f8a52e83a08378e70d0eb26a151 very big big commit
squash 3fd93497ec8741d960b69e2d651e31339f5517ab Feature bonjour: foo bar baz done
```

Tout est en un seul HORRIBLE commit en haut de master. Pour s'en convaincre

```bash
git log -2
```

# Everything is presque done

C'est gerrit, on push d'un certaine façon

```bash
git push origin HEAD:refs/for/master
```

Un petit `git checkout -` pour retourner sur la branche `feature-bonjour` et eviter de faire des ajouts sur cette branche qui va disparaitre.

# Tout n'est pas parfait

Des modif' sont demandées ou conflit avec origin/master pas de panique.

On part de l'idée que personne n'a ajouté de code sur votre commit gerrit :

On retourne sur sa branche, on met à jour

```bash
git checkout feature-bonjour
git fetch
# On aime etre à jour
git rebase origin/master
# On bosse
git commit -m 'Forgot one case'
```

Maintenant on se retrouve à l'etape **# On squash** avec un commit en plus. Donc le rebase va ressembler à ca :

```bash
# Branch anonyme
git checkout --detach

git rebase -i master

# On edite de la sorte
pick e21512bd881e9eb183eafc81738e0e817ddcaa52 first commit
fixup dbd709da1e78fcd4c3a700583c6fed514bb55011 second commit
fixup 8e7a606fd6797f8a52e83a08378e70d0eb26a151 very big big commit
squash 3fd93497ec8741d960b69e2d651e31339f5517ab Feature bonjour: foo bar baz done
fixup 1943dbfe09c9076bca44e91943dbfe09c9076bca Forgot one case
```

Hé oui, on 'squash' le joli message bien formatté, mais on 'fixup' le dernier commit car on n'a pas besoin du message

On met à jour gerrit (chez moi, 11003, pas chez vous !)

```bash
git push origin HEAD:refs/changes/11003
# on evite les betises
git checkout -
```

Enjoy la mocheté du **un seul commit**.

# HELP, j'ai tout perdu mes commits

Cherchez de l'aide autour de `git reflog` <- tous vos commits s'y trouvent

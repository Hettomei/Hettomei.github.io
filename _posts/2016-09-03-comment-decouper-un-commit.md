---
layout: post
title: "Comment découper un commit"
date: 2016-09-03 18:25:06 UTC
comments: false
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

```bash
git rebase -i master

# et sous mon éditeur préféré apparait

pick e21512bd881e9eb183eafc81738e0e817ddcaa52 first commit
pick dbd709da1e78fcd4c3a700583c6fed514bb55011 second commit
pick 8e7a606fd6797f8a52e83a08378e70d0eb26a151 very big big commit
pick 3fd93497ec8741d960b69e2d651e31339f5517ab last commit before merge
```

il faut modifier 'VERY BIG BIG COMMIT' ce qui donne

```bash
pick e21512bd881e9eb183eafc81738e0e817ddcaa52 first commit
pick dbd709da1e78fcd4c3a700583c6fed514bb55011 second commit
e 8e7a606fd6797f8a52e83a08378e70d0eb26a151 very big big commit
pick 3fd93497ec8741d960b69e2d651e31339f5517ab last commit before merge
# :wq
```

on quitte le rebase, il passe `first` et `second` en haut de master

puis on arrive ici

```bash
Stopped at 8e7a606... VERY BIG BIG COMMIT
You can amend the commit now, with

        git commit --amend

        Once you are satisfied with your changes, run

        git rebase --continue
```

Le commit à déjà été appliqué, on est juste apres, pour preuve :

```bash
git --oneline -1
ff85617 VERY BIG BIG COMMIT
```

donc il faut reprendre et splitter


Reprendre le precedent commit :

cette command supprime le dernier commit et remet toutes les modifs
dans "Changes to be committed:"

```bash
git reset --soft HEAD~1
```

maintenant, supposons qu'on avait commité les fichier a.txt et b.txt
mais que chaque fichier suffit par commit :

on enleve b.txt

```bash
git reset HEAD b.txt
git commit -m "file a remove empty char"

git add b
git commit -m "file b.txt remove compute length"
```

là notre commit 'VERY BIG BIG COMMIT' n'existe plus, il a été
remplacé par "file a ..." et "file b.txt..." .

On peut reprendre le rebase toujours en cours :

```bash
git rebase --continue

git log --oneline
dd9198a Last commit before merge
86253b0 file b.txt remove compute length
f4bc3e5 file a remove empty char
22fc7b8 second commit
e8c71d9 first commit
```

notre branche est nickel, on merge

```bash
git checkout master
git merge --no-ff -
git push
```

+ `--no-ff`
"no fast forward" donc meme si je suis en haut de master et donc qu'un merge est inutile
je le garde quand meme, comme ca dans mon historique je vois clairement ma branche.
Pratique pour revert

+ `-` (le tiret à la fin du git merge)
Vous connaissez `cd -` 'retourne au dossier precedant' maintenant vous connaissez `git checkout -`

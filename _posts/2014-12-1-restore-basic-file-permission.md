---
layout: post
title: "Problemes dans vos droits unix ?"
date: 2014-11-15 18:25:06 UTC
comments: true
---

Salut, vous venez de récupérer un bakcup qui n'a pas géré les droits.

Résultat vous vous retrouvé avec des permissions bizarre, et pire, la coloration
de ```ls``` sous zsh est horrible ?! Voici la solution :

(Je pars d'une config basic qui ne propose aucun fichier de type "execution")

## Restaurer les fichiers :

```
find . -type f -exec chmod 644 {} +
```

## Restaurer les dossiers :

```
find . -type d -exec chmod 755 {} +
```

## Supprimer ces .DS_Store chiant

```
alias rmDS='find . -name ".DS_Store" -depth -exec rm {} \;'
```

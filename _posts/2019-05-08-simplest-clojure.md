---
layout: post
title: "Simplest clojure project"
date: 2019-05-08 07:29:27+00:00
comments: true
---

# Comment lancer un script clojure ?

## Install

On va au plus simple, on oublie leiningen, boot au `java -cp` ....

On install les clojure tools [https://clojure.org/guides/getting_started](https://clojure.org/guides/getting_started)

```
curl -O https://download.clojure.org/install/linux-install-1.10.0.442.sh
chmod +x linux-install-1.10.0.442.sh
sudo ./linux-install-1.10.0.442.sh
```

(Répeter l'operation avec les nouveaux liens pour une upgrade)

Tout marche ?
```
clj --help

Usage: clojure [dep-opt*] [init-opt*] [main-opt] [arg*]
       clj     [dep-opt*] [init-opt*] [main-opt] [arg*]
```

## Le fichier

```
;cat hello.clj

(ns simplest.clojure.code
  (:gen-class))

(defn hello-world []
  (println "Hello World"))

(hello-world)
```

## Run

```
clj hello.clj
Hello World
```

Merci au revoir....

## Better

Ça fonctionne. Mais on peut faire mieux.

Je déteste ces fichiers de code qui se lance juste parce qu'on les 'charges'.
Je veux que tout soit fonction.

```
;cat hello.clj

(ns simplest.clojure.code
  (:gen-class))

(defn hello-world []
  (println "Hello World"))

(defn -main [& args]
  (hello-world))
```

Il ne se passe rien :
```
clj hello.clj
```

Normal, rien n'oblige à lancer la fonction -main. 
Il faut l'appeler de cette facon :

```
clj --init hello.clj --main simplest.clojure.code
Hello World
```

+ -i: l'input
+ --main "Call the -main function from namespace" Donc il faut lui passer le namespace

C'est beaucoup mieux. Maiiiiiiiis contraignant.

On peut définir le tout avec un project.clj, mais la on sort du cadre du 'simplest clojure'.

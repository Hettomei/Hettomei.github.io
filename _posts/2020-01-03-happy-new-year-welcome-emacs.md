---
layout: post
title: "Passer de Vim à Emacs ?"
date:  2020-01-03 14:36:00+00:00
comments: true
---

# Emacs

Nouvelle année. Nouveau défi : Après 7 ans de Vim, je regarde ce que propose Emacs.

Liste en vrac des trouvailles.

## Comment implementer vim-commentary

Ma premiere commande ajouté/créé :

```
(evil-define-key 'normal 'global (kbd "gcc") 'comment-line)
```

Et ca fonctionne !!

Le must, pour trouver 'comment-line j'ai juste fait `M-x comment` puis `tab` et j'ai vu les possibilité.

Bon, finalement, j'ai installé le package [evil-commentary](https://github.com/linktohack/evil-commentary) car il gere vraiment le mouvement.

how to install :
```
M-x package-install RET evil-commentary

;; then, inside ~/.emacs.d/init.el
(evil-commentary-mode)
```

# Comment reproduire l'excellent vim-skip dans emacs ?

J'ai toujours utilisé [vim-skip](https://github.com/jayflo/vim-skip) qui permet d'avancer
moitié par moitié en appuyant sur `s`.

Je ne trouve pas d'equivallent evil-skip

elisp me semble très bien foutu.

15 minutes apres :

```
(defun middle-of-line-forward ()
  "Put cursor at the middle of the rest of the line. try to mimic vim-skip s"
  (interactive)
  (goto-char (/ (+ (point) (point-at-eol)) 2)))

(defun middle-of-line-backward ()
  "Put cursor at the middle of the beginning of the line. try to mimic vim-skip S"
  (interactive)
  (goto-char (/ (+ (point) (point-at-bol)) 2)))

(define-key evil-normal-state-map (kbd "s") 'middle-of-line-forward)
(define-key evil-normal-state-map (kbd "S") 'middle-of-line-backward)
```

ET CA MARCHE !

DU PREMIER COUP !

Mind blown.

# Afficher / voir les fonctions / trouver la source d inspiration :

```
M-x RET find-function RET what-line
```

Et soudain, vous voyez :

```
(defun what-line ()
  "Print the current buffer line number and narrowed line number of point."
  (interactive)
  (let ((start (point-min))
	(n (line-number-at-pos)))
    (if (= start 1)
	(message "Line %d" n)
      (save-excursion
	(save-restriction
	  (widen)
	  (message "line %d (narrowed line %d)"
		   (+ n (line-number-at-pos start) -1) n))))))
```

# Avoir le meme comportement que ...

Vim:
```
set scrolloff=3
```

Emacs :
```
(custom-set-variables
 '(hscroll-margin 15)
 '(hscroll-step 1)
 '(scroll-conservatively 10000)
 '(scroll-margin 3)
 '(scroll-step 1))
```

---

Vim :
```
set nowrapscan
```

Emacs :
```
(custom-set-variables
 '(evil-search-wrap nil))
```

---

Remember last location in file

vim :
```
augroup lastlocation
  autocmd!
  autocmd bufreadpost * if line("'\"") > 0 && line("'\"") <= line("$") | execute "normal! g'\"" | endif
augroup end
```

emacs :
```
(custom-set-variables
 '(save-place-mode t))
```

---

Activate mouse in terminal

vim :
```
if has('mouse')
  set mouse=a
endif
```

emacs :
```
(custom-set-variables
 '(xterm-mouse-mode t))
```

# Tout semble exister, tout semble clair.

En écrivant ce post, j'ai vu des trailing whitespace.

Pour les effacer

Sous vim : `:%s/\s\+$//e`

Sous emacs : `M-x delete-trailing-whitespace` (bien sur, en faisant `tab` tout le long)

# Liens

+ [How to consider emacs : no prestige, it is just a tool](https://www.youtube.com/watch?v=FLjbKuoBlXs)
+ [Evil is an extensible vi layer for Emacs](https://evil.readthedocs.io/en/latest/overview.html#installation-via-package-el)
+ [vim-skip](https://github.com/jayflo/vim-skip)
+ [vim-commentary](https://github.com/tpope/vim-commentary)
+ [evil-commentary](https://github.com/linktohack/evil-commentary)
+ Ma configuration : <https://github.com/Hettomei/my_computer_conf/blob/master/default/emacs.d/init.el>
+ Post écris avec markdown-mode, more at <https://jblevins.org/projects/markdown-mode/>

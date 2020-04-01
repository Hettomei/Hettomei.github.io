---
layout: post
title: "get vim insert C-n in Doom Emacs"
date:  2020-04-01 13:53:00+00:00
comments: true
---

# Doom Emacs

Thanks hlissner for bringing <https://github.com/hlissner/doom-emacs>

# completion in buffer

Doom comes with [company][http://company-mode.github.io/]

# Setup package

```
(use-package! company
  :config
  ;; disable auto popup after x seconds
  (setq company-idle-delay nil
        ;; allow code completion inside comments and string (took 5hours to figure out...)
        company-dabbrev-code-everywhere t)
  (define-key company-active-map (kbd "<tab>") #'company-complete-common)
  ;; you can use space to complete word
  (define-key company-active-map (kbd "SPC") #'company-complete-selection))
```

# Setup C-n


```
(map! :i  "C-n" #'+company/dabbrev
      :i  "C-p" #'+company/dabbrev-code-previous)
```

# I want a space after pressing... space

```
;; Add a space
(defun company-after-completion-hook (&rest _ignored)
  (just-one-space))

(add-hook! 'company-completion-finished-hook #'company-after-completion-hook)
```

Enjoy

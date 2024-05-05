---
title: Advice browse-url
categories: [emacs]
---

Emacs has this effect of scale where the more you use it (and in more
different areas), the more useful it becomes.

For example, when browsing code/data in the repo/repl/shell/db, if you
use stripe, you find those "tagged_ids" like `cus_123`, `sub_123`....

With this little trick here, we can advice the browse-url function so
that we amend the url path depending on a pattern matching result.

```elisp
;;; (keymap-global-set "C-c o" 'browse-url-at-point)
(defun rgc/add-stripe (args)
  "prepend the right stripe path to be able to find the entity
you're looking for"
  (let* ((arg (s-replace-regexp "^https?://" "" (car args)))
         (path
          (cond
           ((s-match "^price_.*" arg) "prices")
           ((s-match "^sub_.*" arg) "subscriptions")
           ((s-match "^prod_.*" arg) "products")
           ((s-match "^evt_.*" arg) "events")
           ((s-match "^cus_.*" arg) "customers"))))
    (if path
        (cons (concat "https://dashboard.stripe.com/test/" path "/" arg) nil)
      args)))

(advice-add 'browse-url :filter-args #'rgc/add-stripe)
```

After evaluating this, placing the cursor in a `sub_123123` word and
calling `browse-url-at-point` will DWIM and bring me to the url of
that entity.

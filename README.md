# Da lao's init.el

Learn from a dalao and so many interesting things;
```
(setq package-archives '(("gnu"   . "https://elpa.emacs-china.org/gnu/")
                         ("melpa" . "https://elpa.emacs-china.org/melpa/")))
(package-initialize) ;; You might already have this line

(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(markdown-command
   "pandoc -f markdown -t html -s --mathml --highlight-style pygments --standalone --quiet")
 '(org-agenda-files (quote ("~/manage.org")))
 '(package-selected-packages
   (quote
    (magit solarized-theme evil-nerd-commenter evil-surround org-evil evil-leader evil-cleverparens evil-escape evil-snipe use-package rainbow-delimiters org-pomodoro emms-player-mpv markdown-mode slime-company slime pdf-tools company-math latex-extra auctex org-bullets color-theme-sanityinc-solarized counsel hungry-delete))))

(require 'hungry-delete)
(global-hungry-delete-mode)

(load-theme 'solarized-dark t)

(tool-bar-mode -1)

(ivy-mode 1)
(setq ivy-use-virtual-buffers t)
(setq enable-recursive-minibuffers t)
(global-set-key "\C-s" 'swiper)
(global-set-key (kbd "C-c C-r") 'ivy-resume)
(global-set-key (kbd "<f6>") 'ivy-resume)
(global-set-key (kbd "M-x") 'counsel-M-x)
(global-set-key (kbd "C-x C-f") 'counsel-find-file)
(global-set-key (kbd "<f1> f") 'counsel-describe-function)
(global-set-key (kbd "<f1> v") 'counsel-describe-variable)
(global-set-key (kbd "<f1> l") 'counsel-find-library)
(global-set-key (kbd "<f2> i") 'counsel-info-lookup-symbol)
(global-set-key (kbd "<f2> u") 'counsel-unicode-char)
(global-set-key (kbd "C-c g") 'counsel-git)
(global-set-key (kbd "C-c j") 'counsel-git-grep)
(global-set-key (kbd "C-c k") 'counsel-ag)
(global-set-key (kbd "C-x l") 'counsel-locate)
;;(global-set-key (kbd "C-S-o") 'counsel-rhythmbox)
(define-key minibuffer-local-map (kbd "C-r") 'counsel-minibuffer-history)

;; org mode
(add-hook 'org-mode-hook (lambda () (org-bullets-mode 1)))
(with-eval-after-load 'org
  (setq org-startup-indented t) ; Enable `org-indent-mode' by default
  (add-hook 'org-mode-hook #'visual-line-mode))
;; http://www.mediaonfire.com/blog/2017_07_21_org_protocol_firefox.html
;; https://orgmode.org/worg/org-contrib/org-protocol.html#org6e070c8
(require 'org-protocol)
(setq org-capture-templates
      (quote
       (("L"
         "Default template"
         entry
         (file+headline "~/capture.org" "Notes")
         "* %^{Title}\n\n  Source: %u, %c\n\n  %i"
         :empty-lines 1)
        ;; ... more templates here ...
        )))
(setq org-protocol-default-template-key "w")

;; (setq my-fonts '("Consolas" "XHei Mono" "Segoe UI Emoji"))
;; (create-fontset-from-fontset-spec standard-fontset-spec) ;to make --daemon work
;; (dolist (font (reverse my-fonts))
;;   (set-fontset-font "fontset-standard" 'unicode font nil 'prepend))
;; (add-to-list 'default-frame-alist '(font . "fontset-standard"))
;; (set-face-attribute
;;  'default nil :font "Consolas 12")
;; Chinese Font
;; (dolist (charset '(kana han symbol cjk-misc bopomofo))
;;   (set-fontset-font (frame-parameter nil 'font)
;;                     charset
;;                     (font-spec :family "XHei Mono" :size 14)))

;; (dolist (charset '(kana han symbol cjk-misc bopomofo))
;;   (set-fontset-font "font-chinese"
;;                     charset
;;                     (font-spec :family "XHei Mono" :size 14)))
;; (add-to-list 'default-frame-alist '(font . "fontset-chinese"))

;; (require 'pyim)
;; (require 'pyim-basedict)
;; (pyim-basedict-enable) 
;; (setq default-input-method "pyim")
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )

(add-hook 'after-init-hook 'global-company-mode)
(add-hook 'LaTeX-mode-hook #'latex-extra-mode)
(add-hook 'LaTeX-mode-hook #'prettify-symbols-mode)

;; delimiters
(add-hook 'prog-mode-hook #'rainbow-delimiters-mode)
(add-hook 'prog-mode-hook #'show-paren-mode)

;; 4.28.827
(pdf-tools-install)

 ;; to use pdfview with auctex
 ;; (setq TeX-view-program-selection '((output-pdf "PDF Tools"))
 ;;    TeX-view-program-list '(("PDF Tools" TeX-pdf-tools-sync-view))
 ;;    TeX-source-correlate-start-server t) ;; not sure if last line is neccessary

 ;; to have the buffer refresh after compilation
 (add-hook 'TeX-after-compilation-finished-functions
        #'TeX-revert-document-buffer)

(add-to-list 'auto-mode-alist '("\\.markdown\\'" . markdown-mode))
(add-to-list 'auto-mode-alist '("\\.md\\'" . markdown-mode))

(autoload 'gfm-mode "markdown-mode"
   "Major mode for editing GitHub Flavored Markdown files" t)
(add-to-list 'auto-mode-alist '("README\\.md\\'" . gfm-mode))

(define-key pdf-view-mode-map "j" 'pdf-view-next-line-or-next-page)
(define-key pdf-view-mode-map "k" 'pdf-view-previous-line-or-previous-page)
(define-key pdf-view-mode-map (kbd "C-s") 'isearch-forward-regexp)

(defun my-inhibit-startup-screen-always ()
  "Startup screen inhibitor for `command-line-functions`.
Inhibits startup screen on the first unrecognised option."
  (ignore (setq inhibit-startup-screen t)))

(add-hook 'command-line-functions #'my-inhibit-startup-screen-always)

(recentf-mode 1)
(setq recentf-max-menu-items 25)
(run-at-time nil (* 5 60) 'recentf-save-list)

(setq inferior-lisp-program "sbcl")
(setq slime-contribs '(slime-fancy slime-company))

;; (define-key company-active-map (kbd "\C-n") 'company-select-next)
;; (define-key company-active-map (kbd "\C-p") 'company-select-previous)
;; (define-key company-active-map (kbd "\C-d") 'company-show-doc-buffer)
;; (define-key company-active-map (kbd "M-.") 'company-show-location)

;; markdown mode


;; insert date
(defun insert-date (prefix)
  "Insert the current date. With prefix-argument, use ISO format. With
   two prefix arguments, write out the day and month name."
  (interactive "P")
  (let ((format (cond
                 ((not prefix) "%Y-%m-%d %H:%M:%S")
                 ((equal prefix '(4)) "%Y-%m-%d")
                 ((equal prefix '(16)) "%H:%M:%S")
		 ((equal prefix 3) "%b-%d-%Y")))
        (system-time-locale "en_US"))
    (insert (format-time-string format))))
(global-set-key (kbd "C-c d") 'insert-date)


;; column mode
(setq column-number-mode 1)

;; Stop the beep
(setq visible-bell 1)

(evil-mode 1)
(global-evil-leader-mode)
(evil-leader/set-leader "<SPC>")
(setq evil-default-state 'emacs)
(define-key evil-emacs-state-map (kbd "C-o") 'evil-execute-in-normal-state)
(evil-snipe-mode +1)
(evil-snipe-override-mode +1)
(evil-define-key 'visual evil-snipe-local-mode-map "z" 'evil-snipe-s)
(evil-define-key 'visual evil-snipe-local-mode-map "Z" 'evil-snipe-S)
;; Vim key bindings
(require 'evil-leader)
(global-evil-leader-mode)
(evil-leader/set-key
  "ci" 'evilnc-comment-or-uncomment-lines
  "cl" 'evilnc-quick-comment-or-uncomment-to-the-line
  "ll" 'evilnc-quick-comment-or-uncomment-to-the-line
  "cc" 'evilnc-copy-and-comment-lines
  "cp" 'evilnc-comment-or-uncomment-paragraphs
  "cr" 'comment-or-uncomment-region
  "cv" 'evilnc-toggle-invert-comment-line-by-line
  "."  'evilnc-copy-and-comment-operator
  "\\" 'evilnc-comment-operator ; if you prefer backslash key
)
(put 'upcase-region 'disabled nil)

;;;;;;
(scroll-bar-mode -1)
(global-linum-mode t)
(global-hl-line-mode t)

(setq inhibit-splash-screen t)
(setq-default cursor-type 'bar) 
(setq make-backup-files nil)
(setq initial-frame-alist (quote ((fullscreen . maximized))))

(delete-selection-mode t)
(defun open-my-init-file()
  (interactive)
  (find-file "~/.emacs.d/init.el"))

(global-set-key (kbd "<f2>") 'open-my-init-file)
```

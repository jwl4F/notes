# notes

``` bash
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'
}

git_status() {
    local branch=$(parse_git_branch)
    if [ -n "$branch" ]; then
        local status=$(git status --porcelain 2>/dev/null)
        local ahead=$(git rev-list --count @{u}..HEAD 2>/dev/null)
        local behind=$(git rev-list --count HEAD..@{u} 2>/dev/null)
        local color="\033[1;32m"  # Bold Green by default
        local symbols=""

        # Check for changes
        if [ -n "$status" ]; then
            color="\033[1;33m"  # Bold Yellow if there are changes
        fi

        # Check for ahead/behind
        [ "$ahead" -gt 0 ] && symbols+=" \033[1m↑$ahead"
        [ "$behind" -gt 0 ] && symbols+=" \033[1m↓$behind"

        echo -e " $color($branch)$symbols\033[00m"
    fi
}

# Bold Green for [user@host]
userhost="\[\033[1;32m\][\u@\h]\[\033[00m\]"

export PS1="${userhost}:\w\$(git_status)\n\$ "
```

```
(load-theme 'gruber-darker t)

(global-set-key (kbd "C-c !") 'shell-command)
(global-set-key (kbd "C-z") 'undo)
(global-set-key (kbd "C-c b") 'compile)
(global-set-key (kbd "C-c C-b") 'compile)
(global-set-key (kbd "M-z") 'toggle-truncate-lines)
(setq compile-command "./build.sh")

(setq display-line-numbers-type 'relative)
(global-display-line-numbers-mode)

(setq inhibit-startup-screen t)

(tool-bar-mode 0)
(menu-bar-mode 0)
(scroll-bar-mode 0)
(column-number-mode 1)
(show-paren-mode 1)

(setq display-buffer-alist
      '(("\\*compilation\\*"
         (display-buffer-reuse-window
          display-buffer-in-side-window)
         (side . bottom)
         (slot . 0)
         (window-height . 0.3))))

;;(add-to-list 'load-path (expand-file-name "~/simpc/"))
;;(require 'simpc-mode)
;;(add-to-list 'auto-mode-alist '("\\.[hc]\\(pp\\)?\\'" . simpc-mode))
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(package-selected-packages '(gruber-darker-theme)))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
```

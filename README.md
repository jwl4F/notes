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

---
title: "Terminal Setting"
date: 2018-10-25T15:18:42+09:00
draft: false
Tags: ["mac", "terminal"]
Categories: ["mac"]
---

## `~/.zshrc`
```
#
# Executes commands at the start of an interactive session.
#
# Authors:
#   Sorin Ionescu <sorin.ionescu@gmail.com>
#

# Source Prezto.
if [[ -s "${ZDOTDIR:-$HOME}/.zprezto/init.zsh" ]]; then
  source "${ZDOTDIR:-$HOME}/.zprezto/init.zsh"
fi

# Customize to your needs...
eval "$(rbenv init -)"

# tabtab source for serverless package
# uninstall by removing these lines or running `tabtab uninstall serverless`
[[ -f /usr/local/lib/node_modules/serverless/node_modules/tabtab/.completions/serverless.zsh ]] && . /usr/local/lib/node_modules/serverless/node_modules/tabtab/.completions/serverless.zsh
# tabtab source for sls package
# uninstall by removing these lines or running `tabtab uninstall sls`
[[ -f /usr/local/lib/node_modules/serverless/node_modules/tabtab/.completions/sls.zsh ]] && . /usr/local/lib/node_modules/serverless/node_modules/tabtab/.completions/sls.zsh
export PATH="/usr/local/opt/libpq/bin:$PATH"
export PATH="/usr/local/opt/curl/bin:$PATH"
export PATH="/usr/local/opt/go/libexec/bin:$PATH"
```

## `~/.zpreztorc`
```
#
# Sets Prezto options.
#
# Authors:
#   Sorin Ionescu <sorin.ionescu@gmail.com>
#

#
# General
#

# Set case-sensitivity for completion, history lookup, etc.
# zstyle ':prezto:*:*' case-sensitive 'yes'

# Color output (auto set to 'no' on dumb terminals).
zstyle ':prezto:*:*' color 'yes'

# Set the Zsh modules to load (man zshmodules).
# zstyle ':prezto:load' zmodule 'attr' 'stat'

# Set the Zsh functions to load (man zshcontrib).
# zstyle ':prezto:load' zfunction 'zargs' 'zmv'

# Set the Prezto modules to load (browse modules).
# The order matters.
zstyle ':prezto:load' pmodule \
  'environment' \
  'terminal' \
  'editor' \
  'history' \
  'directory' \
  'spectrum' \
  'utility' \
  'completion' \
  'prompt'

#
# Autosuggestions
#

# Set the query found color.
# zstyle ':prezto:module:autosuggestions:color' found ''

#
# Editor
#

# Set the key mapping style to 'emacs' or 'vi'.
zstyle ':prezto:module:editor' key-bindings 'vi'

# Auto convert .... to ../..
# zstyle ':prezto:module:editor' dot-expansion 'yes'

#
# Git
#

# Ignore submodules when they are 'dirty', 'untracked', 'all', or 'none'.
# zstyle ':prezto:module:git:status:ignore' submodules 'all'

#
# GNU Utility
#

# Set the command prefix on non-GNU systems.
# zstyle ':prezto:module:gnu-utility' prefix 'g'

#
# History Substring Search
#

# Set the query found color.
# zstyle ':prezto:module:history-substring-search:color' found ''

# Set the query not found color.
# zstyle ':prezto:module:history-substring-search:color' not-found ''

# Set the search globbing flags.
# zstyle ':prezto:module:history-substring-search' globbing-flags ''

#
# Pacman
#

# Set the Pacman frontend.
# zstyle ':prezto:module:pacman' frontend 'yaourt'

#
# Prompt
#

# Set the prompt theme to load.
# Setting it to 'random' loads a random theme.
# Auto set to 'off' on dumb terminals.
#zstyle ':prezto:module:prompt' theme 'powerline'
zstyle ':prezto:module:prompt' theme 'sorin'
#zstyle ':prezto:module:prompt' theme 'paradox'
#zstyle ':prezto:module:prompt' theme 'steeef'
#zstyle ':prezto:module:prompt' theme 'pure'

#
# Ruby
#

# Auto switch the Ruby version on directory change.
# zstyle ':prezto:module:ruby:chruby' auto-switch 'yes'

#
# Screen
#

# Auto start a session when Zsh is launched in a local terminal.
# zstyle ':prezto:module:screen:auto-start' local 'yes'

# Auto start a session when Zsh is launched in a SSH connection.
# zstyle ':prezto:module:screen:auto-start' remote 'yes'

#
# SSH
#

# Set the SSH identities to load into the agent.
# zstyle ':prezto:module:ssh:load' identities 'id_rsa' 'id_rsa2' 'id_github'

#
# Syntax Highlighting
#

# Set syntax highlighters.
# By default, only the main highlighter is enabled.
# zstyle ':prezto:module:syntax-highlighting' highlighters \
#   'main' \
#   'brackets' \
#   'pattern' \
#   'line' \
#   'cursor' \
#   'root'
#
# Set syntax highlighting styles.
# zstyle ':prezto:module:syntax-highlighting' styles \
#   'builtin' 'bg=blue' \
#   'command' 'bg=blue' \
#   'function' 'bg=blue'

#
# Terminal
#

# Auto set the tab and window titles.
# zstyle ':prezto:module:terminal' auto-title 'yes'

# Set the window title format.
# zstyle ':prezto:module:terminal:window-title' format '%n@%m: %s'

# Set the tab title format.
# zstyle ':prezto:module:terminal:tab-title' format '%m: %s'

#
# Tmux
#

# Auto start a session when Zsh is launched in a local terminal.
# zstyle ':prezto:module:tmux:auto-start' local 'yes'

# Auto start a session when Zsh is launched in a SSH connection.
# zstyle ':prezto:module:tmux:auto-start' remote 'yes'

# Integrate with iTerm2.
# zstyle ':prezto:module:tmux:iterm' integrate 'yes'
#

alias gip='curl http://checkip.amazonaws.com/'
alias lip='ifconfig en0 | grep inet'

export AWS_CONFIG_FILE=/Users/niinuma/config/awscli.config
export AWS_CREDENTIAL_PROFILES_FILE=~/.aws/config

alias ss='ssh $(grep -iE "^host[[:space:]]+[^*]" ~/.ssh/config|peco|awk "{print \$2}")'
alias ss-cw='ssh $(grep -iE "^host[[:space:]]+[^*]" ~/.ssh/cw/config|peco|awk "{print \$2}")'

function peco-select-history() {
  local tac
  if which tac &gt; /dev/null; then
    tac="tac"
  else
    tac="tail -r"
  fi
  BUFFER=$(history -n 1 | eval $tac | peco --query "$LBUFFER")
  CURSOR=$#BUFFER
  zle clear-screen
}

zle -N peco-select-history
bindkey '^r' peco-select-history

alias awsshell='aws-shell'

setopt print_eight_bit
setopt auto_pushd
setopt auto_cd

setopt bang_hist
setopt extended_history
setopt hist_ignore_dups
setopt share_history
setopt hist_reduce_blanks

autoload -U compinit; compinit
setopt auto_list
setopt auto_menu
setopt list_packed
setopt list_types

setopt nolistbeep
setopt nobeep

autoload colors
colors

alias gogl='/Users/takachan/goo.gl.sh'

alias now='date +%Y%m%d%H%M%S'

alias ssh-keygen='ssh-keygen -C ""'

eval "$(pyenv init -)"
export PATH="$HOME/.pyenv/shims:/usr/local/google-cloud-sdk/bin:$PATH"

EDITOR=vim

alias dsstore='find /usr/local -name '.DS_Store' -type f -ls -delete'
export PATH="$HOME/.embulk/bin:$PATH"

export VAULT_ADDR='http://13.114.102.188:8200'
export VAULT_TOKEN=3j16qM44XBDvbinyj1sE1Ppe
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
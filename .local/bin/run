#!/bin/bash
shopt -s expand_aliases

## Git and Zsh must be installed
## This is the curl command to run the script
## curl -sL https://bit.ly/3CnJXWo | bash
## or
## curl -sL https://raw.githubusercontent.com/bkmo/.zsh-dotfiles/master/.local/bin/run | bash

git clone --bare -b master https://github.com/bkmo/.zsh-dotfiles.git $HOME/.zsh-dotfiles &&

alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'

config --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME config --local status.showUntrackedFiles no &&

echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.bashrc

echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc

config --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME checkout

echo "Deal with conflicting files, then run (possibly with -f flag if you are OK with overwriting)"
echo "/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME checkout -f"

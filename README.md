
This is the curl command to run the restore script
curl -sL https://bit.ly/3CnJXWo | bash

#!/bin/bash
git init --bare $HOME/.zsh-dotfiles &&
#alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME' &&
/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME config --local status.showUntrackedFiles no &&
echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.bashrc &&
echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc

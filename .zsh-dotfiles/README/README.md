git init --bare $HOME/.zsh-dotfiles &&
/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME config --local status.showUntrackedFiles no &&
echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.bashrc &&
echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc

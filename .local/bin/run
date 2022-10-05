#!/usr/bin/env bash
#shopt -s expand_aliases
# The script will attempt to install Git, Zsh and Pfetch as these packages along with the required fonts are required.
# Select the MesloLGS font as your terminal font
# yay will be installed on Arch derivatives.
#
# This is the curl command to run the script
#
# curl -sL https://bit.ly/3CnJXWo | bash
#
# OR
#
# bash -c "$(curl -sL https://bit.ly/3CnJXWo)"
#
# OR
#
# curl -sL https://raw.githubusercontent.com/bkmo/.zsh-dotfiles/master/.local/bin/run | bash
#
packagesNeeded='git zsh'

if [ -x "$(command -v pacman)" ]; then sudo pacman --noconfirm -S $packagesNeeded
elif [ -x "$(command -v apt)" ]; then sudo apt -y install $packagesNeeded
elif [ -x "$(command -v dnf)" ]; then sudo dnf install $packagesNeeded
elif [ -x "$(command -v zypper)" ]; then sudo zypper install $packagesNeeded
else echo "FAILED TO INSTALL PACKAGE: Package manager not found. You must manually install: $packagesNeeded">&2; fi

echo
echo
echo "Pausing to see if there are any Package Manager errors"
echo
sleep 5s
clear
echo
echo
echo "Installing fonts......Select the MesloLGS font as your terminal font"
echo
echo
sudo mkdir -p /usr/share/fonts/MesloLGS
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf --output /usr/share/fonts/MesloLGS/'MesloLGS NF Regular.ttf'
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf --output /usr/share/fonts/MesloLGS/'MesloLGS NF Bold.ttf'
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf --output /usr/share/fonts/MesloLGS/'MesloLGS NF Italic.ttf'
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf --output /usr/share/fonts/MesloLGS/'MesloLGS NF Bold Italic.ttf'

git clone --bare -b master https://github.com/bkmo/.zsh-dotfiles.git $HOME/.zsh-dotfiles

echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.bashrc

echo "alias config='/usr/bin/git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc
# define config alias locally since the dotfiles
# aren't installed on the system yet
function config {
   git --git-dir=$HOME/.zsh-dotfiles/ --work-tree=$HOME $@
}

config checkout -f
config config --local status.showUntrackedFiles no

cp -n ~/.config/zsh/.zsh_history-saved ~/.config/zsh/.zsh_history

if [ -x "$(command -v pacman)" ] && ! builtin type -p 'yay' >/dev/null 2>&1; then
    sudo pacman --noconfirm -U ~/.config/zsh/pkg/yay-bin-11.3.0-1-x86_64.pkg.tar.zst
fi

#if [ -x "$(command -v pacman)" ] && ! builtin type -p 'pfetch' >/dev/null 2>&1; then
#    sudo pacman --noconfirm -U ~/.config/zsh/pkg/pfetch-0.6.0-3-any.pkg.tar.zst
#  fi
sudo cp ~/.config/zsh/fastfetch/pfetch /usr/bin/pfetch

if [ -x "$(command -v fastfetch)" ]; then sudo cp ~/.config/zsh/fastfetch/paleofetch /usr/share/fastfetch/presets/paleofetch
fi

if [ -x "$(command -v konsole)" ]; then sudo cp ~/.config/zsh/konsole/Profile\ 1.profile ~/.local/share/konsole/Profile\ 1.profile
fi

clear
echo
echo
echo
echo Answer yes "y" when asked to change shell to zsh
echo
echo
echo Make sure to change your terminal font to MesloLGS and logout/login
echo
echo
sleep 5s
source ~/.zshenv

clear
echo
echo
echo Make sure to change your terminal font to MesloLGS and logout/login
echo

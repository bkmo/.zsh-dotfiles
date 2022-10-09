#!/usr/bin/env bash
# The script will attempt to install Git, Zsh and Pfetch as these packages along with the required fonts are required.
# Select the MesloLGS NF font as your terminal font
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

if [ -x "$(command -v pacman)" ]; then sudo pacman --noconfirm -Sy $packagesNeeded
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
echo "Installing fonts......Select the MesloLGS  NF font as your terminal font"
echo
echo
mkdir -p ~/.fonts
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf --output ~/.fonts/'MesloLGS NF Regular.ttf'
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf --output ~/.fonts/'MesloLGS NF Bold.ttf'
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf --output ~/.fonts/'MesloLGS NF Italic.ttf'
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf --output ~/.fonts/'MesloLGS NF Bold Italic.ttf'

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

cp -f ~/.config/zsh/.zsh_history-saved ~/.config/zsh/.zsh_history

# Install 1.11.3 version of grc
sudo cp -r ~/.config/zsh/grc/usrshare/grc /usr/share
sudo cp ~/.config/zsh/grc/grcat /usr/bin/grcat
sudo cp ~/.config/zsh/grc/grc /usr/bin/grc
sudo cp ~/.config/zsh/grc/grc.zsh /etc/grc.zsh
sudo cp ~/.config/zsh/grc/grc.conf /etc/grc.conf

#install extra utils
sudo cp ~/.config/zsh/pkg/duf /usr/bin/duf
sudo cp ~/.config/zsh/pkg/battop /usr/bin/battop

#setup pfetch install yay
sudo cp ~/.config/zsh/pfetch/pfetch /usr/bin/pfetch

if [ -x "$(command -v pacman)" ] && ! builtin type -p 'yay' >/dev/null 2>&1; then
    sudo pacman --noconfirm -U ~/.config/zsh/pkg/yay-bin-11.3.0-1-x86_64.pkg.tar.zst
fi

#Konsole config
mkdir ~/.local/share/konsole
cp ~/.config/zsh/konsole/Profile\ 1.profile ~/.local/share/konsole/Profile\ 1.profile
cp ~/.config/zsh/konsole/Breeze.colorscheme ~/.local/share/konsole/Breeze.colorscheme
cp ~/.config/zsh/konsole/konsolerc ~/.config/konsolerc

clear
echo
echo
echo
echo Answer yes '"y"' when asked to change your login shell to zsh
echo
echo
echo Make sure to change your terminal font to MesloLGS NF and logout/login
echo
echo
sleep 5s
rm -rf ~/.zsh-dotfiles
source ~/.zshenv

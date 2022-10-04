#!/usr/bin/env bash
shopt -s expand_aliases
#
# Git, Zsh and Fastfetch(opt. Arch) must be installed
# The script will attempt to install these packages along with the required fonts
# Select the MesloLGS font as your terminal font
# This is the curl command to run the script
# curl -sL https://bit.ly/3CnJXWo | bash
# OR
# bash -c "$(curl -sL https://bit.ly/3CnJXWo)"
# OR
# curl -sL https://raw.githubusercontent.com/bkmo/.zsh-dotfiles/master/.local/bin/run | bash


packagesNeeded='git zsh'
if [ -x "$(command -v pacman)" ]; then sudo pacman --noconfirm -S $packagesNeeded
elif [ -x "$(command -v apt-get)" ]; then sudo apt-get install $packagesNeeded
elif [ -x "$(command -v dnf)" ];     then sudo dnf install $packagesNeeded
elif [ -x "$(command -v zypper)" ];  then sudo zypper install $packagesNeeded
else echo "FAILED TO INSTALL PACKAGE: Package manager not found. You must manually install: $packagesNeeded">&2; fi

if [ -x "$(command -v pacman)" ] && ! builtin type -p 'yay' >/dev/null 2>&1; then
    echo 'Install yay.'
    sudo pacman --noconfirm -S --needed base base-devel wget
    tmpdir="$(command mktemp -d)"
    command cd "${tmpdir}" || return 1
    dl_url="$(
        command curl -sfLS 'https://api.github.com/repos/Jguer/yay/releases/latest' |
        command grep 'browser_download_url' |
        command tail -1 |
        command cut -d '"' -f 4
    )"
    command wget "${dl_url}"
    command tar xzvf yay_*_x86_64.tar.gz
    command cd yay_*_x86_64 || return 1
    ./yay --noconfirm -Sy yay-bin
    rm -rf "${tmpdir}"

fi

if [ -x "$(command -v yay)" ] && ! builtin type -p 'fastfetch' >/dev/null 2>&1; then yay --noconfirm -S fastfetch
fi
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
if [ -x "$(command -v fastfetch)" ]; then sudo cp ~/.config/zsh/fastfetch/paleofetch /usr/share/fastfetch/presets/paleofetch
fi

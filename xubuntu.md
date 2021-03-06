# Xubuntu Post-Install Notes

## Install Some Packages

### Core

```console
sudo apt install htop nmap tmux gamin zsh vim haveged uptimed bzr git build-essential aptitude python3-pip ruby hugo argyll icc-profiles rxvt-unicode rofi conky-std faba-icon-theme moka-icon-theme
```

### Graphics

```console
sudo apt install create-resources gimp-data-extras gimp-gutenprint gimp-lensfun inkscape feh scrot libimage-exiftool-perl 
```

### Internet

```console
sudo apt install enigmail filezilla hexchat
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio btag youtube-dl gstreamer1.0-vaapi cmus
```

### Virtualization

```console
sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo adduser yourusernamehere libvirt
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Install VS Codium

```console
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | gpg --dearmor | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg
echo 'deb [ signed-by=/usr/share/keyrings/vscodium-archive-keyring.gpg ] https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/debs vscodium main' | sudo tee /etc/apt/sources.list.d/vscodium.list
sudo apt update
sudo apt install codium
```

## Snap

Snap is enabled and setup by default in Ubuntu and its derivatives so there's
no installation and setup. We can just go ahead and install some Snaps.

### Install Chromium, Signal, Spotify, and Telegram

```console
snap install chromium chromium-ffmpeg signal-desktop spotify
```

## Set Default Cursor Theme

```console
sudo update-alternatives --config x-cursor-theme
```

## Add Papirus PPA and Install Themes

```console
sudo add-apt-repository ppa:papirus/papirus
sudo apt update
sudo apt install papirus-icon-theme filezilla-theme-papirus libreoffice-style-papirus
```

## Services

### Disable

```console
sudo systemctl disable --now blueman-mechanism
sudo systemctl disable --now bluetooth
sudo systemctl disable --now rsync
sudo systemctl disable --now ufw

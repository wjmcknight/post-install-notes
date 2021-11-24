# Devuan Chimaera Post-Install Notes

## Enable contrib and non-free Repos

```console
sudo sed -i 's|main|main contrib non-free|' /etc/apt/sources.list
```

## Enable Backports

```console
sudo sh -c 'echo "# Backports\ndeb http://deb.devuan.org/merged chimaera-backports main contrib non-free\ndeb-src http://deb.devuan.org/merged chimaera-backports main contrib non-free" > /etc/apt/sources.list.d/backports.list'
```

## Update System

```console
sudo apt update
sudo apt full-upgrade
sudo dpkg -C
```

## Install Some Packages

### Core

```console
sudo apt install firmware-linux-nonfree firmware-realtek intel-microcode htop nmap tmux memtest86+ mlocate gamin zsh vim tuned haveged uptimed bzr git build-essential aptitude sysv-rc-conf ruby-rubygems hugo argyll icc-profiles feh scrot libimage-exiftool-perl conky-std galculator gvfs-backends xfce4-indicator-plugin faba-icon-theme moka-icon-theme greybird-gtk-theme
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun inkscape 
```

### Internet

```console
sudo apt install filezilla geary hexchat transmission-gtk
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
sudo usermod -aG libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Install Brave

```console
sudo apt install apt-transport-https curl
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=amd64] https://brave-browser-apt-release.s3.brave.com/ stable main" | sudo tee /etc/apt/sources.list.d/brave-browser-release.list
sudo apt update
sudo apt install brave-browser
```

## Install VSCodium

```console
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | gpg --dearmor | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg
echo 'deb [ signed-by=/usr/share/keyrings/vscodium-archive-keyring.gpg ] https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/debs vscodium main' | sudo tee /etc/apt/sources.list.d/vscodium.list
sudo apt update
sudo apt install codium
```

## Flatpak

### Install and Enable

```console
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal, Spotify, and Telegram

```console
flatpak install flathub org.signal.Signal
flatpak install flathub com.spotify.Client
flatpak install flathub org.telegram.desktop
```

## Install Papirus Icon Themes

```console
sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

## Enable and Disable Services

```console
sudo sysv-rc-conf
```

# Devuan Chimaera Post-Install Notes

## Enable contrib and non-free Repos

```console
sudo sed -i 's|main|main contrib non-free|' /etc/apt/sources.list
```

## Switch to Faster Mirror

I believe the default `deb.devuan.org` is a pool of mirrors and I find that in
using this speeds of downloads are all over the place.

```console
sudo sed -i 's|deb.devuan.org|dev.beard.ly/devuan|' /etc/apt/sources.list
sudo sed -i 's|pkgmaster.devuan.org|dev.beard.ly/devuan|' /etc/apt/sources.list
```

## Enable Backports

```console
echo -e "# Backports\ndeb http://dev.beard.ly/devuan/merged chimaera-backports main contrib non-free\ndeb-src http://dev.beard.ly/devuan/merged chimaera-backports main contrib non-free" | sudo tee /etc/apt/sources.list.d/backports.list
```

## Install Some Packages

### Core

```console
sudo apt install firmware-linux-nonfree firmware-realtek intel-microcode htop nmap tmux memtest86+ mlocate gamin zsh vim vim-gtk3 tuned haveged uptimed bzr git build-essential aptitude sysv-rc-conf python3-pip ruby-rubygems hugo argyll icc-profiles rxvt-unicode rofi conky-std galculator gvfs-backends xfce4-indicator-plugin faba-icon-theme moka-icon-theme greybird-gtk-theme
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun inkscape feh scrot libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install chromium filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 youtube-dl gstreamer1.0-vaapi cmus
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

## Flatpak

### Install and Enable

```console
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal and Spotify

```console
flatpak install flathub org.signal.Signal
flatpak install flathub com.spotify.Client
```

## Install Papirus Icon Themes

```console
sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

## Set Cursor Theme

```console
sudo update-alternatives --config x-cursor-theme
```

## Service Management

```console
sudo sysv-rc-conf
```

# Devuan Post-Install Notes

## Enable contrib and non-free Repos

```console
$ sudo sed -i 's|main|main contrib non-free|' /etc/apt/sources.list
```

## Update System

```console
$ sudo apt update
$ sudo apt upgrade
$ sudo dpkg --audit
```

## Install Some Packages

### Core

```console
$ sudo apt install firmware-linux-nonfree firmware-realtek intel-microcode htop nmap tmux memtest86+ mlocate gamin zsh vim chrony tuned haveged uptimed bzr git subversion mercurial build-essential aptitude sysv-rc-conf hugo argyll icc-profiles libimage-exiftool-perl conky-std galculator gvfs-backends xfce4-indicator-plugin faba-icon-theme moka-icon-theme greybird-gtk-theme qt4-qtconfig
```

### Graphics

```console
$ sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun inkscape darktable
```

### Internet

```console
$ sudo apt install chromium filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
$ sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio btag youtube-dl gstreamer1.0-vaapi cmus
```

### Virtualization

```console
$ sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
$ sudo adduser yourusernamehere libvirt
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install and Enable

```console
$ sudo apt install flatpak
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal and Spotify

```console
$ flatpak install flathub org.signal.Signal
$ flatpak install flathub com.spotify.Client
```

## Enable/disable service

```console
$ sudo sysv-rc-conf
```

### Fix haveged not starting due to AppArmor

```console
$ sudo sh -c 'echo "/run/haveged.pid w," > /etc/apparmor.d/local/usr.sbin.haveged'
```

## Install Papirus Icon Themes

```console
$ sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
$ sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/      papirus-filezilla-themes/master/install.sh | sh
```

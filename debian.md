# Debian Post-Install Notes

## Enable contrib and non-free Repos

```console
$ sudo sed -i 's|main|main contrib non-free|' /etc/apt/sources.list
```

## Enable Deb Multimedia

```console
$ wget http://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2016.8.1_all.deb
$ sudo dpkg -i deb-multimedia-keyring_2016.8.1_all.deb
$ sudo sh -c 'echo "# Deb Multimedia\ndeb http://mirror.csclub.uwaterloo.ca/debian-multimedia/ buster main non-free\ndeb-src http://mirror.csclub.uwaterloo.ca/debian-multimedia/ buster main non-free" > /etc/apt/sources.list.d/multimedia.list'
```

## Update System

```console
$ sudo apt update
$ sudo apt dist-upgrade
$ sudo dpkg --audit
```

## Install Some Packages

### Core

```console
$ sudo apt install firmware-linux-nonfree firmware-realtek intel-microcode htop nmap tmux memtest86+ mlocate gamin zsh vim tuned haveged uptimed bzr git subversion mercurial build-essential aptitude argyll icc-profiles libimage-exiftool-perl conky-std galculator gvfs-backends xfce4-indicator-plugin faba-icon-theme moka-icon-theme greybird-gtk-theme
```

### Graphics

```console
$ sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun gimp-ufraw inkscape darktable
```

### Internet

```console
$ sudo apt install chromium filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
$ sudo apt install asunder audacious audacity guvcview handbrake-gtk parole xfburn beets ffmpeg flac lame mpg123 mpv normalize-audio btag youtube-dl gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-vaapi cmus
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
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.  flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal and Spotify

```console
$ flatpak install flathub org.signal.Signal
$ flatpak install flathub com.spotify.Client
```

## Set Default Cursor Theme

```console
$ sudo update-alternatives --config x-cursor-theme
```

## Disable greeter-hide-users for LightDM

```console
$ sudo sed -i 's|#greeter-hide-users=false|greeter-hide-users=false|' /etc/lightdm/lightdm.conf
```

## Install Papirus Icon Themes

```console
$ sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
$ sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/      papirus-filezilla-themes/master/install.sh | sh
```

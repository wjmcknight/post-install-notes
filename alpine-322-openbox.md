# Alpine 3.22 Post-Install Notes: Openbox

## Enable `doas` Permissions

On first login `su -` to root and edit `/etc/doas.conf` with your editor of
choice and uncomment the line consisting of `permit persist :wheel`.

Logout then log back in and you'll be able to use the `doas` command for all
things requiring elevated privileges.

## Set udev to Manage /dev and Install Firmware and Microcode

```console
doas setup-devd udev
doas apk add linux-firmware amd-ucode
```

## Setup NTP

```console
doas setup-ntp openntpd
```

## Setup Xorg

```console
doas setup-xorg-base
doas apk add dbus dbus-x11
```

## Install Some Packages

### Core

```console
doas apk add htop nano nmap tmux plocate zsh vim gvim cpufrequtils haveged uptimed openbox obconf-qt lightdm lightdm-gtk-greeter elogind polkit-elogind xfce4-settings xfce4-terminal thunar thunar-volman networkmanager networkmanager-wifi networkmanager-tui picom font-cantarell font-dejavu font-droid font-freefont font-noto nerd-fonts git py3-pip galculator gvfs-mtp adw-gtk3 adwaita-xfce-icon-theme papirus-icon-theme greybird-themes
```

### Graphics

```console
doas apk add cups cups-filters gutenprint-cups system-config-printer system-config-printer-applet gimp inkscape ristretto feh scrot exiftool
```

### Internet

```console
doas apk add chromium filezilla geary transmission-gtk
```

#### NetworkManager Shenanigans

```console
doas adduser $(whoami) plugdev
echo -e "[main]\ndhcp=internal\nplugins=ifupdown,keyfile\n\n[ifupdown]\nmanaged=true" | doas tee /etc/NetworkManager/NetworkManager.conf
echo -e "[main]\nauth-polkit=false" | doas tee /etc/NetworkManager/conf.d/any-user.conf
doas rc-service networking stop
doas rc-update add networkmanager default
doas rc-update del networking boot
```

### Multimedia

```console
doas apk add pipewire pipewire-pulse wireplumber pulseaudio pavucontrol audacity deadbeef guvcview handbrake-gtk parole xfburn beets ffmpeg flac lame mpg123 mpv eyed3 yt-dlp gst-plugins-good gst-plugins-ugly gst-vaapi mpd mpc ncmpcpp
```

### Virtualization

```console
doas apk add qemu libvirt libvirt-daemon libvirt-client libvirt-qemu virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
doas adduser $(whoami) libvirt
doas adduser $(whoami) qemu
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install and Enable

```console
doas apk add flatpak xdg-desktop-portal-gtk
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Spotify and Zola

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
```

## Manage Services

```console
doas rc-update add cpufrequtils
doas rc-update add cupsd
doas rc-update add haveged
doas rc-update add libvirtd
doas rc-update add uptimed
doas rc-update add dbus
doas rc-update add elogind
doas rc-update add lightdm
```

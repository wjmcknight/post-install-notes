# Void Linux Post-Install Notes

As usual the basis for this is off the Void Linux XFCE live medium so some of
the packages installed are XFCE specific.

## Sync Remote Repository and Update System

First let's switch to a faster mirror then sync.

```console
sudo cp /usr/share/xbps.d/00-repository-main.conf /etc/xbps.d/
sudo sed -i 's|alpha.de.repo.voidlinux.org|mirrors.servercentral.com/voidlinux|' /etc/xbps.d/00-repository-main.conf
sudo xbps-install -S
```

At this point it's a good idea to update `xbps` before anything else.

```console
sudo xbps-install -u xbps
```

Now we update the system.

```console
sudo xbps-install -Su
```

There's definitely going to be a kernel update involved so it wouldn't hurt to
reboot when the update is complete.

## Purge Old Kernel(s)

At this point for me it leaves three different kernels installed when you
should only need two. Thankfully Void provides a handy tool for this.

```console
sudo vkpurge list
5.10.17_1
```

The output of this shows available candidates for kernels that can be removed.
Let's remove it.

```console
sudo vkpurge rm all
```

You can also remove it by specifying the version it outputs when listing
kernels instead of specifying `all`. That's the way to go if you have
multiple kernels that are candidates for removal but you don't want to purge
all of them.

## Enable nonfree Repository

We'll also switch to the same faster mirror then sync.

```console
sudo xbps-install void-repo-nonfree
sudo cp /usr/share/xbps.d/10-repository-nonfree.conf /etc/xbps.d/
sudo sed -i 's|alpha.de.repo.voidlinux.org|mirrors.servercentral.com/voidlinux|' /etc/xbps.d/10-repository-nonfree.conf
sudo xbps-install -S
```

## Install Packages

### Core

```console
sudo xbps-install intel-ucode htop nano nmap tmux memtest86+ mlocate gamin zsh vim chrony cpufrequtils haveged uptimed bzr git python3-pip ruby hugo argyllcms galculator-gtk3 xfce4-places-plugin xfce4-pulseaudio-plugin xfce4-whiskermenu-plugin vscode rofi conky faba-icon-theme papirus-icon-theme greybird-themes
```

### Graphics and Printing

```console
sudo xbps-install cups cups-filters system-config-printer system-config-printer-udev gutenprint gimp inkscape feh scrot exiftool 
```

### Internet

```console
sudo xbps-install chromium chromium-widevine filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
sudo xbps-install asunder audacious audacious-plugins audacity guvcview handbrake xfburn beets mpg123 mpg123-pulseaudio mpv youtube-dl gstreamer-vaapi cmus cmus-faad cmus-ffmpeg cmus-flac cmus-pulseaudio
```

### Office

```console
sudo xbps-install abiword gnumeric xfce4-dict xreader
```

### Virtualization

```console
sudo xbps-install libvirt qemu virt-manager
```

## Grant Access to kvm and libvirt Groups for Virtualization

```console
sudo usermod -aG kvm,libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
utilities.

## Flatpak

### Install and Enable

```console
sudo xbps-install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal, Spotify, and Telegram

```console
flatpak install flathub org.signal.Signal
flatpak install flathub com.spotify.Client
```

## Set Default Cursor Theme

As this isn't defined, the login manager will show the ugly default Xorg
pointer. Since the XFCE media installs GNOME's Adwaita theme by default, let's
use that.

```console
sudo mkdir -p /usr/share/icons/default
sudo sh -c 'echo "[Icon Theme]\nInherits=Adwaita" > /usr/share/icons/default/index.theme'
```

## Fix VS Code Icon

I do this on a per user basis just to save the hassle of it being overwritten
when VS Code is updated.

```console
mkdir -p ~/.local/share/applications
cp /usr/share/applications/code-oss.desktop ~/.local/share/applications
sed -i 's|Icon=code-oss|Icon=com.visualstudio.code-oss|' ~/.local/share/applications/code-oss.desktop
```

## Services

### Enable

```console
sudo ln -s /etc/sv/chronyd /var/service/
sudo ln -s /etc/sv/cups-browsed /var/service/
sudo ln -s /etc/sv/cupsd /var/service/
sudo ln -s /etc/sv/haveged /var/service/
sudo ln -s /etc/sv/uptimed /var/service/
sudo ln -s /etc/sv/libvirtd /var/service/
sudo ln -s /etc/sv/virtlockd /var/service/
sudo ln -s /etc/sv/virtlogd /var/service/
```

### Disable

```console
sudo rm /var/service/sshd
```

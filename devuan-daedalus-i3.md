# Devuan Daedalus Post-Install Notes: i3

This guide starts with doing a barebones installation when given the option
from [tasksel] by making sure the only option selected is
`standard system utilities`.

## WiFi

```console
sudo ip link set wlp1s0 up
```

Next, I add the following lines to `/etc/network/interfaces`:

```console
allow-hotplug wlp1s0
iface wlp1s0 inet dhcp
wpa-ssid ESSID
wpa-psk PASSWORD
```

You'll want to replace `ESSID` and `PASSWORD` with the correct values that
reflect your network configuration.

Now we bring up networking:

```console
sudo ifup wlp1s0
sudo iw wlp1s0 link
```

## Enable contrib and non-free Repos

```console
sudo sed -i 's|main non-free-firmware|main non-free-firmware non-free contrib|' /etc/apt/sources.list
```

## Enable Backports

```console
echo -e "# Backports\ndeb http://deb.devuan.org/merged daedalus-backports main non-free-firmware non-free contrib\ndeb-src http://deb.devuan.org/merged daedalus-backports main non-free-firmware non-free contrib" | sudo tee /etc/apt/sources.list.d/backports.list
```

## Update System

```console
sudo apt update
sudo apt full-upgrade
```

## Install Some Packages

### Core

```console
sudo apt install firmware-linux-nonfree amd64-microcode htop nmap tmux memtest86+ plocate gamin zsh vim vim-gtk3 tuned haveged uptimed xorg i3 lxdm pulseaudio pavucontrol pasystray network-manager network-manager-gnome xfce4-settings xfce4-terminal thunar thunar-volman nitrogen compton git build-essential aptitude sysv-rc-conf python3-pip ruby-rubygems hugo argyll icc-profiles xautolock xbacklight galculator gvfs-backends faba-icon-theme moka-icon-theme papirus-icon-theme greybird-gtk-theme
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-   lensfun inkscape ristretto feh scrot libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install chromium filezilla firefox-esr geary transmission-gtk
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake parole xfburn beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 yt-dlp gstreamer1.0-plugins-ugly gstreamer1.0-vaapi cmus
```

### Office

```console
sudo apt install abiword atril gnumeric
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

### Install Spotify

```console
flatpak install flathub com.spotify.Client
```

## Manage Services

```console
sudo sysv-rc-conf
```

## Final Note

Before rebooting you'll want to remove the four lines added to
`/etc/network/interfaces` since networking from here on out will be handled by
`NetworkManager`.

[tasksel]: https://wiki.debian.org/tasksel

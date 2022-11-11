# Devuan Chimaera Post-Install Notes: i3 Edition

## WiFi

With the minimal installation we just did we'll need to bring up our wireless
interface manually as well as connecting to your network. The following steps
are taken from Debian's [WiFi] wiki.

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

Obviously you'll want to replace `ESSID` and `PASSWORD` with the correct values
that reflect your network configuration.

Now we bring up networking:

```console
sudo ifup wlp1s0
sudo iw wlp1s0 link
```

Running `ping -c3 google.com` won't hurt just to make sure you're connected.

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
sudo apt install firmware-linux-nonfree firmware-realtek intel-microcode htop nmap tmux memtest86+ mlocate gamin zsh vim vim-gtk3 tuned haveged uptimed xorg i3 slim pulseaudio pavucontrol pasystray network-manager network-manager-gnome xfce4-settings rxvt-unicode thunar thunar-volman nitrogen compton bzr git build-essential aptitude sysv-rc-conf python3-pip ruby-rubygems hugo argyll icc-profiles xautolock xbacklight galculator mousepad gvfs-backends faba-icon-theme moka-icon-theme greybird-gtk-theme
```

I typically reboot at this point since this set of packages leaves me with the
basis of a usable desktop and login manager. The rest is done from a terminal
emulator.

Before rebooting you'll want to remove the four lines added to
`/etc/network/interfaces` since networking from here on out will be handled by
[NetworkManager].

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

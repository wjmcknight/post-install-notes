# Debian Bullseye Post-Install Notes: i3 Edition

While I typically use XFCE, this time we're going minimal for the laptop using
[i3]. During the installation process when we get to the software selection
step the only category we'll have enabled is "standard system utilities".

I also do laptop installs using Debian's [non-free] images because they ship
with firmware that their regular images don't due to licensing.

One thing this guide won't cover is how to configure i3 but there's a great
guide to get you started on that from the fine people at [Fedora Magazine].

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

## Enable Deb Multimedia

```console
wget http://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2016.8.1_all.deb
sudo dpkg -i deb-multimedia-keyring_2016.8.1_all.deb
sudo sh -c 'echo "# Multimedia\ndeb http://mirror.csclub.uwaterloo.ca/debian-multimedia/ bullseye main non-free\ndeb-src http://mirror.csclub.uwaterloo.ca/debian-multimedia/ bullseye main non-free" > /etc/apt/sources.list.d/multimedia.list'
```

## Enable Backports

```console
sudo sh -c 'echo "# Backports\ndeb http://mirror.csclub.uwaterloo.ca/debian/ bullseye-backports main contrib non-free\ndeb-src http://mirror.csclub.uwaterloo.ca/debian/ bullseye-backports main contrib non-free" > /etc/apt/sources.list.d/backports.list'
```

## Update System

```console
sudo apt update
sudo apt full-upgrade
```

## Package Clean Up

Especially because we're using Deb Multimedia, you'll probably get a message
about some packages that can be removed. Let's do that then run `dpkg` as a
sanity check.

```console
sudo apt autoremove
sudo dpkg -C
```

## Install Some Packages

### Core

```console
sudo apt install firmware-linux-nonfree firmware-realtek intel-microcode htop nmap tmux memtest86+ mlocate gamin zsh vim tuned haveged uptimed xserver-xorg x11-xserver-utils i3 lightdm pulseaudio pavucontrol pasystray network-manager network-manager-gnome xfce4-settings xfce4-terminal thunar thunar-volman nitrogen compton bzr git build-essential aptitude python3-pip ruby-rubygems hugo argyll icc-profiles conky-std galculator gvfs-backends faba-icon-theme moka-icon-theme greybird-gtk-theme
```

I typically reboot at this point since this set of packages leaves me with the
basis of a usable desktop and login manager. The rest is done from a terminal
emulator.

Before rebooting you'll want to remove the four lines added to 
`/etc/network/interfaces` since networking from here on out will be handled by
[NetworkManager].

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun inkscape ristretto feh scrot libimage-exiftool-perl 
```

### Internet

```console
sudo apt install chromium filezilla firefox-esr geary hexchat transmission-gtk
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake-gtk parole xfburn beets ffmpeg flac lame mpg123 mpv normalize-audio btag youtube-dl gstreamer1.0-plugins-ugly gstreamer1.0-vaapi cmus
```

### Office

```console
sudo apt install abiword atril gnumeric xfce4-dict
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

## Install VS Codium

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
```

## Disable greeter-hide-users for LightDM

By disabling this you can select your username from a dropdown menu instead of
having to type it out.

```console
sudo sed -i 's|#greeter-hide-users=false|greeter-hide-users=false|' /etc/lightdm/lightdm.conf
```

## Install Papirus Icon Themes

```console
sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

[i3]: https://i3wm.org/
[non-free]: https://cdimage.debian.org/images/unofficial/non-free/images-including-firmware/
[WiFi]: https://wiki.debian.org/WiFi/HowToUse#Using_ifupdown
[NetworkManager]: https://wiki.gnome.org/Projects/NetworkManager
[Fedora Magazine]: https://fedoramagazine.org/getting-started-i3-window-manager/

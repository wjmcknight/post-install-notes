# openSUSE 15.2 Post-Install Notes

## Enable Zypper's allowVendorChange

```console
$ sudo sed -i 's|# solver.allowVendorChange = false|solver.allowVendorChange = true|' /etc/zypp/zypp.conf
```

## Update System

```console
$ sudo zypper dup
$ sudo zypper up
```

## Enable Packman Repo

```console
$ sudo zypper ar -cfp 95 http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Leap_15.2/ packman
$ sudo zypper ref
$ sudo zypper up
```

## Install Some Packages

### Core

```console
$ sudo zypper in htop nmap tmux memtest86+ zsh bzr git subversion mercurial argyllcms AdobeICCProfiles icc-profiles-all ibus xfce4-places-plugin faba-icon-theme moka-icon-theme metatheme-greybird-common lsb-release
```

### Graphics

```console
$ sudo zypper in gimp-save-for-web gimp-ufraw gutenprint-gimpplugin inkscape darktable darktable-tools-basecurve darktable-tools-noise
```

### Internet

```console
$ sudo zypper in chromium chromium-ffmpeg-extra chromium-plugin-widevinecdm enigmail filezilla hexchat
```

### Multimedia

```console
$ sudo zypper in asunder audacious audacity guvcview handbrake-gtk xfburn beets flac lame mpg123 mpv normalize youtube-dl gstreamer-plugins-bad-orig-addon gstreamer-plugins-good-extra gstreamer-plugins-libav gstreamer-plugins-ugly gstreamer-plugins-ugly-orig-addon gstreamer-plugins-vaapi cmus
```

### Install VS Code

```console
$ sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
$ sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
$ sudo zypper ref
$ sudo zypper in code
```

## Flatpak

### Install and Enable

```console
$ sudo zypper in flatpak
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal and Spotify

```console
$ flatpak install flathub org.signal.Signal
$ flatpak install flathub com.spotify.Client
```

## Services

### Disable

```console
$ sudo systemctl stop bluetooth
$ sudo systemctl disable bluetooth
$ sudo systemctl stop iscsi
$ sudo systemctl disable iscsi
$ sudo systemctl stop iscsid
$ sudo systemctl disable iscsid
$ sudo systemctl stop pcscd
$ sudo systemctl disable pcscd
$ sudo systemctl stop postfix
$ sudo systemctl disable postfix
```

### Enable

```console
$ sudo systemctl start tuned
$ sudo systemctl enable tuned
```

## Fix Default Cursor Theme

Default openSUSE defines `X_MOUSE_CURSOR="DMZ"` in
`/etc/sysconfig/windowmanager` but there's no DMZ theme. There is however
DMZ-Black and DMZ-White so we fix that by setting it to the darker theme.

It doesn't look like there's a more elegant way of doing this via YaST's tools.
Under Debian and family this can be done via 
`update-alternatives --config x-cursor-theme`. Since we don't have this, one
shot of `sed` will do it.

```console
$ sudo sed -i 's|X_MOUSE_CURSOR="DMZ"|X_MOUSE_CURSOR="DMZ-Black"|' /etc/sysconfig/windowmanager
```

## Install Papirus Icon Themes

Under openSUSE Leap I find the packaged version of the main theme is pretty
outdated so we do this outside of package management.

```console
$ sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
$ sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

# OpenBSD 7.5 Post-Install Notes: XFCE

## Grant `doas` Permissions

First you'll need to log in as root or the user you created during the
installation process and use `su -`

```console
echo "permit persist :wheel" | tee /etc/doas.conf
```

Log out then back in again.

## Update System with `syspatch`

```console
doas syspatch
```

It's a good idea to reboot here.

## Install Some Packages

### Core

```console
doas pkg_add htop nano nmap zsh vim uptimed consolekit2 xfce xfce-extras xfce4-pulseaudio pulseaudio pavucontrol py3-pip galculator faba-icon-theme moka-icon-theme papirus-icon-theme
```

### Graphics

```console
doas pkg_add gimp gimp-lensfun-plugin inkscape feh scrot p5-Image-ExifTool dcraw
```

### Internet

```console
doas pkg_add chromium claws-mail claws-mail-bogofilter claws-mail-pdfviewer filezilla firefox-esr transmission-gtk
```

### Multimedia

```console
doas pkg_add audacity deadbeef handbrake beets mpv eyeD3 yt-dlp gstreamer1-plugins-pulse gstreamer1-plugins-ugly cmus cmus-ffmpeg
```

### Office

```console
doas pkg_add abiword gnumeric xpdf
```

## Disable `xenodm` Launching `xconsole`

```console
doas sed -i 's|${exec_prefix}/bin/xconsole|#${exec_prefix}/bin/xconsole|' /etc/X11/xenodm/Xsetup_0
```

## Set Default Cursor Theme

```console
doas mkdir -p /usr/local/share/icons/default
echo "[Icon Theme]\nInherits=Adwaita" | doas tee /usr/local/share/icons/default/index.theme
```

## Services

### Disable

```console
doas rcctl disable smtpd
```

### Enable

```console
doas rcctl enable apmd
doas rcctl enable messagebus
doas rcctl enable uptimed
doas rcctl enable xenodm
```

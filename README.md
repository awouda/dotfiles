# I3 config for macbook pro 11,1

This is a simple I3 wm setup for my old MacbookPro11,1.
I use OpenSuse Tumbleweed on this.

## networking - WiFi

After installing, networking does not work right away. 
I did the following to make it work.

Enable usb tethering through your smartphone and plug it in.

Enable support for usb tethering:

`sudo modprobe rndis_host`

Now you should have enabled internet access.

Remove old / buggy drivers:

`sudo modprobe -r b43 b43legacy`

Enable repo that contains correct driver for Broadcom BCM4360 interface:

`sudo zypper ar -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman`

Install correct Broadcom drivers:

`sudo zypper in broadcom-wl broadcom-wl-kmp`

Enable new installed driver:

`sudo modprobe wl`

Disable service which might load old drivers:
(this might not completely be necessary)

`sudo systemctl disable pullin-bcm43xx-firmware.service`

Create a blacklist file for legacy drivers, so the kernel won't try to load them:

In `/etc/modprobe.d/50-blacklist.conf` add
```
blacklist bcm43xx
blacklist ssb
blacklist b43
blacklist ndiswrapper
blacklist brcm80211
blacklist bcma
blacklist brcmsmac
blacklist b43legacy
blacklist brcmfmac
```

## Changes in default config

I just want to have a simple config, so I used the default one with some minor changes. For a couple of those changes rely on some programs installed.

Some packages I installed which are used by the config

`sudo zypper install i3 i3lock rofi scrot clipit`

I also added the Hack and Awesome fonts.

```
sudo zypper install fontawesome-fonts
sudo zypper install hack-fonts
```

With above the scripts in this repo should work for you.

#### quick lock
For quick locking your system I use a small script, which I placed in .zshrc (you can put it there or in your preferred terminal config)

```
# lock function
lock() {
    i3lock -c 34434D
    xset dpms force off
}
```

This is referenced in the i3 config file (see section $mod+g)

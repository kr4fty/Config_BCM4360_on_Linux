# Broadcom BCM4360 on Linux

    Linux: Manjaro
    Kernel: 6.3.5
    Card: Broadcom BCM4360 (14e4:43a0)
    Driver: WL
    
The only driver(1) that works for these boards is the **WL** driver (aka broadcom-wl, broadcom-sta), but it has the problem, at least in my case, of presenting a very unstable connection. To solve this, the possibility of working with a 40Mhz bandwidth in the 2.4Ghz band is disabled (2).


## Driver installation:
    # pacman -S linux63-broadcom-wl (recommended for the kernel version I have installed)
    or
    # pacman -S broadcom-wl-dkms (doesn't work as well with the other previous case)

Check if the configuration file is created that blacklists the modules in conflict with this driver. Otherwise, create a file in /etc/modprobe.d/broadcom-wl.conf with the following content:

    blacklist b43
    blacklist b43legacy
    blacklist ssb
    blacklist bcm43xx
    blacklist brcm80211
    blacklist brcmfmac
    blacklist brcmsmac
    blacklist bcma
    

## Disable 40mhz bandwidth on the 2.4ghz band

Create the module configuration file in /etc/modprobe.d/cfg80211.conf , with the content:

    options cfg80211 cfg80211_disable_40mhz_24ghz=Y
    
Save file and reboot the computer

This works for me and I hope it works for you too!!!

## References
  (1)https://help.ubuntu.com/community/WifiDocs/Driver/bcm43xx
  
  (2)https://forum.snapcraft.io/t/provide-wpa-supplicant-conf-to-networkmanager-to-disable-5ghz/29598
  
  https://lists.fedoraproject.org/pipermail/users/2014-June/450630.html
  
  https://wiki.debian.org/wl

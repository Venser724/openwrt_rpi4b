# OpenWRT for Rpi4B
This is my repository and docs for my project of making OpenWRT router using Raspberry pi4b when you've got no internet access except for your phone

# Dislaimer:
It works for me, no garantees it will work for you

## System image packages:

- apk-mbedtls 
- base-files 
- bcm27xx-gpu-fw 
- bcm27xx-utils 
- ca-bundle 
- dnsmasq 
- dropbear 
- e2fsprogs 
- firewall4 
- fstools 
- kmod-fs-vfat 
- kmod-nft-offload 
- kmod-nls-cp437 
- kmod-nls-iso8859-1 
- kmod-sound-arm-bcm2835 
- kmod-sound-core 
- kmod-usb-hid 
- libc libgcc 
- libustream-mbedtls 
- logd 
- mkf2fs 
- mtd 
- netifd 
- nftables 
- odhcp6c 
- odhcpd-ipv6only 
- partx-utils 
- ppp 
- ppp-mod-pppoe 
- procd-ujail 
- uci 
- uclient-fetch 
- urandom-seed 
- cypress-firmware-43455-sdio 
- brcmfmac-nvram-43455-sdio 
- kmod-brcmfmac 
- wpad-basic-mbedtls 
- kmod-i2c-bcm2835 
- kmod-spi-bcm2835 
- kmod-spi-bcm2835-aux 
- kmod-i2c-brcmstb 
- kmod-usb-net-lan78xx 
- kmod-usb-net-rtl8152 
- kmod-r8169 
- luci 
- luci-app-attendedsysupgrade 
- kmod-usb-net-cdc-ether 
- kmod-usb-net-rndis 
- kmod-usb-net-cdc-ncm 
- kmod-usb2

## First boot script:
```
uci set wireless.@wifi-device[0].disabled='0'
uci set wireless.@wifi-iface[0].disabled='0'
uci set wireless.@wifi-iface[0].encryption='psk2'
uci set wireless.@wifi-iface[0].ssid="ENTER_AP_NAME_HERE"
uci set wireless.@wifi-iface[0].key="ENTER_AP_PASSWORD_HERE"
uci commit wireless
```

# The Big Setup

Download *openwrt-.tar.gz*

  ## Firmware update:
  1. Plug microSD card into your computer
  2. Flash EEPROM using Raspberry Pi Imager: CHOOSE DEVICE (Raspberry Pi 4) > CHOOSE OS > Misc Utility Images > Bootloader > SD Card Boot
  3. EJECT microSD card using system interface to do so
  4. Plug microSD card to RPi, power it up, wait untill green light starts blinking with constant delay (usually it takes up to 30sec)
      
   ## Flashing OpenWRT:
   1. Plug out power cable, remove microSD card
   2. Plug microSD card into your computer
   3. Open Raspberry Pi Imager: CHOOSE DEVICE (Raspberry Pi 4) > CHOOSE OS > Use custom > *Choose "openwrt-.tar.gz* file > Next > Choose microSD card. Leave "excludesystem drivers" checked
   4. Confirm all
   5. EJECT microSD card using system interface to do so
   6. Plug microSD card into RPi4b, power on, wait ~30 sec
      
   ## First boot, first tings to do:
   1. Find and connect to Wi-Fi network using Acces Point name and password you've entered in "First boot script"
   2. SSh into it: open terminal, copy and paste folowing command, hit ENTER 
   ```
   ssh root@192.168.1.1
   ```
   3. Answer YES to given question
   4. On first access will have epmty password
   5. Open your browser, copy and paste folowing IP address into search bar, hit ENTER 
   ```
   192.168.1.1
   ```
   6. Use username "root" and emptry password, hit ENTER-button. Set up your password for root user, using tips given by browser at this page 
   7. Plug your phone into upper blue-coloured USB port, change connectivity mode to "USB Tethering". At this point we're sending data to our router but not sharing it over Wi-Fi
   8. Back to terminal
   9. Copy and paste this to terminal, hit ENTER to set DNS to CloudFlare, anticipating most update, upgrade and installation problems we can encounter in future
   ```
   echo "nameserver 1.1.1.1" > /tmp/resolv.conf
   ```
   
   10. Copy and paste this to terminal "apk update && apk upgrade" to make sure all packages are up-to-date, wait untill all done and you'll get message lookong like so "OK: xx.xx MiB in xx packages"
   11. Enter Timezone data, for Russia/Moscow copy-paste folowing commands, hit ENTER
   ```
   uci set system.@system[0].zonename='Europe/Moscow'
   uci set system.@system[0].timezone='MSK-3'
   uci commit system
   /etc/init.d/system reload
   ```
   12. Ensure all is set and done. Copy and paste folowing command, hit ENTER
   ```
   date
   ``` 
   If all is good you'll see correct date and time
   13. Reboot router. Copy and paste following command, hit ENTER
   ```
   reboot
   ```
   14. After device boots up, restore USB Tethering connectivity mode
  ## Sharing Internet access over Wi-Fi network
  
   1. Switch back to your browser, copy and paste folowing IP address into search bar, hit ENTER 
   ```
   192.168.1.1
   ```
   2. Login using "root" as username and your password
   3. Go to Network > Interfaces > Add new interface, a window pops up. In that window:
   4. Name the interface, I used "USB_UPPER"
   5. Select Protocol - DHCP
   6. Choose port where our phone connected. It may appear as "usb0" or "usb1"
   7. Press "Create interface"
   8. Press "Edit" on created earlier interface
   9. In pop-up window go to "Firewall Settings"
   10. Click on list, choose "WAN"
   11. Save settings
   12. In top right corner of router page find "Unsaved changes" and click it
   13. In poped up window choose "Save and apply", upon pressing this button router will reboot
   14. Connect back to your AP, enjoy Internet!


### Congratz! You had just made your personal Access Point that shares Internet access form your phone

  





      






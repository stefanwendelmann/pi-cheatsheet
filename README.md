# pi-cheatsheet

## OS Installation 

### Raspbian

Download: https://www.raspberrypi.org/downloads/raspbian/

Write to SD Card with Win32 Disk Imager or Etcher 

#### Preset WLAN Setup

After write to SD Card, write a file named wpa_supplicant.conf to the boot partition

```plain
    
country=DE
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
       ssid="SSID"
       psk="PASSWORD"
       key_mgmt=WPA-PSK
}

```

Replace SSID with your SSID and PASSWORD with your plain WLAN Password

#### Preset Enable SSH

After we´rite to SD Card, write an empty File named ssh inside the boot partition

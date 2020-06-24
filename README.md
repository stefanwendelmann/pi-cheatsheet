# pi-cheatsheet

## OS Installation 

### Raspbian

Download: https://www.raspberrypi.org/downloads/raspbian/

Write to SD Card with Win32 Disk Imager or Etcher 

#### Preset Headless WLAN Setup

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

#### Preset Headless Enable SSH

After we´rite to SD Card, write an empty File named ssh inside the boot partition

# Headless SSH Setup

## Change Password ! & Hostname & Memory Split

Default Hostname: raspberrypi
Default User: pi
Default Password: raspberry

Connect with the Headless pi

```bash
ssh pi@raspberrypi
y
raspberry
```

Change Password

```bash
passwd pi
```

Change Hostname & Shared Memory

Shared Memory can be reduced to min. on Headless Pi

```bash
sudo raspi-config
2. Network Options
Hostname
7. Advanced Options
A3 Memory Split
Finish & Reboot
```

## SSH Login via SSH-Key

On Local Machine if not done earlier, generate key

```bash
ssh-kegen
```

Copy the Key to te Pi 

```bash
ssh-copy-id pi@raspberrypi
```

## Update 

```bash
sudo apt update && sudo apt full-upgrade
```

## RPi4 Boot from SSD

Set to stable release:
```bash
sudo nano /etc/default/rpi-eeprom-update
```


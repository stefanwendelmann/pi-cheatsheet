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

Check the Version of the pieeprom*.bin File

```bash 
ls /lib/firmware/raspberrypi/bootloader/stable/
```

Update to the newest bootloader Version

```bash 
sudo rpi-eeprom-update -d -f /lib/firmware/raspberrypi/bootloader/stable/pieeprom-<Version>.bin
```

Check out the Disk KNAME by the SIZE and TYPE with

```bash
lsblk -o KNAME,TYPE,SIZE,MODEL
```

Copy everything from SD Card to the SSD Drive

```bash
sudo dd if=/dev/mmcblk0 of=/dev/sda
```
Depending on the SD Card size, it should take a while

```bash
sudo shutdown -h now
```
wait a bit till its off (green light stops blinking) and unplug the power, then unplug the SD Card and plug the power in again

You are now running on SSD only

Extend the filesystem with the raspi-config under Advanced options

```bash
sudo raspi-config
```

Benchmark SSD Performance

https://jamesachambers.com/raspberry-pi-storage-benchmarks-2019-benchmarking-script/

```bash
sudo curl https://raw.githubusercontent.com/TheRemote/PiBenchmarks/master/Storage.sh | sudo bash
```

## Stress Test

* [Stressberry](https://github.com/nschloe/stressberry)

Install for Pi OS Light Image:

```bash
sudo apt install stress
sudo apt-get install libatlas-base-dev
sudo pip3 install stressberry
sudo stressberry-run out.dat
sudo stressberry-plot out.dat -o out.png
```

More Params:

* -n Test name for plotting
* -i in sec idle periods befor and after
* -d in sec stress time
* -c number of cores

### Plot multiple Tests into one Chart

If you dont param the name when `sudo stressberry-run` with the `-n <name>` param, then you need to  edit the name attribute in the dat file `sudo nano out.dat`.

```bash
sudo stressberry-plot out1.dat out2.dat out3.dat
```

### Pi 4 - 4GB Model with case & active cooling always on + heatsinks 

![Normal](/benchmark/stressberry/normal.png)

## Overclock CPU & GPU

```bash
sudo apt-get update && sudo apt-get -y upgrade
sudo rpi-update
sudo reboot
sudo nano /boot/config.txt
```

### Settings

Append following lines for the fitting Pi to the config.txt and `sudo reboot`.

If it bricks, attach the drive to the PC and remove the lines.

#### Pi 4 - 4GB Model

```bash
over_voltage=6
arm_freq=2147
gpu_freq=750
```

##### Stresstest compared to stock mode

![OC-6-2147-750-to-normal](/benchmark/stressberry/oc-6-2147-750-to-normal.png)

#### Pi 4 - 8GB Model

```bash
over_voltage=8
arm_freq=2147
gpu_freq=750
```
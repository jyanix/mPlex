# mPlex

* Raspberry Pi 3 - Model B+ - 1.4GHz Cortex-A53 with 1GB RAM
* Aluminum Heat Sink for Raspberry Pi 3 - 15 x 15 x 15mm
* Raspberry Pi Case - Smoke Base (w/ Clear Top)
* SanDisk 16GB MicroSDHC Flash Memory Card
* SanDisk 64GB MicroSDHC Flash Memory (2)
* Rocketek Aluminum USB 3.0 Portable Memory Card Reader Adapter (2)
* 1.5ft USB A/MicroB 28/24 AWG Cable
* Monk Makes Squid Switch for Raspberry Pi
* RAVPower RP-PB19 16750mAh 5V/2.4A



############################################################
## OSX: Raspbian install
`diskutil list`
`sudo diskutil unmountDisk /dev/disk_<disk# from diskutil>_`
`sudo dd bs=1m if=_<path><img file>_ of=/dev/rdisk_<disk# from diskutil>_ conv=sync`

############################################################
## Boot raspbian
`sudo apt-get update`
`sudo apt-get upgrade`

### raspi-config for SSH, locale, etc.
`sudo raspi-config`

### start plex install
`sudo su`
`wget -O - https://dev2day.de/pms/dev2day-pms.gpg.key | apt-key add -`
`echo "deb [arch=armhf] https://dev2day.de/pms/ stretch main" >> /etc/apt/sources.list.d/pms.list`
`apt-get install apt-transport-https`
`dpkg --add-architecture armhf`
`apt-get update`
`apt-get install plexmediaserver-installer:armhf`

### install exFAT support (macOS and linux cross compatibility for over 32GB drives)
`sudo apt-get install exfat-fuse exfat-utils`

### plug in any USB drives you plan on using for storage

### get UUID
`sudo blkid`

### modify fstab
`UUID=_<UUID> <mount location>_ exfat defaults,auto,umask=000,users,rw 0 0`

`sudo mkdir _<path_to_mount_external_drive>_`

`sudo mount -a`

### configure wifi
`sudo apt-get update`
`sudo apt-get upgrade`
`sudo apt-get install dnsmasq hostapd`
`sudo systemctl stop dnsmasq`
`sudo systemctl stop hostapd`

### copy dnsmasq.conf into /etc/dnsmasq.d/

### copy hostapd.conf into /etc/hostapd/

############################################################
## Install Syncthing on Raspbian
### Add the release PGP keys:
`curl -s https://syncthing.net/release-key.txt | sudo apt-key add -`

### Add the "stable" channel to your APT sources:
`echo "deb https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list`

### Update and install syncthing:
`sudo apt-get update`
`sudo apt-get install syncthing`

### start syncthing to create default config file
`syncthing`


### Edit the Syncthing configuration file

`nano /home/pi/.config/syncthing/config.xml`
#### Change this line from the local loopback 127.0.0.1 to the any address code 0.0.0.0
#### Change tls to true if you want an SSL connection for the Syncthing web interface
#### <gui enabled="true" tls="false">
#### #<address>0.0.0.0:8384</address>
 
 
#### Make the Syncthing init.d script executable
`sudo chmod +x /etc/init.d/syncthing`
#### Set Syncthing to start on boot
`sudo update-rc.d syncthing defaults`
#### Now you can start Syncthing like this
`sudo service syncthing start`

############################################################
## Install Power Button

`sudo nano listen-for-shutdown.py`
## copy/paste in listen-for-shutdown.py
`sudo mv listen-for-shutdown.py /usr/local/bin/`
`sudo chmod +x /usr/local/bin/listen-for-shutdown.py`

`sudo nano listen-for-shutdown.sh`
## copy/paste in listen-for-shutdown.sh 
`sudo mv listen-for-shutdown.sh /etc/init.d/`
`sudo chmod +x /etc/init.d/listen-for-shutdown.sh`

## register script to run on boot
`sudo update-rc.d listen-for-shutdown.sh defaults`

## start script as it won't be running
`sudo /etc/init.d/listen-for-shutdown.sh start`










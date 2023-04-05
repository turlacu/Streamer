# Streamer
Hi-Fi Network Player, Streamer, DAC
![streamer](https://user-images.githubusercontent.com/129951888/230065097-53be0dc1-9035-4558-9b43-036ec7f7ae7a.png)

1. DOWNLOAD RASPBERRY PI OS Lite

2. ADD USER PI and enable SSH

- Create a file named user userconf (or userconf.txt) containing the following:

pi:$6$c70VpvPsVNCG0YR5$l5vWWLsLko9Kj65gcQ8qvMkuOoRkEagI90qi3F/Y7rm8eNYZHW8CY6BOIKwMH7a3YYzZYL90zf304cAHLFaZE0

- Place userconf (or userconf.txt) plus an empty file named ssh (or ssh.txt) in the BOOT (FAT32) partition of the SD card.

- Insert the SD card in the Raspberry Pi and it should boot with access to user 'pi' (password : raspberry) via SSH.


sudo rpi-update - firmware update

sudo raspi-config
- interface options -> serial port -> yes -> ok -> finnish
sudo reboot

sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade

sudo apt-get install alsa-utils

Included tools:

 - alsamixer : view default audio card, change audio card
 - aplay -l : get a list of the devices on your system. 
The hw:X,Y comes from this mapping of your hardware -- in this case, X is the card number, while Y is the device number
 - aplay -L :view all configurations


sudo apt-get install mpd mpc

sudo nano /etc/mpd.conf


bind_to_address                 "localhost"
bind_to_address         "/run/mpd/socket"
port                            "6600"
auto_update     "yes"



adaugi asta:

audio_output {
        type            "alsa"
        name            "I2S OUT"
        device          "hw:0"  
        format          "*:*:*"
        mixer_type      "hardware"      
        mixer_device    "default"       
        mixer_control   "PCM"           
        mixer_index     "0"  
        dop             "yes"           
}

Install samba

sudo apt-get install samba

sudo nano /etc/samba/smb.conf

adauga:

[internal memory]
Comment = Shared Folder
Path = /var/lib/mpd/music
read only = No
guest ok = Yes

make folder writeable:

sudo chmod 777 /var/lib/mpd/music
sudo reboot

Install serial

ls /dev/tty*
- verificare interfata seriala: /dev/ttyS0 sau /dev/ttyAMA0

sudo apt install python3-pip
sudo python3 -m pip install pyserial


sudo nano /home/pi/vfd.py - aici copiezi scriptul (verifica interfata seriala)
sudo chmod 755 /home/pi/vfd.py

/home/pi/vfd.py - rulezi scriptul

sudo reboot

sudo nano /etc/systemd/system/vfd.service - aici copiezi service

[Unit]
Description=VFD Display Service
After=mpd.service

[Service]
ExecStart=/usr/bin/python /home/pi/vfd.py
StandardOutput=syslog
StandardError=syslog
Type=simple
Restart=on-failure

[Install]
WantedBy=multi-user.target


sudo systemctl start vfd
sudo systemctl status vfd - trebuie sa apara ACTIVE
sudo systemctl enable vfd - il faci sa porneasca la boot


music_directory         "/var/lib/mpd/music"
aplay --list-devices - list audio devices
df -h (see mounted devices and space available)
sudo cat /proc/asound/card*/pcm*p/sub*/hw_params - verify the format really being sent to the physical sound chip
nano  /etc/asound.conf - alsa config
sudo cat /proc/asound/cards - view sound cards
mpc outputs - audio outputs


sudo shutdown -h now
sudo reboot

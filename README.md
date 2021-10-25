# docker-compose-rpi

Strategically placed Raspberry Pi 3 Model B+ for Bluetooth communication (and Playstation 5)

* [lolouk44/xiaomi_mi_scale](https://github.com/lolouk44/xiaomi_mi_scale)
* [fphammerle/switchbot-mqtt](https://github.com/fphammerle/switchbot-mqtt)
* [dhleong/playactor](https://github.com/dhleong/playactor)

---

#### Setup Raspberry Pi OS Lite (32-bit)
https://www.raspberrypi.com/software/<br>https://en.wikipedia.org/wiki/Raspberry_Pi_OS#Release_history

**[Advanced options](https://www.raspberrypi.com/documentation/computers/getting-started.html#advanced-options)** `Ctrl` + `Shift` + `X`

* **Enable SSH**
  * Allow public-key authentication only
* **Configure wifi (2,4 GHz)**
  * SSID: *******
  * Password: *************
  * Wifi country: SE
* **Set locale settings**
  * Time zone: Europe/Stockholm
  * Keyboard layout: se

#### Docker

```bash
ssh pi@raspberrypi.local
sudo apt-get update -y && sudo apt-get upgrade -y
curl -sSL https://get.docker.com | sh
sudo apt install docker-compose -y
sudo gpasswd -a $USER docker
sudo reboot
```

<details>
<summary>Other services</summary>

#### Rclone

https://rclone.org/drive/

```bash
curl https://rclone.org/install.sh | sudo bash
rclone sync -i /home/pi remote:rclone
```

#### Samba

```bash
sudo apt install samba -y
<no>
sudo smbpasswd -a pi
sudo truncate -s 0 /etc/samba/smb.conf
sudo nano /etc/samba/smb.conf
sudo /etc/init.d/smbd restart
```

**smb.conf**

```
[global]
client min protocol = SMB2
client max protocol = SMB3
vfs objects = catia fruit streams_xattr
fruit:metadata = stream
fruit:model = RackMac
fruit:posix_rename = yes
fruit:veto_appledouble = no
fruit:wipe_intentionally_left_blank_rfork = yes
fruit:delete_empty_adfiles = yes
security = user
encrypt passwords = yes
workgroup = WORKGROUP
server role = standalone server
obey pam restrictions = no
map to guest = never
[pi]
comment = Pi Directories
browseable = yes
path = /home/pi
read only = no
create mask = 0775
directory mask = 0775
```

`CTRL+O`, `Enter`, `CTRL+X`

</details>

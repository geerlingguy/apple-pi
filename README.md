## Apple Pi

A Raspberry Pi meant to serve as an AppleTalk server for my old Macs.

This project is pretty simple, for now—it's an Ansible playbook that configures a Raspberry Pi 5 as an AppleTalk server using [`netatalk`](https://netatalk.io), in addition to an SMB server using `samba`, so I can use it to share files between my newer computers and my old Macs.

It's a lot easier to use AppleTalk than to burn 650 MB CDs (a rare commodity), try to write floppy disks, or move files [back and forth on CF cards using `hfsutils`](https://www.jeffgeerling.com/blog/2024/getting-files-and-powerbook-3400c-hfsutils) in the Terminal!

## Choosing a Raspberry Pi

Almost any Raspberry Pi will do. You don't need a lot of speed for most old Macs, assuming you can even get them on an Ethernet network!

You could chose a 10/100 Mbps switch—even most 1 Gbps switches should work fine between the Pi and older Macs. But if you want to interface with newer/faster Macs, like G4s and G5s, I would recommend using a Gigabit network, with a Pi 4 or Pi 5.

For my own setup, I used the following:

  - Raspberry Pi 5 (4GB RAM)
  - Argon Neo 5 M.2 NVMe Case
  - Raspberry Pi 5 27W Power Adapter
  - 500 GB WD SN750 M.2 NVMe SSD (2280)

Since I am booting my Pi off an NVMe SSD, I made sure to update it's bootloader and used `sudo raspi-config` to change the Boot Order so it would boot off NVMe or USB, then try microSD (otherwise it will only boot off SD or USB).

I flashed Raspberry Pi OS (Lite) to the NVMe SSD, installed it in the case, and booted the Pi. I logged in and made sure I could do so without using a password, using an SSH key.

## Setup

Make sure you have Ansible installed (I installed it with `pip3 install ansible`), and make sure you can [SSH into the Raspberry Pi without a password](https://raspberrypi.stackexchange.com/a/54536/6506).

Then copy the example files into place and adjust them to your needs:

```
cp example.config.yml config.yml
cp example.hosts.ini hosts.ini
```

Then install required Ansible roles

```
ansible-galaxy role install geerlingguy.security geerlingguy.samba
```

And finally, run the playbook:

```
ansible-playbook main.yml
```

## Set Samba passwords

For the time being, I'm connecting as the `pi` user; to set a Samba password:

```
sudo smbpasswd -a pi
```

Then enter the password twice. Now you should be able to connect using Samba.

## Connect to AppleTalk Share

From a computer with access to AppleTalk (requires TCP support—so Mac OS 7.1 or later minimum), connect to `afp://[pi IP or hostname]` and use the `pi` username and the `pi` user account's password.

## Connect to Samba Share

From a computer with access to Samba, connect to `smb://[pi IP or host]` and use the `pi` user and the password you set earlier.

## Things remaining to be done

  - Configure regular rsync backups from Apple Pi to my primary NAS (each of the main folders).
  - Alternatively, skip using local NVMe and mount a volume from the primary NAS to share via AppleTalk
  - See if there are other services that could be hosted on the Pi for the benefit of old Macs.
  - Think harder about account management / passwords, and segmenting the Mac part of the network on its own VLAN while still being able to get at this server from another network.

## License

GPLv3 or later

## Author

Jeff Geerling

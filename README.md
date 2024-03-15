## Apple Pi

A Raspberry Pi meant to serve as an AppleTalk server for my old Macs.

This project is pretty simple, for now—it's an Ansible playbook that configures a Raspberry Pi 5 as an AppleTalk server, in addition to an SMB server, so I can use it to share files between my newer computers and my old Macs.

It's a lot easier to use AppleTalk than to burn 650 MB CDs (a rare commodity), try to write floppy disks, or move files [back and forth on CF cards using `hfsutils`](https://www.jeffgeerling.com/blog/2024/getting-files-and-powerbook-3400c-hfsutils) in the Terminal!

## Choosing a Raspberry Pi

TODO.

## Setup

Make sure you have Ansible installed (I installed it with `pip3 install ansible`), and make sure you can [SSH into the Raspberry Pi without a password](https://raspberrypi.stackexchange.com/a/54536/6506).

Then copy the example files into place and adjust them to your needs:

```
cp example.config.yml config.yml
cp example.hosts.ini hosts.ini
```

TODO.

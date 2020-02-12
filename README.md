# k3s-paasmonkeys
My own k3s cluster on Raspberry Pi.

![PAASMONKEYS](images/paasmonkeys.png)]

## Getting started

### Flash SD Card
* Download Raspbian Lite
* Flash SD card using Etcher
* Mount the SD card and create a text file named "ssh" in the boot partition.

### Power-up the Raspberry with the SD Card
It will be accessible on your network over ssh using the following command:
```
ssh pi@raspberrypi.local
```
Log in with the password raspberry and then type:
```
sudo raspi-config
```
Do the following actions:
* Set the GPU memory split to 16mb
* Set the hostname
* Change the password for the pi user

Set static IP
```
ifconfig | grep -i inet
sudo nano /etc/dhcpcd.conf
```
Add to end of this file:
``
interface eth0
static ip_address=192.168.2.xxx/24
static routers=192.168.2.1
static domain_name_servers=192.168.2.1
``

## Join Node
SSH to Master
```
ssh pi@k3s-paasmonkey-m01
```
Copy public key to the Node
```
ssh-copy-id pi@k3s-paasmonkey-n0x.local
```
Join Node to the Cluster
```
export AGENT_IP=192.168.2.xxxx
export SERVER_IP=192.168.2.120
export USER=pi
k3sup join --ip $AGENT_IP --server-ip $SERVER_IP --user $USER
```

Credits:
========

* [Will it cluster? k3s on your Raspberry Pi](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/)

License
-------

See the [LICENSE file](LICENSE) for license text and copyright information.

# k3s-paasmonkeys
My own k3s cluster on Raspberry Pi.

![PAASMONKEYS](images/paasmonkeys.png)

## Prerequisites
* 6 SD card 32GB like [SanDisk 32GB ULTRA microSDHC Card Class 10](https://www.amazon.com/gp/product/B007JTKLEK/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B007JTKLEK&linkCode=as2&tag=alexellisuk-20&linkId=72069d86b6c70e1dc49c2f0ce35f08ef)
* 6 Raspberry Pi 3b+ (1 Masters and 5 Worker)
* 8-port Ethernet Gigabit Switch
* 8-way USB Power-supply
* 6 Ethernet cables
* 6 Micro-USB cables
* Raspberry Pi Cluster case

## Getting started
![PAASMONKEYS](images/cluster.png)

### Flash SD Card
* Download Raspbian Lite
* Flash SD card using Etcher
* Mount the SD card and create a text file named "ssh" in the boot partition.

### Customize the Raspberry
Insert the SD card and turn on your Raspberry. It will be accessible on your network over ssh using the following command:
```
ssh pi@raspberrypi.local
```
Log in with the password "raspberry" and then type:
```
sudo raspi-config
```
Do the following actions:
* Set the GPU memory split to 16mb
* Set the hostname
* Change the password for the pi user

#### Set static IP
```
ifconfig | grep -i inet
sudo nano /etc/dhcpcd.conf
```
Add to end of this file:
```
interface eth0
static ip_address=192.168.2.xxx/24
static routers=192.168.2.1
static domain_name_servers=192.168.2.1
```
#### Enable container features
edit /boot/cmdline.txt 
```
sudo nano /boot/cmdline.txt
```
and add the following to the end of the line:
```
cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
```
Now reboot the device:
```
sudo shutdown -r now
```

### Add your ssh key
Copy public key to the Node
```
ssh-copy-id pi@k3s-paasmonkey-n0x.local
```
Now you can rely on your public key to log into each RPi without typing a password in.

## Join Node
SSH to our Master
```
ssh pi@k3s-paasmonkey-m01.local
```
Copy public key to the Node
```
ssh-copy-id pi@k3s-paasmonkey-n0x.local
```
Join Node to the Cluster
```
export AGENT_IP=192.168.2.xxx
export SERVER_IP=192.168.2.120
export USER=pi
k3sup join --ip $AGENT_IP --server-ip $SERVER_IP --user $USER
```

### Api Gateway
[Kong](https://konghq.com/) is an open-source API gateway that is built on top of a lightweight proxy, Nginx. 

TODO:

## Exposing Api Gateway on Private Kubernetes Clusters with a Public IP
[inlets-operator](https://github.com/inlets/inlets-operator) automates the creation of an inlets exit-node on public cloud, and runs the client as a Pod inside your cluster. Your Kubernetes Service will be updated with the public IP of the exit-node and you can start receiving incoming traffic immediately.

The cost of the LoadBalancer with a IaaS like DigitalOcean is around 5 USD / mo.

TODO:

Credits:
========

* [Will it cluster? k3s on your Raspberry Pi](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/)
* [Inlets Operator — Exposing Services on Private Kubernetes Clusters with a Public IP](https://itnext.io/inlets-operator-exposing-services-on-private-kubernetes-clusters-with-a-public-ip-e701c64693ae)

License
-------

See the [LICENSE file](LICENSE) for license text and copyright information.

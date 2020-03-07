- [Kong](#kong)
  * [Troubleshooting](#troubleshooting)
    + [Kong doesn't support ARM7](#kong-doesn-t-support-arm7)
  * [Exposing Api Gateway on Private Kubernetes Clusters with a Public IP](#exposing-api-gateway-on-private-kubernetes-clusters-with-a-public-ip)
  * [Deploy Kong in our Cluster](#deploy-kong-in-our-cluster)

# Kong
[Kong](https://konghq.com/) is an open-source API gateway that is built on top of a lightweight proxy, Nginx.

## Troubleshooting
### Kong doesn't support ARM7

You can find the [issue here](https://discuss.konghq.com/t/support-linux-arm-v7/5610?u=ymedlop). I was trying to compile it on ARM7 but I was wasting so much time and I wasn't getting any results. 

Because of that and because Kong support now ARM64. I upgraded one of my RPi to a 64 Bit Kernel following this [article](https://illegalexception.schlichtherle.de/raspbian/2019/09/21/how-to-upgrade-raspbian-to-a-64-bit-kernel/).

I used Kubernetes Node affinity to define my RPI node as a special node with arm64 arch and I updated my k8s template to only work in this kind of RPI Node.

I found that the Kong image was built on linux/arm64/v8 and my stack was requiring linux/arm64. I built my own version [ymedlop/kong:2.0.1-ubuntu](https://hub.docker.com/repository/docker/ymedlop/kong/tags?page=1).

About the ingress-controler image. I used [leandrocarneiro/kong-ingress-controller:0.7.0](https://hub.docker.com/r/leandrocarneiro/kong-ingress-controller/tags).

### Konga doesn't support ARM64
[Konga](https://github.com/pantsel/konga) is a GUI to Kong Admin API with these features:
* Manage all Kong Admin API Objects.
* Import Consumers from remote sources (Databases, files, APIs etc.).
* Manage multiple Kong Nodes.
* Backup, restore and migrate Kong Nodes using Snapshots.
* Monitor Node and API states using health checks.
* Email & Slack notifications.
* Multiple users.
* Easy database integration (MySQL, postgresSQL, MongoDB).

[Github issue about ARM64](https://github.com/pantsel/konga/issues/457) because of that I am working on my own image with ARM64 support.

## Exposing Api Gateway on Private Kubernetes Clusters with a Public IP
We going to use [inlets-operator](https://github.com/inlets/inlets-operator). It automates the creation of an inlets exit-node on public cloud, and runs the client as a Pod inside your cluster. Your Kubernetes Service will be updated with the public IP of the exit-node and you can start receiving incoming traffic immediately.

The cost of the LoadBalancer with a IaaS like DigitalOcean is around 5 USD / mo.

## Deploy Kong in our Cluster
![PAASMONKEYS](https://aws1.discourse-cdn.com/standard10/uploads/konghq/optimized/2X/2/227898ccec868321002dd9b365c49c924182ba90_2_690x134.png)
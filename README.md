# k3s-paasmonkeys


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

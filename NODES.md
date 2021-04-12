# Nodes
## panda
My old laptop. 1 TB; Intel Core i7-5500U 2.40GHz; 2x DDR3 1600MHz 4GB;
`192.168.0.134`

## gloss
An old mini computer. 230 GB; Intel Atom D510 1.66GHz; 1x DDR2 667MHz 2GB;
`192.168.0.125`

## dcapo
Old desktop PC. 572 GB; Intel Core Dual 1.80GHz; 4x DDR2 800MHz 1GB;
`192.168.0.107`

## romba
[Acer Aspire R3610][acer-r3610]. 320GB; Intel Atom 1.60GHz; 1x DDR2 800MHz 2GB;
`192.168.0.105`

## rasp1
[Raspberry pi 4 B][pi-4]. SD card 32GB; Quad core Cortex-A72 1GHz; 4GB
LPDDR4-3200 SDRAM; `192.168.0.112`

# Setup 

0. Assuming a fresh installation of Ubuntu server.

1. Check that `.ssh/authorized_keys` contains key
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFc5Y4zd8S6HPGR/dWmdwG7j4XAQIq+4quywtpeUUmNA michael@porkbrain.com
```

2. `sudo apt upgrade -y`

3. `sudo snap install microk8s --classic`


4. Configure permissions and restart session
```
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
su - $USER
```

5. `microk8s disable ha-cluster` (see [`microk8s.md`](microk8s.md#issues))

6. Add other nodes to the `/etc/hosts` file. Here's a list of nodes and their
   internal IPs. Note that the node itself should be on `127.0.0.1`.
```
192.168.0.125 gloss
192.168.0.134 panda
192.168.0.105 romba
192.168.0.107 dcapo
192.168.0.112 rasp1
```

7. Add new node's hostname and IP to other nodes' `/etc/hosts`.

8. `microk8s add-node` on master

<!-- Resources -->
[pi-4]: https://www.raspberrypi.org/products/raspberry-pi-4-model-b/specifications
[acer-r3610]: https://en.wikipedia.org/wiki/Acer_AspireRevo

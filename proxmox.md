## Remove Node from cluster
```bash
# Execute the following commands via proxmox shell or ssh
# terminal(recommened). In this way you don’t have to empty all
# VMs from a node before removing a cluster from it.

sudo systemctl restart pve-cluster
sudo systemctl stop pve-cluster
sudo pmxcfs -l
[main] notice: forcing local mode (although corosync.conf exists)
sudo rm -f /etc/pve/cluster.conf /etc/pve/corosync.conf
sudo rm -f /var/lib/pve-cluster/corosync.authkey
sudo systemctl stop pve-cluster
sudo rm /var/lib/pve-cluster/.pmxcfs.lockfile
sudo systemctl restart pve-cluster
sudo systemctl restart pvedaemon
sudo systemctl restart pveproxy
systemctl restart pvestatd
```

## Apply Network Changes
```bash
ifreload -a
```


## Home Assistant
```bash
# Adicionar USB ao container
no arquivo /etc/pve/lxc/<id do container>.conf
lxc.cgroup2.devices.allow: c 188:* rwm
lxc.mount.entry: /dev/ttyUSB0 dev/ttyUSB0 none bind,optional,create=file

# Corrigir chmod usb.
No nó do proxmox executa
sh chmod a+rwx /dev/ttyUSB0

# Corrigir conexão claudflare
No arquivo configuration.yaml, adiciona
http:
  use_x_forwarded_for: true
  trusted_proxies:
  - 172.18.0.1 # Ip do proxy que está sendo bloqueado
```

# Utilitários Ubuntu-server

## Copy SSH Key into remote ubuntu-server
```console
cat ~/.ssh/id_rsa.pub | ssh user@remote.host 'dd of=.ssh/authorized_keys oflag=append conv=notrunc'
```

## Extend LVM Disk on Linux Ubuntu ([artigo](https://netshopisp.medium.com/how-to-extend-lvm-disk-on-linux-ubuntu-20-04-35b1c2d5d5e9))
```console
# To find out the physical volumes (PV)
sudo vgdisplay

# To find out the Volume group name (LV)
sudo lvdisplay

# Extend the Logical volume
sudo lvm lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
# Make output show the correct size of your file system
sudo resize2fs -p /dev/mapper/ubuntu--vg-ubuntu--lv
```
## Impedir desligamento notebok ao abaixar a tela
```console
# Ignora ação de baixar a tela
sudo nano /etc/systemd/logind.conf
  HandleLidSwitch=ignore
  HandleLidSwitchExternalPower=ignore
  HandleLidSwitchDocked=ignore
sudo systemctl restart systemd-logind.service

# Desliga a tela após 300s
sudo nano /etc/default/grub
  GRUB_CMDLINE_LINUX="consoleblank=300"
sudo update-grub


```
## Remove subscription notice
```console
# list of available CPU governor states
cd /usr/share/javascript/proxmox-widget-toolkit/
cp proxmoxlib.js proxmoxlib.js.bak
nano proxmoxlib.js
>> ctrl + w -> if(res ==
>>> change to if (false)
systemctl restart pveproxy

```


## Configure Proxmox for lower power usage ([link](https://community.home-assistant.io/t/psa-how-to-configure-proxmox-for-lower-power-usage/323731))
```console
# list of available CPU governor states
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors

# enable a more power efficient state
echo "powersave" | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

```

## Configure Proxmox volumes
```console
# remove local-lvm

# enable a more power efficient state
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root

```

## Run powertop --auto-tune after boot
```console
cat << EOF | tee /etc/systemd/system/powertop.service
[Unit]
Description=PowerTOP auto tune

[Service]
Type=oneshot
Environment="TERM=dumb"
RemainAfterExit=true
ExecStart=/usr/sbin/powertop --auto-tune

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable powertop.service

```

## Enable Wake on LAN ([artigo](https://www.golinuxcloud.com/wake-on-lan-ubuntu/))
```bash
## get card name and MAC Address
ip a
## card supports?
sudo ethtool enp2s0
# >>> Wake-on: d

## Enable
sudo ethtool -s <enp2s0> wol g
# >>> Wake-on: g

## Send magic packet
sudo wakeonlan 30:5a:3a:0d:ac:0d
```

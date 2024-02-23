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

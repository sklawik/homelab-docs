```tab
Adding software as systemd service.
```
So it may start as system boots and we have full control over logs, auto-restarts when service dies, etc.
Real life example using sunshine.AppImage

```bash
chmod +x ~/Applications/sunshine.AppImage\
mkdir -p ~/.config/systemd/user
nano ~/.config/systemd/user/sunshine.service
```

copy & paste this config for systemd service while having file open in nano from command above:
Requires and After may listen for external services so depending on them app may start after other services are up and running.

```service
[Unit]
Description=Sunshine streaming service
After=graphical-session.target
PartOf=graphical-session.target
# Requires=postgresql.service
# After=postgresql.service

[Service]
Type=exec
ExecStart=/home/user/Applications/sunshine.AppImage
Restart=on-failure
RestartSec=5

[Install]
WantedBy=graphical-session.target
```
we can check paths and users using:
```bash
whoami
realpath ~/Applications/sunshine.AppImage
```

then commands to apply the service and manage it:

```bash
systemctl --user daemon-reload

systemctl --user enable sunshine.service

systemctl --user start sunshine.service

systemctl --user enable --now sunshine.service

systemctl --user status sunshine.service

journalctl --user -u sunshine.service -f

systemctl --user start sunshine

systemctl --user stop sunshine

systemctl --user restart sunshine

systemctl --user status sunshine

systemctl --user disable sunshine
```

--user parameter is needed for graphical applications

Enabling service to start even after user doesn't sign in yet:
```bash
loginctl enable-linger USER
```

logs
```bash
sudo journalctl -u sunshine.service
```


```bash
restricted port management
```
```bash
sudo sysctl -w net.ipv4.ip_unprivileged_port_start=53
```
-w stands for write (re-write default values)
```tab
ROUTING
```

<details>

  ```
Save iptables configuration:
```bash
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

See network config
```bash
nmcli show
```

Example network configuration history config
```bash
  36  sudo iptables -A FORWARD -i wlan0 -o end0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
   37  sudo sysctl -w net.ipv4.ip_forward=1
   38  sudo iptables -A FORWARD -i end0 -o wlan0 -j ACCEPT
   39  nmcli show
   40  sudo iptables -A FORWARD -i enp1s0u1u1 -o end0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
   41  sudo iptables -A FORWARD -i wlan0 -o end0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
   42  ping 8.8.8.8
   43  iptables -t nat -L POSTROUTING -n -v
   44  iptables -L FORWARD -n -v
   45  iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
   46  iptables -A FORWARD -i end0 -o wlan0 -j ACCEPT
   47  iptables-save | sudo tee /etc/iptables/rules.v4
```


Example wlan0 > end0 (ethernet) forward config
```bash

# iptables -L FORWARD -n -v
 pkts bytes target     prot opt in     out     source               destination         
    0     0 ACCEPT     0    --  wlan0  end0    0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED
  202 13161 ACCEPT     0    --  end0   wlan0   0.0.0.0/0            0.0.0.0/0           
    0     0 ACCEPT     0    --  wlan0  end0    0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED
 iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
 iptables -A FORWARD -i end0 -o wlan0 -j ACCEPT
```

</details>


Overwriting conflicting packages while updating arch / arch based distro.
Example when update will throw error that folder already exists and can not proceed:

```bash
pacman -Syu --overwrite "path1,path2,path3"
```

To find out disk ID we use
```bash
sudo blkid
```
as similiar as we list disks that are connected
```bash
lsblk
# and
sudo lsblk -f /dev/disk
```
Edit `/etc/fstab` file
and put line
```bash
UUID=YOUR_DISK_ID  PATH_TO_FOLDER  auto  defaults,nofail  0  2
```


Enable ipv4 routing in linux kernel and save it for future restarts:
```bash
sudo nano /etc/sysctl.d/99-ip-forward.conf
```
write:
```bash
net.ipv4.ip_forward = 1
```

save it and enable without rebooting system
```bash
sudo sysctl --system
```


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


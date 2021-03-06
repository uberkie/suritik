## Suritik

Python script for adding Suricata alerts into Mikrotik routers.

It's reading from Suricata eve-log file named alerts.json.

Requirements:
- python-librouteros
- python-ujson
- python-pyinotify

In suricata.yaml add another eve-log:
```
  - eve-log:
      enabled: yes 
      filetype: regular
      filename: alerts.json   
      types:      
        - alert
```

Additionally, if using logrotate for rotating logs, you should have 'copytruncate' option in /etc/logrotate.d/suricata.

I'm using it as a systemd service:

```
[Unit]
Description=Suricata to Mikrotik API in Python
After=network.target time-sync.target
Requires=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /usr/local/bin/suritik.py
Restart=on-failure
RestartSec=3
Environment=PYTHONUNBUFFERED=1

[Install]
WantedBy=multi-user.target
```

--------------------------------------------------------------------
Credits for idea:
- tomfisk - https://forum.mikrotik.com/viewtopic.php?f=2&t=111727
- elmaxid - https://github.com/elmaxid/Suricata2MikroTik

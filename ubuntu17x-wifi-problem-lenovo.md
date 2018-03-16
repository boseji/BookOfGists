# Ubuntu 17.X WiFi problem with Lenovo

This problem specifically occours with Lenovo laptops.

The problem happens when the laptop goes to any type of standby , sleep or hybrid-sleep.

It causes the WiFi the not funciton again after wakeup.

This probelem was reported and solutions were offered :

https://askubuntu.com/questions/761180/wifi-doesnt-work-after-suspend-after-16-04-upgrade

Here is the Working Solution:

https://askubuntu.com/a/761220

This problem was seen on 17.10 version in Jan'2018 and Mar'2018.

It may be fixed in the future updates.

## Solution using a Command

```shell
sudo systemctl restart network-manager.service
```

16.04 aand above runs on systemd, so this should work.

## Creating a Resume Service

### 1. Create the Services File in `nano` editor

```shell
sudo nano /etc/systemd/system/wifi-resume.service
```

### 2. Service File Contents

```shell
#/etc/systemd/system/wifi-resume.service
#sudo systemctl enable wifi-resume.service
[Unit]
Description=Restart networkmanager at resume
After=suspend.target
After=hibernate.target
After=hybrid-sleep.target

[Service]
Type=oneshot
ExecStart=/bin/systemctl restart network-manager.service

[Install]
WantedBy=suspend.target
WantedBy=hibernate.target
WantedBy=hybrid-sleep.target
```

### 3. Activate the Service

```shell
sudo systemctl enable wifi-resume.service
```


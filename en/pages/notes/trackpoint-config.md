# Configuring Trackpoint sensitivity/speed on linux systems


To find the device path of your trackoint

```bash
$ find /sys/devices/platform/i8042 -name name | xargs grep -Fl TrackPoint | sed
's/\/input\/input[0-9]*\/name$//'
```

To change values for current session (resets at boot) for testing...

`echo 220 | sudo tee /sys/devices/platform/i8042/serio1/serio2/sensitivity`

`echo 100 | sudo tee /sys/devices/platform/i8042/serio1/serio2/speed`

## Creating a udev rule

To apply settings during system start.

`sudo vim /etc/udev/rules.d/trackpoint.rules`

```bash
ACTION=="add",SUBSYSTEM=="input",ATTR{name}=="TPPS/2 IBM \
TrackPoint",ATTR{device/sensitivity}="200",ATTR{device/speed}="120"
```


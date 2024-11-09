---
layout: post
title: "NVIDIA GPUs fan control from terminal"
tags: hardware, deep learning
---
It's a long standing issue that NVIDIA doesn't allow GPUs fan control via command line. NVIDIA only allows to control fans of consumer GPUs via GUI tool. That might be painful and inconvenient on headless Deep Learning rigs.

A [relevant thread](https://forums.developer.nvidia.com/t/how-to-set-fanspeed-in-linux-from-terminal/72705) stays open on NVIDIA devs forum for at least last 5 years. Official documentation is sparse on this matter, and third-party resources like [tips&trick on archlinux wiki](https://wiki.archlinux.org/title/NVIDIA/Tips_and_tricks) are way more informative.

Bellow I describe an effective way of GPU fan control, tested on Ubuntu 22.04.

1. Allow manual fan control by setting corresponding coolbits value in X11 config.
```
sudo nvidia-xconfig -a --cool-bits=4
```

`-a` generates `xorgs.conf` for all GPUs available on your machine
`--cool-bits=4` enables manual GPU fun speed configuration. You can read more about meaning of various coolbits values [here](https://wiki.archlinux.org/title/NVIDIA/Tips_and_tricks#Overclocking_and_cooling)

Open `/etc/X11/xorgs.conf` and make sure that number of `Device` and `Screen` sections equal to number of GPUs on the server. Also make sure `Screen` sections reflects supplied `cool-bits` value. 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "NVIDIA GeForce RTX 4090"
    BusID          "PCI:193:0:0"
EndSection

Section "Screen"
    Identifier     "Screen0"
    Device         "Device0"
    Monitor        "Monitor0"
    DefaultDepth    24
    Option         "AllowEmptyInitialConfiguration" "True"
    Option         "Coolbits" "4"
    SubSection     "Display"
        Depth       24
    EndSubSection
EndSection

...
```

2. Now with manual fan speed configuration enabled it's sufficient to call `nvidia-settings` emulating run of xserver. 
Script bellow accepts single argument that specifies fan speed for all GPUs. Calling `sudo gpufancontrol.sh 80` will set fans at 80% speed.

```bash
#!/bin/bash

set -eo pipefail

FAN_SPEED=$1

DISPLAY=:1
startx -- ${DISPLAY} &
sleep 1
nvidia-settings -c ${DISPLAY} -a GPUFanControlState=1 -a GPUTargetFanSpeed=${FAN_SPEED}
killall Xorg
exit 1
```


You can download script from [gist](https://gist.github.com/taras-sereda/15d1afb127a332721e008950574b972d#file-gpufancontrol-sh). That's it, now you have an easy way of fan speed control from the command line. In my experience automatic GPUs NVIDIA tends to keep fans at lower speed, perhaps trying to optimise for noise level. But my servers sit in a basement and I care more about hardware being not overheated. Moreover I use cheap gaming PCIe raisers that tend to work unstable in higher temperatures. While writing this I'm re running a training that previously was failing due to PCIe errors. Peak observed temperatures were ~65C on average, fans were spinning at 40% speed. Now when I set fans at 95% speed, temperatures reduced to 45C. A stunning GPU temperature reduction by 20C, so far training is stable!

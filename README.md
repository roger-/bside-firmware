# Overview

Here's a quick guide to updating the BSIDE ESR02 Pro component tester firmware to version 1.51m.

It requires compiling firmware, opening your device and soldering wires and then flashing the microcontroller with an AVR programmer (you can also use an Arduino for this).

# Compiling firmware

Following assumes your're using a Linux and have Docker installed.

1. Download the [latest firmware source](https://github.com/madires/Transistortester-Warehouse/blob/master/Firmware/m-firmware) (note as of 1.51m there is a fix for `cap.c` that also needs to be downloaded)
1. Unpack the gzip file and overwrite the existing files with the ones in `modified/` in this repo
1. Compile using a Docker-ized toolchain (saves needing to install it locally):
    ```
    docker run --rm --privileged -v $(pwd):/build lpodkalicki/avr-toolchain make
    ```
1. If that works, you should be left with some `ComponentTester.*` files (you can also use the files in `firmware/` on this repo)

Note there are various options in `config.h` you can play with, but some won't fit on the AVR flash.

# Soldering connections for flashing

Now open your device and [identify which PCB version you have](https://www.eevblog.com/forum/testgear/bside-esr02-pro-transistor-tester-(teardown-quick-test)/msg2866314/#msg2866314). If yours does not have all the ICSP pins exposed on the unpopulated headers, then you have the same version as me and need to solder as in the picture below:

![image](https://github.com/roger-/bside-firmware/assets/1389709/1362924a-63dd-4134-a1b8-575cbbf48763)

Then you need to connect the pins to your programmer. I used an AVR ISP mkII, which doesn't supply the required 5V, so I modded it [as per here](https://forum.arduino.cc/t/add-power-to-an-atmel-avrisp-mkii/122925) except with momentary switch after the diode.

# Flashing

Now you should be able to flash it using

```
docker run --rm --privileged -v (pwd):/build lpodkalicki/avr-toolchain upload
```

# References
* https://www.eevblog.com/forum/testgear/$20-lcr-esr-transistor-checker-project/
* https://www.eevblog.com/forum/testgear/bside-esr02-pro-transistor-tester-(teardown-quick-test)/

# Final steps

Make sure your unsolder the wires you used for flashing and recalibrate your unit.

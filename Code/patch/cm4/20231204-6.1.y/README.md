# Patching the uConsole CM4 with the uConsole CM4

I'm not a huge fan of cross-compiling, and have always preferred to "dogfood" it. The original compilation instructions were also just wrong? I don't know what's going on there.

## Prerequisites

First, you will need the programs required to compile the kernel.

```
sudo apt install git bc bison flex libssl-dev make
```

Next you will need the kernel.

```
git clone https://github.com/labrat97/uconsole-linux
```

## Compiling

First, go into the kernel source directory and make the kernel compilation config that will be placed at `.config` in the source root.

```
cd uconsole-linux
KERNEL=kernel8.img make bcm2711_defconfig
```

Next, you compile the kernel. As a note, this won't take too much ram but will take a long time.

```
KERNEL=kernel8.img make -j4 Image.gz modules dtbs
```

## Installation

**This will destroy your boot partition if you mess this up. Please back this partition up if you would like to potentially save time later.**

First, we install the modules.

```
sudo make modules_install
```

Then, we have to copy the compiled files into the boot partition.

```
sudo cp arch/arm64/boot/dts/broadcom/*.dtb /boot/
sudo cp arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm64/boot/dts/overlays/README /boot/overlays/
sudo cp arch/arm64/boot/Image.gz /boot/kernel8.img
```

Next, you need to make sure that the correct modules are loaded when booting. First, find the tag of the kernel modules that you just installed with `ls /lib/modules`. Then do the following:

```
sudo cp .config /boot/config-[kernel version found above]
sudo update-initramfs -d -k 5.10.17-v8+
sudo update-initramfs -c -k [kernel version found above]
```

In order to use compiled kernel8.img with CM4 in uConsole, we have to setup a config.txt. This should already be in the /boot partition, but it you should remove the `[pi4]` line as (if I'm not mistaken) the definition has changed and the specification is redundant.

```
disable_overscan=1
dtparam=audio=on
max_framebuffers=2

[all]
ignore_lcd=1
dtoverlay=dwc2,dr_mode=host
dtoverlay=vc4-kms-v3d-pi4,cma-384
dtoverlay=devterm-pmu
dtoverlay=devterm-panel-uc
dtoverlay=devterm-misc
dtoverlay=audremap,pins_12_13

dtparam=spi=on
gpio=10=ip,np
dtparam=ant2
```

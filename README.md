# OSD32MP1 Debian SDK
This repository contains the list of repositories used for OSD32MP1 Debian SDK.

Forked from the official [Octavo repo](https://github.com/octavosystems/osd32mp1-debian) for customizing to the TRI Nervous System Compute Node boards.

## Introduction
OSD32MP1 Debian SDK can be used to develop Debian Linux images for OSD32MP1 based platforms. The SDK runs on host desktop preferably runnning Ubuntu. Docker is used to containerize the build environment. Souce code from ST (OpenSTLinux) is used, for TF-A, U-Boot, Linux kernel, GCNano and STM32CubeMP1. So, the SDK is compatible with software from ST. Binary images for both SD card and eMMC can be generated. Cube Programmer can be used to flash the eMMC via USB.

The following repositories make up OSD32MP1 Debian SDK::

| Package | URL |
| ------- | --- |
| TF-A Source code | https://github.com/STMicroelectronics/arm-trusted-firmware.git |
| U-Boot Source code | https://github.com/STMicroelectronics/u-boot.git |
| Linux kernel Source code | https://github.com/STMicroelectronics/linux.git |
| GCNano Binaries | https://github.com/STMicroelectronics/gcnano-binaries.git |
| STM32Cube MP1 Package | https://github.com/STMicroelectronics/STM32CubeMP1.git |
| Debian multistrap Source code | https://github.com/octavosystems/osd32mp1-multistrap-buster.git |
| Docker image Source code |  https://github.com/octavosystems/osd32mp1-debian-docker.git |
| Build tools source code | https://github.com/AvinashUttamchandani-TRI/osd32mp1-build-tools.git |

## Boards supported:
| Board | Webpage |
| ----- | ------- |
| OSD32MP1-RED | https://octavosystems.com/octavo_products/osd32mp1-red/ |
| TRI Nervous System Compute Node | no link yet |

## Requirements
Tested on Ubuntu 18.04 LTS

The following packges are recommended:

| Package | Version |
| ------- | ------- |
| repo | v2.13 or higher|
| git | 2.17 or higher|
| Python | 3.x |
| Docker | 19.x |
| qemu-user-static | v5.4-stm32mp |
| STM32MP1 Cube Programmer | 2.5 or higher|


## OSD32MP1 Target binary versions:

The following version of the image are supported in release:

| Package | Version | Source URL |
| ------- | ------- | ---------- |
| TF-A | v2.0-stm32mp-r1.5 | https://github.com/STMicroelectronics/arm-trusted-firmware.git | 
| U-Boot | v2018.11-stm32mp-r3 | https://github.com/STMicroelectronics/u-boot.git |
| Linux Kernel | v4.19-stm32mp-r3 | https://github.com/STMicroelectronics/linux.git |
| GCNano(binaries) | gcnano-6.2.4_p4-binaries | https://github.com/STMicroelectronics/gcnano-binaries.git |
| STM32CubeMP1 | 1.2 | https://github.com/STMicroelectronics/STM32CubeMP1.git |

## How to use the SDK
### Setup Host computer
1. ```sudo apt-get update```
2. ```sudo apt-get upgrade```
3. ```sudo apt-get install git docker.io repo qemu-user-static```
4. ```sudo systemctl enable docker```
5. ```sudo systemctl start docker```

Install Cube Programmer from here: https://www.st.com/en/development-tools/stm32cubeprog.html
Please see https://wiki.st.com/stm32mpu/wiki/STM32CubeProgrammer for Cube Programmer installation instructions and setup.

### Get SDK source code
1. ```mkdir ~/osd32mp1-workspace```
2. ```cd ~/osd32mp1-workspace```
3. ```repo init -u https://github.com/AvinashUttamchandani-TRI/osd32mp1-debian.git```
4. ```repo sync```

### Compile docker image
1. ```cd ~/osd32mp1-workspace/docker```
2. ```sudo make build```

### Compile target image for OSD32MP1-RED
1. ```cd ~/osd32mp1-workspace/docker```
2. ```sudo make run```
3. ```make all```

### Outputs
The following outputs are generated in ~/osd32mp1-workspace/deploy :

TODO@avinash - update based on final filenames.

| Output | Filename | Comment |
| ------ | -------- | ------- |
| 1 | tf-a-osd32mp1-red.stm32 | Binary for OSD32M1 arm-trusted-firmware |
| 2 | tf-a-osd32mp1-red-trusted.stm32 | Binary for OSD32MP1 arm-trsuted-firmware |
| 3 | u-boot-osd32mp1-red-trusted.stm32 | Binary for OSD32MP1 U-Boot |
| 4 | FlashLayout_sdcard_osd32mp1-red-trusted.tsv | Flash layout file for OSD32MP1-RED trusted - SD card |
| 5 | FlashLayout_sdcard_osd32mp1-red-trusted.raw | Binary image for OSD32MP1-RED trusted - SD card |
| 6 | FlashLayout_emmc_osd32mp1-red-trusted.tsv | Flash layout file for OSD32MP1-RED trsudted - eMMC |
| 7 | octavo-bootfs-debian-lxqt-osd32mp1-red.ext4 | BOOTFS binary for OSD32MP1-RED |
| 8 | octavo-rootfs-debian-lxqt-emmc-osd32mp1-red.ext4 | ROOTFS binary for OSD32MP1-RED - eMMC |
| 9 | octavo-rootfs-debian-lxqt-sdcard-osd32mp1-red.ext4 | ROOTFS binary for OSD32MP1-RED - SD card |
| 10 | octavo-vendorfs-debian-lxqt-osd32mp1-red.ext4 | VENDORFS binary for OSD32MP1-RED |

### Deploy on SD card (where sdX is the device assignment for SD card)
1. ```cd ~/osd32mp1-workspace/deploy/```
2. ```sudo dd if=FlashLayout_sdcard_osd32mp1-red-trusted.raw of=/dev/sdX bs=1M conv=fdatasync status=progress```

### Flashing emmc
No emmc on the board, n/a and untested.


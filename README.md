# Raspberry Pi Pico FreeRTOS Application Template
Adapted from [Using FreeRTOS with the Raspberry Pi Pico](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico).

Describes how to setup the build environment for creating C/C++ applications for the [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/). The Pico H (2021) was used as the target platform.

The Pico is based upon the Raspberry Pi RP2040, which features a dual-core [Arm Cortex-M0+](https://developer.arm.com/Processors/Cortex-M0-Plus) processor with 264kB internal RAM and support for up to 16MB of off-chip flash.

## Development Platform
Assuming the use of a Linux development environment. For this exercise, the Raspberry Pi 4 with 8GB RAM was used running Raspberry Pi OS, Debian GNU/Linux 11 (bullseye), with a 128GB flash card.

## Software Installation
After the development platform has booted...

### Update the OS

`sudo apt -y update`<br>
`sudo apt -y upgrade`<br>
`sudo apt -y autoremove`<br>
`cat /etc/os-release`

### Install Linux Toolset

`sudo apt install cmake`<br>
`sudo apt install gcc-arm-none-eabi`<br>
`sudo apt install build-essential`

### Install Baseline Repositories

`cd ~`<br>
`mkdir freertos-pico`<br>
`cd freertos-pico`<br>
`git clone https://github.com/RaspberryPi/pico-sdk --recurse-submodules`<br>
`git clone -b smp https://github.com/FreeRTOS/FreeRTOS-Kernel --recurse-submodules`

### Set Paths for Build Tools

`export PICO_SDK_PATH=$PWD/pico-sdk`<br>
`export FREERTOS_KERNEL_PATH=$PWD/FreeRTOS-Kernel`<br>

### Make Application Directory

`mkdir appdir`<br>
`cd appdir`

## Ready to Program

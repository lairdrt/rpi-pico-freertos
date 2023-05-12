# Raspberry Pi Pico FreeRTOS Application Template
Adapted from [Using FreeRTOS with the Raspberry Pi Pico](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico).

Describes how to setup the build environment for creating C/C++ applications for the [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/). The Pico H (2021) was used as the target platform.

The Pico is based upon the Raspberry Pi RP2040, which features a dual-core [Arm Cortex-M0+](https://developer.arm.com/Processors/Cortex-M0-Plus) processor with 264kB internal RAM and support for up to 16MB of off-chip flash.

## Development Platform
Assuming the use of a Linux development environment. For this exercise, the Raspberry Pi 4 with 8GB RAM was used running Raspberry Pi OS, Debian GNU/Linux 11 (bullseye), with a 128GB flash card.

## Software Installation
After the development platform has booted, first item is to updated the OS:

`sudo apt -y update`<br>
`sudo apt -y upgrade`<br>
`sudo apt -y autoremove`<br>
`cat /etc/os-release`


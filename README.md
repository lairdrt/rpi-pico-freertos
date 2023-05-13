# Raspberry Pi Pico FreeRTOS Application Template
Adapted from [Using FreeRTOS with the Raspberry Pi Pico](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico).

Describes how to setup the build environment for creating C/C++ applications for the [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/). The Pico H (2021) was used as the target platform.

The Pico is based upon the Raspberry Pi RP2040, which features a dual-core [Arm Cortex-M0+](https://developer.arm.com/Processors/Cortex-M0-Plus) processor with 264kB internal RAM, 2MB on-board QSPI flash, and support for up to 16MB of off-chip flash.

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

### Get the Template CMake File and FreeRTOS Config File
Copy the `CMakeLists.txt` file and the `FreeRTOSConfig.h` file from this repository into your main application directory (`appdir`).

## Ready to Program
At this point, you should all the Pico C/C++ SDK and FreeRTOS libraries installed in the directory above the application directory, and the path environment variables set corretcly. Assuming, for example, that your main file is called `myapp.c`, located in your `appdir` directory, and that you have navigated to that directory.

`mkdir build`<br>
`cd build`<br>
`cmake ..`<br>
`make`

This should result in an object file, called `myapp.uf2`, that can be downloaded to the Pico.

## Reboot the Pico
To reboot the Pico and mount its flash as a drive:
1. Hold down the `BOOTSEL` button on the Pico.
2. Plug in the USB cable from the Pico to the development platform.
3. Release the `BOOTSEL` button.
4. The Pico should appear as a mass storage device on the development platform.

## Download and Run the Program
You'll download and run the program differently depending upon whether or not you're using a desktop file manager.

### Using Desktop
Plugging in the Pico to the development platform causes the Pico's flash to be mounted as a drive. Use the File Manager to copy the `myapp.uf2` file to the Pico's file space. This should cause the file to be loaded into flash and then executed.

### Using Bash Shell
Copy the `myapp.uf2` file to the Pico's drive location, and the Pico will automatically reboot and run the application. For example, if the Pico's mounted flash drive location is `/media/myuser/RPI-RP2`, then issue this command:

`cp myapp.uf2 /media/myuser/RPI-RP2`

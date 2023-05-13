# Raspberry Pi Pico FreeRTOS Application Template
These instructions describe how to setup the build environment for creating C/C++ [FreeRTOS](https://freertos.org/) applications on the [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/). The Pico H (2021) was used as the target platform.

The Pico is based upon the Raspberry Pi [RP2040](https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf), which features a dual-core [Arm Cortex-M0+](https://developer.arm.com/Processors/Cortex-M0-Plus) processor with 264kB internal RAM, 2MB on-board QSPI flash, and support for up to 16MB of off-chip flash.

Adapted from:

[Using FreeRTOS with the Raspberry Pi Pico: Part 1](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico)

The files in the repository are from Part 1 of the tutorial.

See also:

[Using FreeRTOS with the Raspberry Pi Pico: Part 2](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico-part-2)

[Using FreeRTOS with the Raspberry Pi Pico: Part 3](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico-part-3)

[Using FreeRTOS with the Raspberry Pi Pico: Part 4](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico-part-4)

[Using FreeRTOS with the Raspberry Pi Pico and AWS IoT ExpressLink: Part 5](https://embeddedcomputing.com/technology/open-source/linux-freertos-related/using-freertos-with-the-raspberry-pi-pico-and-aws-iot-expresslink-part-5)

[Using FreeRTOS with the Raspberry Pi Pico and AWS IoT ExpressLink: Part 6](https://embeddedcomputing.com/technology/iot/device-management/using-freertos-with-the-raspberry-pi-pico-and-aws-iot-expresslink-part-6)

[Simple Multicore Core to Core Communication Using FreeRTOS Message Buffers](https://www.freertos.org/2020/02/simple-multicore-core-to-core-communication-using-freertos-message-buffers.html)

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

The `CMakeLists.txt` file assumes that:
1. The name of your application is `myapp`.
2. The name of your main program is `main.c`

You'll have to change those values in `CMakeLists.txt` if that is **not** the case.

## Ready to Program
At this point, you should have all of the Pico C/C++ SDK and FreeRTOS libraries installed in the directory above the application directory, and the path environment variables set corretcly.

Assume that you have created your application program (e.g., `main.c`) within the `appdir` directory, and that you have navigated to that directory. You would build the executable file using these commands:

`mkdir build`<br>
`cd build`<br>
`cmake ..`<br>
`make`

This should result in an executable file, called `myapp.uf2`, that can be downloaded to the Pico.

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

## Using coreJSON Library
The [coreJSON](https://github.com/FreeRTOS/coreJSON) library is not included in the standard distrubution of FreeRTOS (for the Pico). So, you have to add it manually.

### Add the Source Files
1. Copy the [core_json.c](https://github.com/FreeRTOS/coreJSON/blob/8d216b5876ba6953e8b60a20d32eae62992d09fe/source/core_json.c) file to the `~/freertos-pico/FreeRTOS-Kernel` directory.
2. Copy the [core_json.h](https://github.com/FreeRTOS/coreJSON/blob/8d216b5876ba6953e8b60a20d32eae62992d09fe/source/include/core_json.h) file to the `~/freertos-pico/FreeRTOS-Kernel/include` directory.

### Modify the Target CMake File
The `CMakeLists.txt` file that you ceated above references the target platform `CMakeLists.txt` file here:

`include($ENV{FREERTOS_KERNEL_PATH}/portable/ThirdParty/GCC/RP2040/FreeRTOS_Kernel_import.cmake)`

That file in turn includes a `library.cmake` file located here:

`include($ENV{FREERTOS_KERNEL_PATH}/portable/ThirdParty/GCC/RP2040/library.cmake)`

## Tools
To view serial data over the USB/UART port of the development platform use GTKTerm:

`sudo apt install gtkterm`

Finding the Pico's mounted USB port is non-trivial. Try:

`dmesg | grep "tty"`

Possible values include `/dev/ttyACM0` at 9600 baud.

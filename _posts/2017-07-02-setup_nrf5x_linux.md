---
layout: post
section-type: post
title: Setup nRF5xx development on Linux
category: tech
tags: [ 'nRF5xx' ]
---
#### Hardware
I use nRF52 Development kit (PCA10040). This is connected to the system through USB cable. It has inbuilt Segger JLink Interface, to flash the program.

#### Tools
I use Ubuntu 14.04 LTS. We need Nordic SDK, GNU tools, JLink software and ARM gcc tools. GNU tools are already available in Linux. So this can be avoided. So we need to download the rest of it.

Download latest <a href="https://developer.nordicsemi.com/nRF51_SDK/" target="blank">Nordik SDK</a>, the <a href="https://launchpad.net/gcc-arm-embedded/+download" target="blank">ARM Tools</a>, the <a href="https://www.segger.com/downloads/jlink" target="blank">JLink Tools</a>. Extract Nordic SDK and ARM gcc tools as follows,

<pre><code data-trim class="bash">
unzip ./nRF5_SDK_13.0.0_04a0bfd.zip -d ~/ble
sudo tar -xvfj ./gcc-arm-none-eabi-5_4-2016q3-20160926-linux.tar.bz2 -C /usr/local
</code></pre>

JLink tools will be available as a installer. So you can install JLink software using the installer.
Now we need to link the arm-gcc to Nordic SDK. To do this go to ~/ble/nRF5x_SDK/components/toolchain/gcc/ and edit Makefile.posix like below

<pre><code data-trim class="shell">
GNU_INSTALL_ROOT := /usr/local/gcc-arm-none-eabi-5_4-2016q3
GNU_VERSION := 5.4.1
GNU_PREFIX := arm-none-eabi
</code></pre>

Next we need to setup nrfjprog, this will help us to easily flash the hex file to nRF chip. Download nRF5x-Command-Line-Tools from <a href="https://www.nordicsemi.com/eng/Products/Bluetooth-low-energy/nRF52832" target="blank">here</a> depending on your machine. Extract the downloaded package somewhere and link the nrfjprog to /usr/bin.

<pre><code data-trim class="bash">
sudo ln -s 'path to nrfjprog' "/usr/bin/nrfjprog"
</code></pre>

Replace 'path to nrfjprog' with the complete path to nrfjprog in the extracted package.

#### Running an example on PCA10040
We are going to run ble_app_hrs on PCA10040. In Nordic SDK go to /examples/ble_peripheral/ble_app_hrs/pca10040/s132/armgcc/. There you will find two files, a Makefile and a linker script. Open Makefile and change

<pre><code data-trim class="Makefile">
CFLAGS += -Wall -Werror -O3 to CFLAGS += -Wall -Werror -O0 -g3 
CFLAGS += -fno-builtin --short-enums to CFLAGS += --short-enums
</code></pre>

The linker script contains information about where to place the program in the flash an where and how much ram is left for it. According to the chip marking you can find out how much RAM and flash you have in the chip. According to the available RAM and flash available and the amount of RAM and flash used by Softdevice we need to make changes in the Linker Script. For PCA10040 Nordic has already specified in the LinkerScript. But if you are using your own custom board and some other Nordic chip with different amount of RAM and Flash, then you have to set the below values accordingly.

<pre><code data-trim class="Makefile">
FLASH (rx) : ORIGIN = 0x1f000, LENGTH = 0x61000
RAM (rwx) :  ORIGIN = 0x20002c38, LENGTH = 0xd3c8
</code></pre>

Now navigate to pca10040/s132/armgcc/ and execute 'make' command. This will create a directory called _build, containing the compiled code of the example. Before flashing the hex file of compiled code, we need to flash the softdevice to the chip. To do that use the following commands,

<pre><code data-trim class="bash">
nrfjprog --eraseall -f nrf52
nrfjprog --program <SDK_ROOT)>/components/softdevice/s132/hex/s132_nrf52_3.0.0_softdevice.hex -f nrf52 --sectorerase 
nrfjprog --reset -f nrf52
</code></pre>

Once the softdevice is flashed, flash the example hex file using below command

<pre><code data-trim class="bash">
nrfjprog --program _build/nrf52832_xxaa.hex -f nrf52 --sectorerase
nrfjprog --reset -f nrf52
</code></pre>

Use the <a href="https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk51.v9.0.0%2Fble_sdk_app_hrs.html" target="blank">Nordik Infocenter</a> to test the Heart Rate Application.
Thats all... Happy coding...

# rtl8814AU
Realtek 8814AU USB WiFi Driver for Raspberry Pi.

Forked from [Thomas Pircher's](https://github.com/tpircher/rtl8814AU/)' repository which is based on version 4.3.21 of an Edimax driver for the EW-7833UAC device.

## How to install EW-7833UAC adapter on Raspberry Pi

This how to is a clone of Edimax [How to install EW-7833UAC adapter on Raspberry Pi](https://edimax.freshdesk.com/support/solutions/articles/14000063861-how-to-install-ew-7833uac-adapter-on-raspberry-pi)

This article is to show how to install the EW-7833UAC AC1750 USB 3.0 adapter on Raspberry Pi(RPi) running Raspbian Operating System(OS).  In this example, the Raspbian(9.4, Stretch) is running kernel version 4.14.34.

Please note that the instructions here are not for cross-compiling from another system.  If you want to do a cross-compile on another system, you need to look for the proper way and then transfer the driver to your RPi for installation.

Everything will be done in a Terminal program.  The commands are in green below.  The character '$' and '#' are just the prompt to indicate you're either a regular or the root user.  You don't need to include them with the commands, i.e. do not type them.  You should input the commands one at a time.

1. Make sure your RPi is able to access the Internet.
2. Open a Terminal program from your Raspbian OS.
3. Make sure your system is up-to-date.  A reboot may be needed if the kernel has been updated.
   ``` bash
   sudo apt update
   sudo apt upgrade
   sudo apt --purge autoremove
   sudo reboot
   ```
4. Install the kernel headers, which are needed to compile the driver.

   ``` bash
   sudo apt install raspberrypi-kernel-headers
   ls /lib/modules/$(uname -r)
   ```
   
   You should be able to see a __build/__ folder with the 2nd command above.  If you don't see it, something is wrong.  You need to check the installation one more time.  Here's an example.

5. Clone the open source driver from Joseph Allen's GitHub repository (https://github.com/JosephAllen/rtl8814AU/) and then change directory.

   ``` bash
   git clone https://github.com/JosephAllen/rtl8814AU.git/
   cd rtl8814AU/
   ```

   > __Note:__  After 'rt' is a letter 'l' (like lemon), not a number '1' (like 1990).  And right after '88' is a number '1' (like 1990), not a letter 'l' (like lemon).  Typing them wrong will cause GitHub to prompt you inputting a Username and a Password, or saying repository not found.

6. Compile and install the GitHub driver using dkms.

   ``` bash
   sudo su
   apt install dkms
   cp -R . /usr/src/rtl8814au-4.3.21
   dkms build -m rtl8814au -v 4.3.21
   dkms install -m rtl8814au -v 4.3.21
   ```

7. Restart your system for the newly installed driver to take effect.

   ``` bash
   sudo reboot
   ```

That's it!

## Removing Driver

If you would like to remove the driver, you can follow the steps below.

``` bash
dkms remove rtl8814au/4.3.21 --all
rm -rf /usr/src/rtl8814au-4.3.21
reboot
```

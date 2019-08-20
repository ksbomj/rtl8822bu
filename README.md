<u>**8822BU for Linux**</u>

Driver for 802.11ac USB Adapter with  
RTL8822BU chipset  
Only STA/Monitor Mode is supported, no AP.  

A few known wireless cards that use this driver include 
* [Edimax EW-7822ULC](http://us.edimax.com/edimax/merchandise/merchandise_detail/data/edimax/us/wireless_adapters_ac1200_dual-band/ew-7822ulc/)
* [ASUS AC-53 NANO](https://www.asus.com/Networking/USB-AC53-Nano/)
* [D-Link DWA-182 (Revision D1 only)](http://ca.dlink.com/products/connect/wireless-ac1200-dual-band-usb-adapter/)


> NOTE: At least v4.7 is needed to compile this module
> sorry people with older kernels, the code is removed.
> Upon request I can work towards making it backwards compatible.

Currently tested on X86_64 and ARM platform(s) **only**,  
cross compile possible.

For compiling type  
`make`  
in source dir  

To install the firmware files  
`sudo make install`


To Unload driver you may need to disconnect the device  

If the driver fails building consult your distro how to  
install the kernel sources and build an <u>external</u> module.


**NOTES**  
This driver allows use of wpa_supplicant by using the nl80211 driver
`wpa_supplicant -Dnl80211`

If installing on Rasberry Pi or other "armv71" devices, edit the Makefile and set `CONFIG_PLATFORM_ARM_RPI = y` and `CONFIG_PLATFORM_I386_PC = n`

On Debian with some wireless managers (KDE confirmed) you must append the following to /etc/NetworkManager/NetworkManager.conf:

[device]
wifi.scan-rand-mac-address=no

Otherwise, you may get stuck in an infinte loop of failed connection and a prompt for password. Source page here:
https://wiki.debian.org/WiFi

**STATUS**  
Driver works fine (some sort of)  
Most of the work is done is cleaning the driver and make this mess **readable**   for conversion.
Updates for wireless-ext/cfg80211  are not accepted.  

**PI (Copyright https://edimax.freshdesk.com/support/solutions/articles/14000062079-how-to-install-ew-7822ulc-utc-adapter-on-raspberry-pi)**

This article is to show how to install the EW-7822ULC/UTC AC1200 USB 2.0/3.0 nano/mini adapters on Raspberry Pi(RPi) running Raspbian Operating System(OS).  In this example, the Raspbian(9.9, Stretch) running kernel version is 4.19.42.

Please note that the instructions here are not for cross-compiling from another system.  If you want to do a cross-compile on another system, you need to look for the proper way and then transfer the driver to your RPi for installation.

Everything will be done in a Terminal program.  The commands are in green below.  The characters '$' and '#' are just the prompt to indicate you're either a regular or the root user.  You don't need to include them with the commands, i.e. do not type them.  You should input the commands one at a time.

[1.]  Make sure your RPi is able to access the Internet.
[2.]  Open a Terminal program from your Raspbian OS.
[3.]  Make sure your system is up-to-date.  A reboot may be needed if the kernel has been updated.

```bash
$ sudo apt update

$ sudo apt upgrade

$ sudo reboot
```

[4.]  Install the kernel headers, which are needed to compile the driver.

```bash
$ sudo apt install raspberrypi-kernel-headers

$ ls /lib/modules/$(uname -r)
```

You should be able to see a build/ folder with the 2nd command above.  If you don't see it, something is wrong.  You need to check the installation one more time.  Here's an example.

If you use the command 'rpi-update' to upgrade the kernel from stable to 'bleeding edge' (a.k.a. testing), the 1st command from the beginning of this Step will not install the headers properly.  Therefore, you won't be able to compile and install the GitHub driver.  At the time of writing, there is no 'easy' workable workaround available.  So, try to stay with the stable kernel if possible.

[5.]  Clone the open source driver from Jack Fan's GitHub repository (https://github.com/EntropicEffect/rtl8822bu ) GitHub repository.

```bash
$ git clone https://github.com/EntropicEffect/rtl8822bu.git 

$ cd rtl8822bu
```

[ Note:  Right after 'rt' is the letter 'l' (like larry), not the number '1' (like 1990).  Typing it wrong will cause GitHub to prompt you inputting a Username and a Password or saying repository not found. ]



[6.]  Modify the Makefile for Raspberry Pi architecture (ARM-based).
```bash
$ nano Makefile
```

[ Note:  nano is just my preferred Text Editor.  You can use any Text Editor you like, e.g. vi. ]

Change the lines #100 and #101 to the following:
```vim
CONFIG_PLATFORM_I386_PC = n

CONFIG_PLATFORM_ARM_RPI = y
```
Make sure you save the file after the modifications.

[7.]  Compile and install the driver.
```bash
$ make

$ sudo make install
```
[8.]  Restart your system for the newly installed driver to take effect.
```bash
$ sudo reboot
```
That's it!

You may need to recompile the driver whenever the kernel is being updated/upgraded provided that the kernel is being supported by the open source driver.  Otherwise, the adapter is not going to work.  To remove the installed the driver, try the following commands.

[A.]  Uninstall/remove the installed driver.
```bash
$ cd rtl8822bu

$ sudo make uninstall

$ make clean
```
Then, repeat the Steps 1~8 to recompile and reinstall the driver.


Enjoy!
  
**BUGS**  

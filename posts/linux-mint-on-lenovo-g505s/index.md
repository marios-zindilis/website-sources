---
title: Linux Mint 15 Installation on Lenovo G505s
first-published: 2013-10-10
description: My personal preferences for setting up Linux Mint on a Lenovo G505s laptop
---

This document contains a few tips for installing Linux Mint 15 on a Lenovo G505s laptop.

Here is a list of hardware components that work out of the box:

*   Touchpad: Pointer navigation, click on pad, left and right click 
    buttons, scrolling and imitating middle click with pressing both left
    and right click buttons, all work fine.
*   Networking, wired and wireless work fine as well.
*   Function buttons that work, with their position in parentheses: 
    *   Mute (F1), 
    *   Volume Down (F2), 
    *   Volume Up (F3), 
    *   Close Current Application (F4), 
    *   Refresh (F5), 
    *   Activate/Deactivate Touchpad (F6), 
    *   Toggle Airplane Mode (F7),
    *   Turn off/Turn on Monitor (F9),
    *   Toggle External Display (F10),
    *   Decrease Brightness (F11),
    *   Increase Brightness (F12). 

Here is a list of hardware components that do NOT work out of the box:

*   Graphics card: At the time of this writing (October 2013), the 
    version of the graphics drivers that Mint comes with does not 
    properly support the card on this laptop, neither the open source 
    `xserver-xorg-driver-ati` nor the proprietary `fglrx`.
    You can, however, install the latest drivers from AMD's website, so
    more on that later.

*   Hotkey for displaying all running applications (F8). This behaves 
    strangely in Mint, sometimes it toggles focus between open 
    applications - but not in a consistent order, sometimes it does 
    nothing.

Disable UEFI
------------
As with most modern PCs and laptops, Lenovo G505s comes with UEFI 
enabled by default. To disable it:

1.  Shutdown.
2.  Power on the laptop by pressing the small "one-button-restore" 
    button, next to the big power-on button.
3.  Select "BIOS Setup".
4.  Once in BIOS, select the "Boot" tab.
5.  Change the value of `Boot Mode` to `Legacy Support`.
6.  Change the value of `Boot Priority` to `Legacy First`.
7.  Save changes and exit (keyboard Fn+F10).


Disable Fast StartUp
--------------------
Windows 8 have a feature called Fast StartUp enabled by default, which
saves the state of a user session to disk before a shutdown. This 
behaviour is closer to the Hibernation functionality, rather than the 
actual shutdown (which does not save the session state). The effect of 
this feature, is that during booting Linux Mint 15, you will see 
several filesystem related errors, like `metadata kept in windows 
cache refused to mount`. 

To disable Fast StartUp:

1.  Boot in Windows 8.
2.  Activate the bottom left hot corner, right click on it. open 
    Control Panel.
3.  Select Power Options.
4.  Select "Choose what the power button does".
5.  Select "Change settings that are currently unavailable".
6.  Deactivate the box labelled "Turn on fast startup".
7.  Shutdown.

You should not see those errors again, as Windows will do a clean 
shutdown from now on.

Boot from Linux Mint 15 Installation DVD
----------------------------------------
To proceed with the installation, boot from the Linux Mint 15 
Installation Disk:

1.  If the laptop is on, shut it down.
2.  Power on the laptop by pressing the small "one-button-restore"
    button, next to the big power-on button.
3.  Select "Boot menu".
4.  Select "SATA ODD" (Optical Disk Drive).
5.  Proceed normally with installation. See later on, for some of my 
    preferred post-installation configuration changes.

Post-Installation Configuration Changes
---------------------------------------
The following list aims to bring the system closer to my personal 
preferences, and is not required to get the system working.

1.  Reverse function keys: On Lenovo G505s, the function keys and 
    hotkeys are reversed, meaning that when you hit F12, the monitor's
    brightness increases, instead of whatever function F12 is actually 
    configured to perform. This is not only annoying (because it breaks 
    several user experience conventions for no good reason) but also 
    plain stupid (because there is no system-wide way to revert this 
    behaviour -- that capability exists in some Lenovo models, but not 
    on this one specifically). 
    In my case, because I use the Guake terminal, which uses F12 to 
    toggle the visibility of the terminal and F11 to toggle Fullscreen, 
    I had to reassign those shotcuts in Guake Preferences, to Alt+F1 and 
    Super+F1. 
2.  Add the "Euro" symbol on the default US English keyboard layout. 
    Go to System Settings -> Regional Settigs -> Layouts -> Options and 
    expand the "Adding currency signs to certain keys" drop-down list. 
    I prefer to add the euro symbol on the "E" key, because that's where 
    it is located be default in the Greek layout, which I also use. 
    Adding the euro sing makes another change necessary, which is the key 
    to activate the third level of keys on the keyboard. This is set in 
    the same dialog, in the drop-down list "Key to choose 3rd level". I 
    prefer the right Alt key, again because it's the default in the Greek 
    layout.
3.  Markdown syntax highlighting in Vim per: http://www.vim.org/scripts/script.php?script_id=2882

Post-Installation Customization
-------------------------------

{% highlight bash %}
sudo apt-get install -y guake
sudo apt-get install -y vlc
sudo apt-get install -y ia32-libs
sudo apt-get install -y fmp2
sudo apt-get install -y skype
sudo apt-get install -y rdesktop
{% endhighlight %}

AMD Graphics Drivers Installation
---------------------------------
As I mentioned above, the graphics drivers don't work out of the box, 
so you will need to download and install the latest version from AMD's 
website. At the time of this writing, the latest stable driver version 
is 13.4. 

1.  Download the drivers from AMD's Website.

2.  Install dependencies:

        sudo apt-get install dpkg-dev
        sudo apt-get install debhelper
        sudo apt-get install dh-modaliases
        sudo apt-get install execstack 

    Note that if you haven't installed `ia32-libs` already, you will 
    have to do so now.

3.  Unzip the downloaded file `.zip` file, you will get a `.run` file.

4.  Make it executable with `chmod +x`.

5.  Build with:

        ./amd-driver-installer-catalyst-13-4-x86.x86_64.run --buildpkg Ubuntu/raring`

    This will generate 3 `.deb` files:

        fglrx_12.104-0ubuntu1_amd64.deb 
        fglrx-dev_12.104-0ubuntu1_amd64.deb 
        fglrx-amdcccle_12.104-0ubuntu1_amd64.deb

6.  Install all the `.deb` files with `sudo dpgk -i *.deb`.

7.  Reboot and enjoy.

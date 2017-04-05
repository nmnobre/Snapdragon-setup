<img src="http://www.cs.man.ac.uk/~nobren/images/snapdragon-setup.pg" height="200">

# snapdragon setup

A guide on how to setup a programming environment for the Open-Q 820 (APQ8096) development kit on Ubuntu. All of the following guidelines have been tested under Ubuntu 14.04.5 LTS.

## Enabling on-device developer options

Please note that on Android 4.2 and newer, Developer options is hidden by default. To make it available, go to Settings > About phone and tap Build number seven times. Return to the previous screen to find Developer options.

Once in Settings > Developer options, make sure to:
* enable USB debugging in the Debugging section;
* select MTP (Media Transfer Protocol) under Select USB Configuration in the Networking section. 

## Android tools

The installation of the following tools is required:
* Android Debug Bridge (adb): `sudo apt-get install android-tools-adb`
* fastboot: `sudo apt-get install android-tools-fastboot`
* Android NDK: [download the latest version for Linux x86_64](https://developer.android.com/ndk/downloads/index.html "Android NDK downloads")

In order to avoid starting adb's server as root, create a file named `/etc/udev/rules.d/51-android.rules` and add `SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", MODE="0666", GROUP="plugdev"` to its content (root permissions required). ATTR{idVendor} specifies the unique vendor ID corresponding to Qualcomm, MODE specifies read/write permissions and GROUP defines the device node's ownership.

Exporting a variable named `ANDROID_NDK` in `~/.bash_profile` pointing to Android NDK's installation directory is recommended to avoid writing its path recurrently, for consistency when writing scripts and, finally, because some of the scripts in my other repositories rely on its existance to function.

Additionally, and especially if the development board does not include a display, it might be useful to install one of the numerous applications that allow viewing and visually controlling Android devices directly from a computer. [Vysor](http://vysor.io "Vysor's official website") is one such example.

## Qualcomm SDKs

Install the Symphony and Hexagon SDKs (I'll soon further elaborate on this).

## Useful links

Additional information can be found in the [Android Developers' website](https://developer.android.com/studio/run/device.html "Android Studio user guide") and in [Nicolas Bernaerts' website](http://bernaerts.dyndns.org/linux/74-ubuntu/328-ubuntu-trusty-android-adb-fastboot-qtadb "Ubuntu 14.04 - Install Android tools").

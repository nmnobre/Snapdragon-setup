<img src="http://www.cs.man.ac.uk/~nobren/images/snapdragon-setup-artwork.png" height="200">

# snapdragon setup

A guide on how to setup a programming environment for the Open-Q 820 (APQ8096) development kit on Ubuntu. All of the following guidelines have been tested under Ubuntu 14.04.5 LTS.

## Enabling on-device developer options

Please note that on Android 4.2 and newer, the Developer options menu is hidden by default. To make it available, go to Settings > About phone and tap Build number seven times. Return to the previous screen to find Developer options.

Once in Settings > Developer options, make sure to:
* enable USB debugging in the Debugging section;
* select MTP (Media Transfer Protocol) under Select USB Configuration in the Networking section. 

## Android tools

The installation of the following tools is required:
* Android Debug Bridge (adb): `sudo apt-get install android-tools-adb`
* fastboot: `sudo apt-get install android-tools-fastboot`
* Android NDK: [download the latest version for Linux x86_64](https://developer.android.com/ndk/downloads/index.html "Android NDK downloads") and extract it using `sudo unzip NDK_PACKAGE_NAME -d EXTRACT_DIR`. For example, if the package's name is `android-ndk-r14b-linux-x86_64.zip` and `/opt/` is the target directory, `sudo unzip android-ndk-r14b-linux-x86_64.zip -d /opt/` should produce the desired results.

In order to avoid starting adb's server as root:
* create a file named `/etc/udev/rules.d/51-android.rules`
* add `SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", MODE="0666", GROUP="plugdev"` to its content

ATTR{idVendor} specifies the unique vendor ID corresponding to Qualcomm, MODE specifies read/write permissions and GROUP defines the device node's ownership. This procedure can be completed with the following command: `echo SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", MODE="0666", GROUP="plugdev" | sudo tee --append /etc/udev/rules.d/51-android.rules`.

Exporting a variable named `ANDROID_NDK` in `~/.bash_profile` pointing to Android NDK's installation directory is recommended to avoid writing its path recurrently, for consistency when writing scripts and, finally, because some of the scripts hosted in this GitHub page rely on its existance to function. For example, `echo >> ~/.bash_profile && echo "export ANDROID_NDK=/opt/android-ndk-r14b" >> ~/.bash_profile` would suffice if Android NDK's installation directory were `/opt/android-ndk-r14b`.

Additionally, and especially if the development board does not include a display, it might be useful to install one of the numerous applications that allow viewing and visually controlling Android devices directly from a computer. [Vysor](http://vysor.io "Vysor's official website") is one such example.

## Qualcomm SDKs

Qualcomm provides programmers with a set of libraries, the Symphony System Manager SDK, to allow for tighter control over the various compute units (i.e. CPU, GPU and DSP) within their processors. Namely, it includes resources for task scheduling, heterogeneous offload, and power and thermal management.

Simply [download the latest version for Linux](https://developer.qualcomm.com/software/symphony-system-manager-sdk "Symphony System Manager SDK") and install it with `sudo dpkg -i SYMPHONY_PACKAGE_NAME`. For example, if the file is called `libsymphony-1.1.2.deb`, then `sudo dpkg -i libsymphony-1.1.2.deb` should be the command to issue. This will decompress the libraries to `/opt/Qualcomm/Symphony/SYMPHONY_VERSION/` (e.g. `/opt/Qualcomm/Symphony/1.1.2/`). At this point, it is recommended to verify the installation by following the instructions described in the provided documentation which should be available in `/opt/Qualcomm/Symphony/SYMPHONY_VERSION/docs/`.



## Useful links

Additional information can be found in the [Android Developers' website](https://developer.android.com/studio/run/device.html "Android Studio user guide") and in [Nicolas Bernaerts' website](http://bernaerts.dyndns.org/linux/74-ubuntu/328-ubuntu-trusty-android-adb-fastboot-qtadb "Ubuntu 14.04 - Install Android tools").

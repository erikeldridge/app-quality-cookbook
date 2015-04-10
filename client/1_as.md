# Android Studio

Android Studio (AS) is built on the IntelliJ platform and provides a mature IDE we can use for Android development.

## Install

[Download](https://developer.android.com/sdk/index.html) and [install](https://developer.android.com/sdk/installing/index.html?pkg=studio) AS.

Accept the android-sdk-license and intel-android-extra-license. The former is for Android. The later lets us use [Intel's HAXM emulator accelerator](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager), which speeds up development.

On Mac, the setup process [requests permission](https://code.google.com/p/android/issues/detail?id=81761) to install the HAXM accelerator. Grant permission.

An IntelliJ welcome screen will launch after installation completes. Click the Configure button and then the SDK Manager button.

Follow [Adding SDK Packages](https://developer.android.com/sdk/installing/adding-packages.html) instructions to install recommended packages. You should have most installed already. Select the Intel x86 image, so we can use HAXM, and the SDK sources, so we can easily see Android open source code in our IDE.

## Hello world

Follow the Android's instructions for [Creating an Android Project](https://developer.android.com/training/basics/firstapp/creating-project.html).

Per [Android’s distribution dashboard](https://developer.android.com/about/dashboards/index.html?utm_source=suzunone), we’ll cover most devices if we have version 2.2 and up.

Click the button on the welcome screen to create a new app. Accept all the defaults.

Click the run button to build your app and launch an emulator. Observe your hello world app running in the emulator.

# Emulator

The Android [emulator](http://developer.android.com/tools/devices/emulator.html) provides a VM we can run our apps in. The VM is called an emulator because it emulates device hardware. This explains why there are many emulator options and why they can be very slow.

You can create new VM configurations using the [Android Virtual Device (AVD) Manager](http://developer.android.com/tools/devices/managing-avds.html).

For development, we want fast feedback. [Intel's HAXM accelerator](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) speeds up VM performance on Intel host machines, which is why we installed it along with AS. Using a fast, hardware device, or [Genymotion](https://www.genymotion.com), are other options.

We can use standard emulators in a test environment when the value of validating functionality on a diversity of devices outweighs the cost of running a slow emulator.
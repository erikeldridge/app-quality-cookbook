# Emulator

The Android emulator provides a VM we can run our apps in. The VM is called an emulator because it emulates device hardware. This exaplins why there are many emulator options and why they can be very slow.

For development, we want fast feedback. [Intel's HAXM accelerator](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) speeds up VM performance on Intel host machines, which is why we installed it above. Using a fast, real device, or [Genymotion](https://www.genymotion.com), are other options.

We can use standard emulators in a test environment when the value of validating functionally on a diversity of devices outweighs the cost of running them.
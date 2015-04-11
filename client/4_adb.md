# Android Debug Bridge

The [Android Debug Bridge (ADB)](http://developer.android.com/tools/help/adb.html) provides command-line access to our app. It works with emulators and [hardware](http://developer.android.com/tools/device.html) devices.

You can find the ADB executable in _&lt;sdk&gt;/platform-tools/_, but it should also be available on your command line.

Use ADB to list attached devices:

```
]$ adb devices
List of devices attached
emulator-5554 device
```

Observe your device listed in the output.

The [logcat](http://developer.android.com/tools/help/logcat.html) command provides us with a way to see the device log:

```
$ adb logcat
--------- beginning of main
W/OpenGLRenderer( 1480): Incorrectly called buildLayer on View: ShortcutAndWidgetContainer, destroying layer...
--------- beginning of system
I/ActivityManager( 1234): START u0 {act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10000000 cmp=com.example.erik.myapplication/.MainActivity (has extras)} from uid 10007 on display 0
W/AudioTrack( 1234): AUDIO_OUTPUT_FLAG_FAST denied by client
W/EGL_emulation( 2569): eglSurfaceAttrib not implemented
W/OpenGLRenderer( 2569): Failed to set EGL_SWAP_BEHAVIOR on surface 0xb45f4380, error=EGL_SUCCESS
I/ActivityManager( 1234): Displayed com.example.erik.myapplication/.MainActivity: +118ms
...
```

It can be noisy, though. Fortunately, logcat enables us to filter by [log level](http://developer.android.com/reference/java/util/logging/Level.html).

For example, we can show only the _error_ level:

```
$ adb logcat *:e
--------- beginning of main
--------- beginning of system
E/memtrack( 2621): Couldn't load memtrack module (No such file or directory)
E/android.os.Debug( 2621): failed to load memtrack module: -2
E/InputDispatcher( 1234): channel '17246e9b com.example.erik.myapplication/com.example.erik.myapplication.MainActivity (server)' ~ Channel is unrecoverably broken and will be disposed!
E/memtrack( 2639): Couldn't load memtrack module (No such file or directory)
E/android.os.Debug( 2639): failed to load memtrack module: -2
...
```

Edit `onCreate` to [log](http://developer.android.com/tools/debugging/debugging-log.html) with _debug_ level the action that launched your app:

```java
public class MainActivity extends ActionBarActivity {
    private static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Intent intent = getIntent();
        Log.d(TAG, "action=" + intent.getAction());
        ...
```

Rerun to compile and install your app. Background and foreground your app to generate log entries.

Filter by _MainActivity_ tag, debug level, and suppress everything else:

```
$ adb logcat MainActivity:d *:s
--------- beginning of main
--------- beginning of system
D/MainActivity( 2649): action=android.intent.action.MAIN
D/MainActivity( 2649): action=android.intent.action.MAIN
D/MainActivity( 2649): action=android.intent.action.MAIN
...
```

## Observe logcat in AS

Open the _Android_ window in AS by navigating to _View > Tool Windows > Android_. Click on the _Devices | Logcat_ tab.

Filter by level, tag, and package name.

## Shell

We can log into our device using ADB in a similar way to how we used `vagrant ssh` to log into our Vagrant VM:

```
$ adb shell
```

Observe you can use a subset of standard Unix navigation utilities on the device:

```
# pwd
/
# ls
acct
cache
charger
...
# cd data/data/com.example.erik.myapplication
# pwd
/data/data/com.example.erik.myapplication
# ls
cache
lib
# exit
$
```

## Pulling files

We don't have much on the device at this time, but if we write files from the application, we can pull them onto our host machine for inspection using ADB. To get some experience, push a file, shell in to verify it's there, and then pull it back out.

```
$ echo "test" > foo.txt
$ adb push foo.txt /data/data/com.example.erik.myapplication/foo.txt
1 KB/s (5 bytes in 0.002s)
$ adb shell
# cd /data/data/com.example.erik.myapplication
# ls
cache
foo.txt
lib
# cat foo.txt                                                                   <
test
# exit
$ adb pull /data/data/com.example.erik.myapplication/foo.txt bar.txt
2 KB/s (5 bytes in 0.002s)
$ ls
bar.txt          foo.txt
$ cat bar.txt
test
```

## Learn more

Use [Android's Dalvik Debug Monitor Server (DDMS)](http://developer.android.com/tools/debugging/ddms.html) for dynamic analysis, monitoring, and file system operations.
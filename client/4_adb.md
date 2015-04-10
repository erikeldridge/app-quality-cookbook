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

Filter by [log level](http://developer.android.com/reference/java/util/logging/Level.html):

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

## Learn more

Use [PID Cat](https://github.com/JakeWharton/pidcat) to pretty print logcat, filtered by package name
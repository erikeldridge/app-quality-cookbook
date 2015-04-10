# Android Studio

Android Studio (AS) is built on the IntelliJ platform and provides a mature IDE we can use for Android development.

## Install

[Download](https://developer.android.com/sdk/index.html) and [install](https://developer.android.com/sdk/installing/index.html?pkg=studio) AS.

Accept the android-sdk-license and intel-android-extra-license. The former is for Android. The later lets us use [Intel's HAXM emulator accelerator](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager), which speeds up development.

On Mac, the setup process [requests permission](https://code.google.com/p/android/issues/detail?id=81761) to install the HAXM accelerator. Grant permission.

An IntelliJ welcome screen will launch after installation completes. Click the Configure button and then the SDK Manager button.

Follow [Adding SDK Packages](https://developer.android.com/sdk/installing/adding-packages.html) instructions to install recommended packages. You should have most installed already. Select the Intel x86 image, so we can use HAXM, and the SDK sources, so we can easily see Android open source code in our IDE.

## Functional test

Follow the Android's instructions for [Creating an Android Project](https://developer.android.com/training/basics/firstapp/creating-project.html).

Per [Android’s distribution dashboard](https://developer.android.com/about/dashboards/index.html?utm_source=suzunone), we’ll cover most devices if we have version 2.2 and up.

Click the button on the welcome screen to create a new app. Accept all the defaults.

Click the run button to build your app and launch an emulator. Observe your hello world app running in the emulator.

## Integration test

In AS, control-click on ApplicationTest and select _Run 'ApplicationTest'_ in the context menu.

Observe all your tests pass.

## Commit

You've got a working version, so commit your changes:

```
$ git init
Initialized empty Git repository in /Users/erik/AndroidStudioProjects/MyApplication/.git/
[tw-mbp-erik MyApplication (master)]$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  .gitignore
  .idea/
  MyApplication.iml
  app/
  build.gradle
  gradle.properties
  gradle/
  gradlew
  gradlew.bat
  settings.gradle
$ git add .
$ git commit -m "Create hello world app"
$ git commit -m "Create hello world app"
[master (root-commit) ac5df1d] Create hello world app
 24 files changed, 472 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 app/.gitignore
 create mode 100644 app/build.gradle
 create mode 100644 app/proguard-rules.pro
 create mode 100644 app/src/androidTest/java/com/example/erik/myapplication/ApplicationTest.java
 create mode 100644 app/src/main/AndroidManifest.xml
 create mode 100644 app/src/main/java/com/example/erik/myapplication/MainActivity.java
 create mode 100644 app/src/main/res/layout/activity_main.xml
 create mode 100644 app/src/main/res/menu/menu_main.xml
 create mode 100644 app/src/main/res/mipmap-hdpi/ic_launcher.png
 create mode 100644 app/src/main/res/mipmap-mdpi/ic_launcher.png
 create mode 100644 app/src/main/res/mipmap-xhdpi/ic_launcher.png
 create mode 100644 app/src/main/res/mipmap-xxhdpi/ic_launcher.png
 create mode 100644 app/src/main/res/values-w820dp/dimens.xml
 create mode 100644 app/src/main/res/values/dimens.xml
 create mode 100644 app/src/main/res/values/strings.xml
 create mode 100644 app/src/main/res/values/styles.xml
 create mode 100644 build.gradle
 create mode 100644 gradle.properties
 create mode 100644 gradle/wrapper/gradle-wrapper.jar
 create mode 100644 gradle/wrapper/gradle-wrapper.properties
 create mode 100755 gradlew
 create mode 100644 gradlew.bat
 create mode 100644 settings.gradle
```
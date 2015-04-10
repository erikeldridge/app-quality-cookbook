
# Dive into Android

Android is a Linux-based operating system and uses Java syntax for application development, so our Unix and Java experience will carry over.

The Android platform defines four [fundamental building blocks](http://developer.android.com/guide/components/fundamentals.html) (activities, services, receivers, providers). For a few reasons [2], however, we'll start by exploring a set of concepts we'll use day-to-day:
* _Activities_, _Views_, and _Intents_ for application logic
* _Manifests_, _Gradle_, and resources for project management

## Activities

The Android [Activity](http://developer.android.com/reference/android/app/Activity.html) class defines how your app responds to [lifecycle](http://developer.android.com/reference/android/app/Activity.html#ActivityLifecycle) events like launching, backgrounding, and stopping.

Conceptually, an Activity corresponds with "a single, focused thing that the user can do." [1]. For example, if we want to enable an app user to compose an email, we can define a _ComposeActivity_.

In AS, open the MainActivity you created above. Set a breakpoint in the `super.onCreate` call in the onCreate method. Note that `setContentView`, which defines the content to show, is called just after this.

Run the debugger by clicking the _debug_ button on it. After the emulator launches and hits your breakpoint, observe no text appears on the emulator screen. Click the _resume program_ button to complete Activity creation and observe your text appears.

## Views

[View](http://developer.android.com/guide/topics/ui/overview.html) classes define the content shown in an activity.

[Layout](http://developer.android.com/guide/topics/ui/declaring-layout.html) classes are views used to position elements on the screen. We can define layouts using Java or XML. You can see a reference to a layout (`R.layout.activity_main`) in the `onCreate` method we looked at above.

Control-click on `R.layout.activity_main`, select _Go To > Declaration_, and observe we're using a `TextView` to show "Hello World!" (via `android:text="@string/hello_world"`).

## Intents

We use [Intents](http://developer.android.com/guide/components/intents-filters.html) to launch other activities and apps. Intents are literally named. They represent the intent of the app user, e.g., the user intended to share a link, and apps can elect to perform actions in response.

Edit `onCreate` to fetch the intent that launched our app and inject it into our view:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Intent intent = getIntent();
    setContentView(R.layout.activity_main);
}
```

Set a breakpoint on the line we call `setContentView` and rerun the debugger.

When the app hits the breakpoint, open the _Evaluate Expression_ window from the _Debug_ panel, as [described in the Android debug documentation](https://developer.android.com/tools/debugging/debugging-studio.html#breakPointsDebug). Use ths window to evaluate `intent.getAction()`.

Observe the action is "android.intent.action.MAIN". In Java terminology, this activity is acting as the _main_ method for the app. In Android terminology, our app has elected to use this activity to handle a launch intent.

## Manifests

An app's [manifest](http://developer.android.com/guide/topics/manifest/manifest-intro.html) is an XML file that defines metadata about the app.

Notably, we use the manifest to declare:
* your app's name
* the min and max (target) Android versions supported by your app
* all the activities in your app
* the [permissions](http://developer.android.com/reference/android/Manifest.permission.html) required by your app
* intents your app responds to, including the main action

## Gradle

We've used Maven up until now to manage our Java projects. Android uses something similar called [Gradle](https://gradle.org/).

Maven defines configuration using XML in a file called _pom.xml_. Android defines configuration using [Groovy](http://groovy-lang.org/) in files called [build.gradle](https://developer.android.com/tools/building/configuring-gradle.html#buildFileBasics).

Take a look at the build files in your hello world app. There's one for _module_ settings and one for _project_ settings. Observe the following:
* Android SDK _target_ and _min_ versions, which supersede those declared in your manifest
* the _repositories_ and _dependencies_ sections correspond to Maven configuration. Like Maven, Android uses the Maven Central respoitory by default
* Gradle supports the notion of _build variants_, i.e., build-time configuration for your app 

## Resources

Like Maven, Android defines a [standard project structure](https://developer.android.com/tools/projects/index.html#ProjectFiles).Android's resource support is worth mentioning in particular.

Resource files live in the _main/res/_ directory, as opposed to _src/main/resources_ for a Maven project.

Android generates IDs when a project is compiled that we can use to reference our resources. We've seen this when we referenced `R.layout.activity_main` in `onCreate`, and saw `@string/hello_world` in _activity_main.xml_. The [R](http://developer.android.com/reference/android/R.html) refers to "resources".

Android does an excellent job providing [appropriate resources](http://developer.android.com/guide/topics/resources/index.html) with respect to [localization](http://developer.android.com/training/basics/supporting-devices/languages.html) and [device capabilities](http://developer.android.com/training/basics/supporting-devices/screens.html).

## Footnotes

* [1] Quote from [Android's Activity documentation](http://developer.android.com/reference/android/app/Activity.html)
* [2] For example, [Advocating Against Fragments](https://corner.squareup.com/2014/10/advocating-against-android-fragments.html), [RxJava-based organization](https://twitter.com/jakewharton/status/385898996884971520), [MVP organization](http://fernandocejas.com/2014/09/03/architecting-android-the-clean-way/), and the fact that we launched a hello world app with only an Activity.
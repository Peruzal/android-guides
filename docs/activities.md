description: Android Activities


## Activity Lifecycle Background

As a user navigates throughout an app, Android maintains the visited activities in a stack, with the currently visible activity always placed at the top of the stack. 

At any point in time a particular activity can be in one of the following 4 states:

| Activity State | Description |
| -------------- |-------------|
| Running | Activity is visible and interacting with the user |
| Paused | Activity is still visible, but no longer interacting with the user |
| Stopped | Activity is no longer visible |
| Killed | Activity has been killed by the system (low memory) or its `finish()` method has been called |

## Activity Lifecycle

The following diagram shows the important state paths of an Activity. The square rectangles represent callback methods you can implement to perform operations when the Activity moves between states. These are described further in the table below the diagram.

![Imgur](https://i.imgur.com/aUd1KA1.png)

| Lifecycle Method | Description | Common Uses  |
| ------------- |-------------| -----|
| `onCreate()` | The activity is starting (but not visible to the user) | Most of the activity initialization code goes here. This is where you [setContentView()](http://developer.android.com/reference/android/app/Activity.html#setContentView(int)) for the activity, initialize views, set up any adapters, etc. |
| `onStart()` | The activity is now visible (but not ready for user interaction) | This lifecycle method isn't used much, but can come in handy to register a BroadcastReceiver to monitor for changes that impact the UI (since the UI is now visible to the user).  |
| `onResume()` | The activity is now in the foreground and ready for user interaction | This is a good place to start animations, open exclusive-access devices like the camera, etc. |
| `onPause()` | Counterpart to `onResume()`. The activity is about to go into the background and has stopped interacting with the user. This can happen when another activity is launched in front of the current activity. | It's common to undo anything that was done in onResume() and to save any global state (such as writing to a file). |
| `onStop()` | Counterpart to `onStart()`. The activity is no longer visible to the user. | It's common to undo anything that was done in `onStart()`. |
| `onDestroy()` | Counterpart to `onCreate(...)`. This can be triggered because `finish()` was called on the activity or the system needed to free up some memory. |  It's common to do any cleanup here. For example, if the activity has a thread running in the background to download data from the network, it may create that thread in onCreate() and then stop the thread here in onDestroy() |
| `onRestart()` | Called when the activity has been stopped, before it is started again | It isn't very common to need to implement this callback.    |

### Handling Configuration Changes

The Activity lifecycle is especially important because whenever an activity leaves the screen, **the activity can be destroyed**. When an activity is destroyed, when the user returns to the activity, the activity will be re-created and the lifecycle methods will be called again. Activities are also **re-created whenever the orientation changes** (i.e the screen is rotated). In order to ensure your application is robust to recreation amongst a wide variety of situations, be sure to review the [[handling configuration changes|Handling-Configuration-Changes]].

### Calling the super class

When overriding any of the methods, you may need to call the superclass implementation.  The rule of thumb is that during initialization, you should always call the superclass first:

```java
public void onCreate() {
   super.onCreate();
   // do work after super class function
   // setContentView(R.layout.main);
}
```

During de-initialization, you should do the work first before calling the super class:

```java
public void onPause() {
   // do work here first before super class function
   // LocalBroadcastManager.getInstance(this).unregisterReceiver(mMessageReceiver);
   super.onPause();
}
```

See this [Stack Overflow article](http://stackoverflow.com/questions/16925579/android-implementation-of-lifecycle-methods-can-call-the-superclass-implementati) for more context.

## Handling Configuration Changes

There are various situations such as when the screen orientation is rotated where the Activity can actually be destroyed and removed from memory and then re-created from scratch again. In these situations, the best practice is to prepare for cases where the Activity is re-created by properly saving and restoring the state.

## Saving and Restoring Activity State

As your activity begins to stop, the system calls `onSaveInstanceState()` so your activity can save state information with a collection of key-value pairs. The default implementation of this method **automatically saves** information about the state of the activity's **view hierarchy**, such as the text in an `EditText` widget or the scroll position of a `ListView`.

To save additional state information for your activity, you must implement `onSaveInstanceState()` and add key-value pairs to the Bundle object. For example:

```java
public class MainActivity extends Activity {
    static final String SOME_VALUE = "int_value";
    static final String SOME_OTHER_VALUE = "string_value";

    @Override
    protected void onSaveInstanceState(Bundle savedInstanceState) {
        // Save custom values into the bundle
        savedInstanceState.putInt(SOME_VALUE, someIntValue);
        savedInstanceState.putString(SOME_OTHER_VALUE, someStringValue);
        // Always call the superclass so it can save the view hierarchy state
        super.onSaveInstanceState(savedInstanceState);
    }
}
```

The system will call that method before an Activity is destroyed. Then later the system will call `onRestoreInstanceState` where we can restore state from the bundle:

```java
@Override
protected void onRestoreInstanceState(Bundle savedInstanceState) {
    // Always call the superclass so it can restore the view hierarchy
    super.onRestoreInstanceState(savedInstanceState);
    // Restore state members from saved instance
    someIntValue = savedInstanceState.getInt(SOME_VALUE);
    someStringValue = savedInstanceState.getString(SOME_OTHER_VALUE);
}
```

Instance state can also be restored in the standard `Activity#onCreate` method but it is convenient to do it in `onRestoreInstanceState` which ensures all of the initialization has been done and allows subclasses to decide whether to use the default implementation. Read [this stackoverflow post](http://stackoverflow.com/a/14676555/313399) for details.

Note that `onSaveInstanceState` and `onRestoreInstanceState` are not guaranteed to be called together. Android invokes `onSaveInstanceState()` when there's a chance the activity might be destroyed. However, there are cases where `onSaveInstanceState` is called but the activity is not destroyed and as a result `onRestoreInstanceState` is not invoked.

Read more on the [Recreating an Activity](http://developer.android.com/training/basics/activity-lifecycle/recreating.html) guide.

## Saving and Restoring Fragment State

Fragments also have a `onSaveInstanceState()` method which is called when their state needs to be saved:

```java
public class MySimpleFragment extends Fragment {
    private int someStateValue;
    private final String SOME_VALUE_KEY = "someValueToSave";
   
    // Fires when a configuration change occurs and fragment needs to save state
    @Override
    protected void onSaveInstanceState(Bundle outState) {
        outState.putInt(SOME_VALUE_KEY, someStateValue);
        super.onSaveInstanceState(outState);
    }
}
```

Then we can pull data out of this saved state in `onCreateView`:

```java
public class MySimpleFragment extends Fragment {
   // ...

   // Inflate the view for the fragment based on layout XML
   @Override
   public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.my_simple_fragment, container, false);
        if (savedInstanceState != null) {
            someStateValue = savedInstanceState.getInt(SOME_VALUE_KEY);
            // Do something with value if needed
        }
        return view;
   }
}
```

For the fragment state to be saved properly, we need to be sure that we aren't **unnecessarily recreating the fragment** on configuration changes. This means being careful not to reinitialize existing fragments when they already exist. Any fragments being initialized in an Activity need to be **looked up by tag** after a configuration change:

```java
public class ParentActivity extends AppCompatActivity {
    private MySimpleFragment fragmentSimple;
    private final String SIMPLE_FRAGMENT_TAG = "myfragmenttag";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        if (savedInstanceState != null) { // saved instance state, fragment may exist
           // look up the instance that already exists by tag
           fragmentSimple = (MySimpleFragment)  
              getSupportFragmentManager().findFragmentByTag(SIMPLE_FRAGMENT_TAG);
        } else if (fragmentSimple == null) { 
           // only create fragment if they haven't been instantiated already
           fragmentSimple = new MySimpleFragment();
        }
    }
}
```

This requires us to be careful to **include a tag for lookup** whenever putting a fragment into the activity within a transaction:

```java
public class ParentActivity extends AppCompatActivity {
    private MySimpleFragment fragmentSimple;
    private final String SIMPLE_FRAGMENT_TAG = "myfragmenttag";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ... fragment lookup or instantation from above...
        // Always add a tag to a fragment being inserted into container
        if (!fragmentSimple.isInLayout()) {
            getSupportFragmentManager()
                .beginTransaction()
                .replace(R.id.container, fragmentSimple, SIMPLE_FRAGMENT_TAG)
                .commit();
        }
    }
}
```

With this simple pattern, we can properly re-use fragments and restore their state across configuration changes.



## Locking Screen Orientation

If you want to lock the screen orientation change of any screen (activity) of your android application you just need to set the `android:screenOrientation` property of an `<activity>` within the `AndroidManifest.xml`:

```xml
<activity
    android:name="com.techblogon.screenorientationexample.MainActivity"
    android:screenOrientation="portrait"
    android:label="@string/app_name" >
    <!-- ... -->
</activity>
```

Now that activity is forced to always be displayed in "portrait" mode. 

## Manually Managing Configuration Changes

If your application doesn't need to update resources during a specific configuration change and you have a performance limitation that requires you to avoid the activity restart, then you can declare that your activity handles the configuration change itself, which prevents the system from restarting your activity. 

However, this technique should be considered **a last resort** when you must avoid restarts due to a configuration change and is not recommended for most applications. To take this approach, we must add the `android:configChanges` node to the activity within the `AndroidManifest.xml`:

```xml
<activity android:name=".MyActivity"
          android:configChanges="orientation|screenSize|keyboardHidden"
          android:label="@string/app_name">
```

Now, when one of these configurations change, the activity does not restart but instead receives a call to `onConfigurationChanged()`:

```java
// Within the activity which receives these changes
// Checks the current device orientation, and toasts accordingly
@Override
public void onConfigurationChanged(Configuration newConfig) {
    super.onConfigurationChanged(newConfig);

    // Checks the orientation of the screen
    if (newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE) {
        Toast.makeText(this, "landscape", Toast.LENGTH_SHORT).show();
    } else if (newConfig.orientation == Configuration.ORIENTATION_PORTRAIT){
        Toast.makeText(this, "portrait", Toast.LENGTH_SHORT).show();
    }
}
```

See the [Handling the Change](http://developer.android.com/guide/topics/resources/runtime-changes.html#HandlingTheChange) docs. For more about which configuration changes you can handle in your activity, see the [android:configChanges](http://developer.android.com/guide/topics/manifest/activity-element.html#config) documentation and the [Configuration](http://developer.android.com/reference/android/content/res/Configuration.html) class.


## Migrating to the AppCompat Library

The [AppCompat](https://developer.android.com/tools/support-library/features.html#v7) support library enables the use of the ActionBar and Material Design specific implementations such as [Toolbar](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html) for older devices down to Android v2.1.   Currently, new projects created through Android Studio incorporate this library by default.  You can confirm by looking at the `build.gradle` file to see the AppCompat library being set:

```gradle
android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"
}

dependencies {
    compile 'com.android.support:appcompat-v7:25.2.0'
}
```

Note that the AppCompat library has an implicit dependency on the support-v4 library.  The support-v4 declaration however does not necessarily need to be declared.  Since the release of the support-v4 version 24.2.0, the library has been [divided into separate modules](https://developer.android.com/topic/libraries/support-library/revisions.html): `support-compat`, `support-core-utils`, `support-core-ui`, `support-media-compat`, and `support-fragment`.  However, because the AppCompat library usually depends on `support-fragment`, which still contains a dependency to all the other modules, you currently cannot take advantage of this change to reduce the number of overall dependencies.

Also notice that once you upgrade to AppCompat v7 v24, you will also be forced to update your Build Tools and `compileSDKVersion` to API 24 too.

There is a current [bug](https://code.google.com/p/android/issues/detail?id=183149) that precludes you from compiling to lower versions.   Once you are using this API version 23 or higher, be aware that the Apache HTTP Client library has been [removed](https://developer.android.com/preview/behavior-changes.html#behavior-apache-http-client).

## Search and replacing changes

Older projects may not include this library, so migrating requires changing the theme references and many of the main imports described in this [blog post](http://android-developers.blogspot.com/2014/10/appcompat-v21-material-design-for-pre.html).  Because the support class declarations are not compatible with the standard Android ones, you need to make sure you are using the imports from the support library entirely.  Otherwise your app is likely to crash.

The simplest is often to do a search-and-replace to start changing the following statements to start using the support libraries.

#### Activities Changes

 * `import android.app.Activity` -> `import android.support.v7.app.AppCompatActivity`
 * `extends Activity` -> `extends AppCompatActivity`

#### Fragment Changes

 * `extends FragmentActivity` -> `extends AppCompatActivity`
 * `import android.support.v4.app.FragmentActivity` -> `import android.support.v7.app.AppCompatActivity` 
 * `import android.app.Fragment` -> `import android.support.v4.app.Fragment`
 * `getFragmentManager()` -> `getSupportFragmentManager()`

#### ActionBar Changes

 * `import android.app.ActionBar` -> `import android.support.v7.app.ActionBar`
 * `getActionBar()` -> `getSupportActionBar()`

#### AlertDialog Changes

Your AlertDialogs should import from the AppCompat support library instead, which takes advantage of the new Material Design theme.

<img src="https://blog.xamarin.com/wp-content/uploads/2015/05/alertdialogs.png"/>

 * `import android.app.AlertDialog` -> `import android.support.v7.app.AlertDialog`

#### Theme XML Changes

If you were migrating from the Holo theme, your new theme  would inherit from `Theme.AppCompat` instead of `android:Theme.Holo.Light`:

```xml
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
```

If you wish to have a style that disables the ActionBar in certain Activity screens but still wish to use many of the ones you custom defined, you can inherit from `AppTheme` and apply the same styles declared in `Theme.AppCompat.NoActionBar`:

```xml
    <style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>
```
If you see `AppCompat does not support the current theme features`, it's likely the `windowNoTitle` setting has not been set or is set to false.  There is more strict enforcement on what this value must be set on newer AppCompat libraries.  See this [Stack Overflow](http://stackoverflow.com/questions/29790070/upgraded-to-appcompat-v22-1-0-and-now-getting-illegalargumentexception-appcompa) article for more context.

#### Menu XML Changes

For your menu/ layout files, add an `app:` namespace .  For menu items, the `showAsAction` must be from the `app` namespace instead of `android` namespace.  It is considered a custom attribute of the support library and will no otherwise be processed correctly without making this change.

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android" xmlns:app="http://schemas.android.com/apk/res-auto">
 <item android:id="@+id/myMenuItem"
       android:title="@string/select"
       android:showAsAction="ifRoom"
       app:showAsAction="ifRoom"
```

If you are using widgets such as the SearchView, you must also use `android.support.v7.widget.SearchView` instead of `android.widget.SearchView`.  Note that the `app` namespace must also be used.

```xml
+<menu xmlns:android="http://schemas.android.com/apk/res/android" xmlns:app="http://schemas.android.com/apk/res-auto">
 
     <item android:id="@+id/contentSearch"
           android:orderInCategory="2"
           android:title="@string/search"
           app:showAsAction="ifRoom"
           app:actionViewClass="android.support.v7.widget.SearchView">
```

#### Changes to Menu Options

The `MenuItemCompat` helper class has a few static methods that should be used in lieu of `MenuItem`:

```java
  @Override
    public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
        inflater.inflate(R.menu.my_menu, menu);
        mSearchView = (SearchView) MenuItemCompat.getActionView(menu.findItem(R.id.contentSearch));
```

#### Changing targetSDKVersion

In addition, setting the `targetSdkVersion` to the latest SDK version ensures that the  AppCompat library will attempt to apply the Material Design assuming the device itself can support it. The support library will still check to see if the minimum SDK version is being used on the device.

```gradle
android {
    targetSdkVersion 24
```

### Known issues

The AppCompat library has issues with Samsung v4.2.2 devices.  See [this issue](https://code.google.com/p/android/issues/detail?id=78377) for more details.


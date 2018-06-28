description: Understanding Android Permissions

# Android Permissions

## Understanding App Permissions

By default, an Android app starts with zero permissions granted to it. When the app needs to use any of the protected features of the device (sending network requests, accessing the camera, sending an SMS, etc) it must obtain the appropriate permission from the user to do so.

Before Marshmallow, permissions were handled at install-time and specified in the `AndroidManifest.xml` within the project. Full list of permissions can be [found here](http://developer.android.com/reference/android/Manifest.permission.html). After Marshmallow, permissions must now be **requested at runtime** before being used. There are a [number of libraries available](https://gist.github.com/dlew/2a21b06ee8715e0f7338) to make runtime permissions easier. If you to get started quickly, check out our guide on [[managing runtime permissions with PermissionsDispatcher]].

### Permissions before Marshmallow

Permissions were much simpler before Marshmallow (API 23). All permissions were handled at install-time. When a user went to install an app from the [Google Play Store](https://play.google.com/store), the user was presented a list of permissions that the app required (some people referred to this as a "wall of permissions". The user could either accept **all** the permissions and continue to install the app or decide not to install the app. It was an all or nothing approach. There was no way to grant only certain permissions to the app and no way for the user to revoke certain permissions after the app was installed. 

Example of pre-Marshmallow permissions requested by the Dropbox app: 

![Imgur](http://i.imgur.com/XeRdzOT.png)

For an app developer, permissions were very simple. To request [one of the many permissions](http://developer.android.com/reference/android/Manifest.permission.html), simply specify it in the `AndroidManifest.xml`:

For example, an application that needs to read the user's contacts would add the following to it's `AndroidManifest.xml`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.android.app.myapp" >
    
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    ...
</manifest>
```

That's all there was to it. The user had no way of changing permissions, even after installing the app. This made it easy for developers to deal with permissions, but wasn't the best user experience.

## Permission Updates in Marshmallow
 
Marshmallow brought large changes to the permissions model. It introduced the concept of runtime permissions. These are permissions that are requested while the app is running (instead of before the app is installed). These permission can then be allowed or denied by the user. For approved permissions, these can also be revoked at a later time.

This means there are a couple more things to consider when working with permissions for a **Marshmallow** app. Keep in mind that your `targetSdkVersion` must be >= `23` and your emulator / device must be running Marshmallow to see the new permissions model. If this isn't the case, see the [[backwards compatibility|Understanding App Permissions#backwards-compatibility]] section to understand how permissions will behave on your configuration.

### Normal Permissions

When you need to add a new permission, first check [this page](http://developer.android.com/guide/topics/security/normal-permissions.html) to see if the permission is considered a [`PROTECTION_NORMAL`](http://developer.android.com/reference/android/content/pm/PermissionInfo.html#PROTECTION_NORMAL) permission. In Marshmallow, Google has designated certain permissions to be "safe" and called these "Normal Permissions". These are things like `ACCESS_NETWORK_STATE`, `INTERNET`, etc. which can't do much harm. Normal permissions are automatically granted at install time and **never** prompt the user asking for permission.

**Important:** Normal Permissions must be added to the `AndroidManifest`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.android.app.myapp" >
    
    <uses-permission android:name="android.permission.INTERNET" />
    ...
</manifest>
```

### Runtime Permissions

If the permission you need to add isn't listed under the normal permissions, you'll need to deal with "Runtime Permissions". Runtime permissions are permissions that are requested as they are needed while the app is running. These permissions will show a dialog to the user, similar to the following one:

![Imgur](http://i.imgur.com/er8yE9J.png)

The first step when adding a "Runtime Permission" is to add it to the `AndroidManifest`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.codepath.androidpermissionsdemo" >

    <uses-permission android:name="android.permission.READ_CONTACTS" />
    ...
</manifest>
```

Next, you'll need to initiate the permission request and handle the result. The following code shows how to do this in the context of an `Activity`, but this is also possible from within a `Fragment`. 

```java
// MainActivity.java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // In an actual app, you'd want to request a permission when the user performs an action
        // that requires that permission.
        getPermissionToReadUserContacts();
    }

    // Identifier for the permission request
    private static final int READ_CONTACTS_PERMISSIONS_REQUEST = 1;

    // Called when the user is performing an action which requires the app to read the
    // user's contacts
    public void getPermissionToReadUserContacts() {
        // 1) Use the support library version ContextCompat.checkSelfPermission(...) to avoid
        // checking the build version since Context.checkSelfPermission(...) is only available
        // in Marshmallow
        // 2) Always check for permission (even if permission has already been granted)
        // since the user can revoke permissions at any time through Settings
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED) {

            // The permission is NOT already granted.
            // Check if the user has been asked about this permission already and denied
            // it. If so, we want to give more explanation about why the permission is needed.
            if (shouldShowRequestPermissionRationale(
                    Manifest.permission.READ_CONTACTS)) {
                // Show our own UI to explain to the user why we need to read the contacts
                // before actually requesting the permission and showing the default UI
            }

            // Fire off an async request to actually get the permission
            // This will show the standard permission request dialog UI
            requestPermissions(new String[]{Manifest.permission.READ_CONTACTS},
                    READ_CONTACTS_PERMISSIONS_REQUEST);
        }
    }

    // Callback with the request from calling requestPermissions(...)
    @Override
    public void onRequestPermissionsResult(int requestCode,
                                           @NonNull String permissions[],
                                           @NonNull int[] grantResults) {
        // Make sure it's our original READ_CONTACTS request
        if (requestCode == READ_CONTACTS_PERMISSIONS_REQUEST) {
            if (grantResults.length == 1 &&
                    grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, "Read Contacts permission granted", Toast.LENGTH_SHORT).show();
            } else {
                // showRationale = false if user clicks Never Ask Again, otherwise true
                boolean showRationale = shouldShowRequestPermissionRationale( this, Manifest.permission.READ_CONTACTS);

                if (showRationale) {
                   // do something here to handle degraded mode
                } else {
                   Toast.makeText(this, "Read Contacts permission denied", Toast.LENGTH_SHORT).show();
                }
            }
        } else {
            super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
}
```

### Permission Groups

[Permission Groups](http://developer.android.com/preview/features/runtime-permissions.html#perm-groups) avoids spamming the user with a lot of permission requests while allowing the app developer to only request the minimal amount of permissions needed at any point in time.

Related permissions are grouped into [one of the permission groups](http://developer.android.com/reference/android/Manifest.permission_group.html). When an app requests a permission that belongs to a particular permission group (i.e. [READ_CONTACTS](http://developer.android.com/reference/android/Manifest.permission.html#READ_CONTACTS)), Android asks the user about the higher level group instead ([CONTACTS](http://developer.android.com/reference/android/Manifest.permission_group.html#CONTACTS)). This way when the app later needs the [WRITE_CONTACTS](http://developer.android.com/reference/android/Manifest.permission.html#WRITE_CONTACTS) permission, Android can automatically grant this itself without prompting the user.

In most of your interaction with the permission API's you'll be working with the individual permissions and not the permission groups, but pay close attention to what the API expects as both permissions and permission groups are Strings.

### Backwards Compatibility

There are 2 main scenarios to think about when it comes to backwards compatibility:

1. Your app is targeting an API less than Marshmallow (`TargetSdkVersion` < `23`), but the emulator / device is Marshmallow:
  * Your app will continue to use the old permissions model.
  * All permissions listed in the `AndroidManifest` will be asked for at install time.
  * Users will be able to revoke permissions after the app is installed. It's important to test this scenario since the results of certain actions without the appropriate permission can be unexpected. 
2. The emulator / device is running something older than Marshmallow, but you app targets Marshmallow (`TargetSdkVersion` >= `23`):
  * Your app will continue to use the old permissions model.
  * All permissions listed in the `AndroidManifest` will be asked for at install time.

### How to Ask For Permissions

Google recommends in this [video](https://youtu.be/5xVh-7ywKpE?t=9m15s) that there are four patterns to consider when thinking about permissions:

<img src="http://imgur.com/nZ2WX8W.png">

Each pattern dictates a different way of requesting permissions.  For instance, when requesting for critical but unclear permissions, use a warm welcome screen to help understand a permission is requested.  For critical permissions, such as a camera app that needs camera permission, ask up-front for it.  Secondary features 
can be requested later in context, such as a geotagging app when asking for a location permission.  For permissions that are secondary and unclear, you should include a rationale explanation if you really need them.

### Storage permissions

Rethink about whether you need read/write storage permissions (i.e. `android.permission.WRITE_EXTERNAL_STORAGE` or `android.permission.READ_EXTERNAL_STORAGE`), which give you all files on the SD card.  Instead, you should use methods on Context to access package-specific directories on external storage.  Your app always have access to read/write to these directories,so there is no need to permissions to request it:

```java
// Application-specific call that doesn't require external storage permissions
// Can be Environment.DIRECTORY_PICTURES, Environment.DIRECTORY_PODCASTS, Environment.DIRECTORY_RINGTONES, 
// Environment.DIRECTORY_NOTIFICATIONS, Environment.DIRECTORY_PICTURES, or Environment.MOVIES
File dir = MyActivity.this.getExternalFilesDir(Environment.DIRECTORY_PICTURES);
```

### Managing Permissions using ADB

Permissions can also be managed on the command-line using `adb` with the following commands. 

Show all Android permissions:

```bash
$ adb shell pm list permissions -d -g
```

Dumping app permission state:

```bash
$adb shell dumpsys package com.PackageName.enterprise
```

Granting and revoking runtime permissions:

```bash
$adb shell pm grant com.PackageName.enterprise some.permission.NAME
$adb shell pm revoke com.PackageName.enterprise android.permission.READ_CONTACTS
```

Installing an app with all permissions granted:

```bash
$adb install -g myAPP.apk
```

## Managing Runtime Permissions with PermissionsDispatcher

As of API 23 (Marshmallow), the permission model for Android [[has changed significantly|Understanding-App-Permissions]]. Now, rather than all being setup at install-time, certain [dangerous permissions](http://developer.android.com/guide/topics/security/permissions.html#normal-dangerous) must be checked and activated at runtime instead. 

For a full breakdown of how runtime permissions work, check out the [[understanding app permissions|Understanding-App-Permissions#permission-updates-in-marshmallow]] guide. This guide is focused on the practical approach for managing runtime permissions, requesting access to a feature, and managing error cases where the permission is denied.

The easiest way to manage runtime permissions is by using third-party libraries. In this guide, we will be taking a look at the [PermissionsDispatcher](https://github.com/hotchemi/PermissionsDispatcher) library. The library is 100% reflection-free and as such does not cause significant performance penalties. 

### Dangerous Permissions

First, we need to recognize the [dangerous permissions](http://developer.android.com/guide/topics/security/permissions.html#normal-dangerous) that require us to request runtime permissions. This includes but is not limited to the following common permissions:

| Name                                         | Description              |
| --------------------------------             | --------------------     |
| `Manifest.permission.READ_CALENDAR`          | Read calendar events     |
| `Manifest.permission.WRITE_CALENDAR`         | Write calendar events    |
| `Manifest.permission.CAMERA`                 | Access camera object     | 
| `Manifest.permission.READ_CONTACTS`          | Read phone contacts      |
| `Manifest.permission.WRITE_CONTACTS`         | Write phone contacts     |
| `Manifest.permission.ACCESS_FINE_LOCATION`   | Access precise location  |
| `Manifest.permission.ACCESS_COARSE_LOCATION` | Access general location  |
| `Manifest.permission.RECORD_AUDIO`           | Record with microphone   | 
| `Manifest.permission.CALL_PHONE`             | Call using the dialer    |
| `Manifest.permission.READ_EXTERNAL_STORAGE`  | Read external or SD      |
| `Manifest.permission.WRITE_EXTERNAL_STORAGE` | Write to external or SD  |

The full list of [dangerous permissions](http://developer.android.com/guide/topics/security/permissions.html#normal-dangerous) contains all permissions that require runtime management. Please note that permissions in the [normal permission group](http://developer.android.com/guide/topics/security/normal-permissions.html) **do not require run-time checks** as [[outlined here|Understanding-App-Permissions#normal-permissions]].

### Installation

Make sure to [[upgrade|Getting-Started-with-Gradle#upgrading-gradle]] to the latest Gradle version. 

And on your app module in `app/build.gradle`:

```gradle
dependencies {
  compile 'com.github.hotchemi:permissionsdispatcher:2.0.7'
  annotationProcessor 'com.github.hotchemi:permissionsdispatcher-processor:2.0.7'
}
```

Be sure to use the latest version: [![Download](https://api.bintray.com/packages/hotchemi/maven/permissionsdispatcher/images/download.svg)](https://bintray.com/hotchemi/maven/permissionsdispatcher/_latestVersion) available.

### Usage

Suppose we wanted to record be able to call a number using the phone's dialer. Since this is a dangerous permission, we need to ask the user for the permission at runtime with the `Manifest.permission.CALL_PHONE` permission. This requires us to do the following:

1. Annotate the activity or fragment with `@RuntimePermissions`
2. Annotate the method that requires the permission with `@NeedsPermission`
3. Delegate the permissions events to a compiled helper class
4. Invoke the helper class in order to trigger the action with permission request
5. **Optional:** Annotate a method which explains the reasoning with a dialog
6. **Optional:** Annotate a method which fires if the permission is denied
7. **Optional:** Annotate a method which fires if the permission will never be asked for again.

First, we need to annotate the activity or fragment with `@RuntimePermissions`:

```java
@RuntimePermissions
public class MainActivity extends AppCompatActivity {
  // ...
}
```

Next, we need to annotate the method that requires the permission with `@NeedsPermission` tag:

```java
@RuntimePermissions
public class MainActivity extends AppCompatActivity {
    @NeedsPermission(Manifest.permission.CALL_PHONE)
    void callPhone() {
        // Trigger the calling of a number here
    }
}
```

After compiling the project, we need to delegate the permission events to the generated helper class ([Activity Name] + PermissionsDispatcher):

```java
@RuntimePermissions
public class MainActivity extends AppCompatActivity {
    // ...
    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        // NOTE: delegate the permission handling to generated method
        MainActivityPermissionsDispatcher.onRequestPermissionsResult(this, requestCode, grantResults);
    }
}
```

In the code base, we can trigger the call with the appropriate runtime permission checks using the generated methods suffixed with `WithCheck` on the helper class:

```java
// NOTE: delegate the permission handling to generated method
MainActivityPermissionsDispatcher.callPhoneWithCheck(this);
```

This will invoke the `callPhone` method wrapped with the appropriate permission checks. 

### Permission Event Handling

We can also optionally configure the rationale dialog, handle the denial of a permission or manage when the user requests never to be asked again:

```java
@RuntimePermissions
public class MainActivity extends AppCompatActivity {

    // ...

    // Annotate a method which explains why the permission/s is/are needed. 
    // It passes in a `PermissionRequest` object which can continue or abort the current permission
    @OnShowRationale(Manifest.permission.CALL_PHONE)
    void showRationaleForPhoneCall(PermissionRequest request) {
        new AlertDialog.Builder(this)
            .setMessage(R.string.permission_phone_rationale)
            .setPositiveButton(R.string.button_allow, (dialog, button) -> request.proceed())
            .setNegativeButton(R.string.button_deny, (dialog, button) -> request.cancel())
            .show();
    }

    // Annotate a method which is invoked if the user doesn't grant the permissions
    @OnPermissionDenied(Manifest.permission.CALL_PHONE)
    void showDeniedForPhoneCall() {
        Toast.makeText(this, R.string.permission_call_denied, Toast.LENGTH_SHORT).show();
    }

    // Annotates a method which is invoked if the user 
    // chose to have the device "never ask again" about a permission
    @OnNeverAskAgain(Manifest.permission.CALL_PHONE)
    void showNeverAskForPhoneCall() {
        Toast.makeText(this, R.string.permission_call_neverask, Toast.LENGTH_SHORT).show();
    }
}
```

With that we can easily handle all of our runtime permission needs.

## Alternatives

In addition the the [PermissionsDispatcher](https://github.com/hotchemi/PermissionsDispatcher) outlined above, there are many other popular permissions libraries with various APIs and alternate designs including the following: 

* [Dexter](https://github.com/Karumi/Dexter)
* [easypermissions](https://github.com/googlesamples/easypermissions)
* [let](https://github.com/canelmas/let)
* [AndroidPermissions](https://github.com/ZeroBrain/AndroidPermissions)
* [Grant](https://github.com/anthonycr/Grant)
* [Assent](https://github.com/afollestad/assent)
* [Ask](https://github.com/00ec454/Ask)

A full list of permissions libraries [can be found here](https://gist.github.com/dlew/2a21b06ee8715e0f7338).


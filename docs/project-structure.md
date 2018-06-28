description: Android Project Structure

# Android Project Structure

![Android App Project Structure](/images/android-app-project-structure.png)

Within an Android app project structure, the most frequently edited folders are:

* `src` - Java source files associated with your project. This includes the `Activity` *controller* files as well as your models and helpers.
* `res` - Resource files associated with your project. All graphics, strings, layouts, and other resource files are stored in the resource file hierarchy under the res directory. 
* `res/layout` - XML layout files that describe the views and layouts for each activity and for partial views such as list items.
* `res/values` - XML files which store various attribute values. These include `strings.xml`, `dimens.xml`, `styles.xml`, `colors.xml`, `themes.xml`, and so on.
* `res/drawable` - Here we store the various density-independent graphic assets used in our application.
* `res/drawable-hdpi` - Series of folders for density specific images to use for various resolutions.

The most frequently edited files are:

* `res/layout/activity_foo.xml` - This file describes the layout of the activity's UI. This means the placement of every view object on one app screen.
* `src/.../FooActivity.java` - The Activity "controller" that constructs the activity using the view, and handles all event handling and view logic for one app screen.
* `AndroidManifest.xml` - This is the Android application definition file. It contains information about the Android application such as minimum Android version, permission to access Android device capabilities such as internet access permission, ability to use phone permission, etc.

Other less edited folders include:

* `gen` - Generated Java code files, this library is for Android internal use only.
* `assets` - Uncompiled source files associated with your project; Rarely used.
* `bin` - Resulting application package files associated with your project once it’s been built.
* `libs` - Contains any secondary libraries (jars) you might want to link to your app.

## Organizing Source Files

Android applications should always be neatly organized with a clear folder structure that makes your code easy to read. In addition, proper naming conventions for code and classes are important to ensure your code is clean and maintainable.

### Naming Conventions

Be sure to check out the [Ribot Code and Style Guidelines](https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md) for a more extensive breakdown of suggested style and naming guidelines.

#### For Java Code

The following naming and casing conventions are important for your Java code:

| Type        | Example                | Description                   | Link |
| ----------- | ------------           | ----------------------------- | ------
| Variable    | `incomeTaxRate`        | All variables should be camel case | [Read More](https://www.cwu.edu/~gellenbe/javastyle/variable.html) |
| Constant    | `DAYS_IN_WEEK`         | All constants should be all uppercase | [Read More](https://www.cwu.edu/~gellenbe/javastyle/constant.html) |
| Method      | `convertToEuroDollars` | All methods should be camel case | [Read More](https://www.cwu.edu/~gellenbe/javastyle/method.html) |
| Parameter   | `depositAmount`        | All parameter names should be camel case | [Read More](https://www.cwu.edu/~gellenbe/javastyle/parameter.html) |

See [this naming guide](https://www.cwu.edu/~gellenbe/javastyle/naming.html) for more details.

#### For Android Classes

Android classes should be named with a particular convention that makes their purpose clear in the name. For example all activities should end with `Activity` as in `MoviesActivity`. The following are the most important naming conventions:

| Name            | Convention                | Inherits            |
| ----------      | ------------------        | ------------        | 
| Activity        | `CreateTodoItemActivity`  | `AppCompatActivity`, `Activity` |
| List Adapter    | `TodoItemsAdapter`        | `BaseAdapter`, `ArrayAdapter`   |
| Database Helper | `TodoItemsDbHelper`       | `SQLiteOpenHelper`  |
| Network Client  | `TodoItemsClient`         | N/A                 |
| Fragment        | `TodoItemDetailFragment`  | `Fragment`          |
| Service         | `FetchTodoItemService`    | `Service`, `IntentService`  |

Use your best judgement for other types of files. The goal is for any Android-specific classes to be identifiable by the suffix. 

### Android Folder Structure

There are several best practices for organizing your app's package structure.

#### Organize packages by category

[The way to do this](http://blog.smartlogic.io/2013/07/09/organizing-your-android-development-code-structure) is to group things together by their category. Each component goes to the corresponding package:

* `com.example.myapp.activities` - Contains all activities
* `com.example.myapp.adapters` - Contains all custom adapters
* `com.example.myapp.models`   - Contains all our data models
* `com.example.myapp.network` - Contains all networking code
* `com.example.myapp.fragments` - Contains all fragments
* `com.example.myapp.utils` - Contains all helpers supporting code.
* `com.example.myapp.interfaces` - Contains all interfaces

Keeping these folders in each app means that code is logically organized and scanning the code is a pleasant experience. You can see a slight variation on this structure as suggested by [Futurice on their best-practices repo](https://github.com/futurice/android-best-practices#java-packages-architecture).

#### Organize packages by application features

Alternatively, we can package-by-feature rather than layers. This approach uses packages to [reflect the feature set](http://www.javapractices.com/topic/TopicAction.do?Id=205). Consider the following package structure [as outlined in this post](https://medium.com/@cesarmcferreira/package-by-features-not-layers-2d076df1964d#.f7znkie19):

* `com.example.myapp.service.*` - Is a subpackage for all background related service packages/classes
* `com.example.myapp.ui.*` - Is a subpackage for all UI-related packages/classes
* `com.example.myapp.ui.mainscreen` - Contains classes related to some app's Main Screen
* `com.example.myapp.ui.detailsscreen` - Contains classes related to some app's Item Details Screen

This feature allows you to place `DetailsActivity`, `DetailsFragment`, `DetailsListAdapter`, `DetailsItemModel` in one package, which provides comfortable navigation when you're working on "item details" feature.

`DetailsListAdapter` and `DetailsItemModel` classes and/or their properties can be made package-private, and thus not exposed outside of the package. Within the package you may access their properties directly without generating tons of boilerplate "setter" methods.

This can make object creation really simple and intuitive, while objects remain immutable outside the package.

## Organizing Resources

Resources should be split up into the following key files and folders:

| Name         | Path                      | Description |
| --------     | -----------               | ----------- |
| XML Layouts  | `res/layout/`             | This is where we put our XML layout files.     |
| XML Menus    | `res/menu/`               | This is where we put our AppBar menu actions.  |
| Drawables    | `res/drawable`            | This is where we put images and XML drawables. | 
| Colors       | `res/values/colors.xml`   | This is where we put [color definitions](http://developer.android.com/guide/topics/resources/more-resources.html#Color). |
| Dimensions   | `res/values/dimens.xml`   | This is where we put [dimension values](http://developer.android.com/guide/topics/resources/more-resources.html#Dimension). | 
| Strings      | `res/values/strings.xml`  | This is where we put strings.           |
| Styles       | `res/values/styles.xml`   | This is where we put style values.      |

See the [full list of resources here](http://developer.android.com/guide/topics/resources/providing-resources.html#ResourceTypes) and note the following:

 * **Don't hardcode color hex values in the layout**. Instead of hardcoding these values, be sure to [move all colors](http://developer.android.com/guide/topics/resources/more-resources.html#Color) into `res/values/colors.xml` and reference the colors in layouts with `@color/royal_blue`.
 * **Don't hardcode margin / padding dimensions in the layout**. Instead of hardcoding these values, be sure to [move all dimension values](http://developer.android.com/guide/topics/resources/more-resources.html#Dimension) into `res/values/dimens.xml` and reference these in layouts with `@dimen/item_padding_left`.
 * To support multiple devices, we can then use the [alternative resources system](http://guides.codepath.com/android/Understanding-App-Resources#providing-alternate-resources) to provide different colors, strings, dimens, styles, etc based on the device type, screen size, API version and much more. 

Be sure to start properly organizing your resources early on in the development of an application. Be sure to check out the [Ribot Code and Style Guidelines](https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md) for a more extensive breakdown of suggested style and naming guidelines.

### Organizing Resources into Subfolders

Often there are questions about organizing not just the source files but also better organizing the [application resources](http://guides.codepath.com/android/Understanding-App-Resources). In a modern app, there are often hundreds of different layout files, drawables, styles, etc and by default these are all grouped together in a flat list within a single subdirectory (i.e `res/layout`).

Refer to [stackoverflow post](http://stackoverflow.com/q/4930398/313399) for a discussion of the other options. 

### Conclusion

It is up to you to decide which of the aforementioned approaches suits your project best.

However, in general Java programming, packaging apps by feature is considered preferable and [makes a lot of sense](http://www.javapractices.com/topic/TopicAction.do?Id=205).

## Retaining Fragments

In many cases, we can avoid problems when an Activity is re-created by simply using fragments. If your views and state are within a fragment, we can easily have the fragment be retained when the activity is re-created:

```java
public class RetainedFragment extends Fragment {
    // data object we want to retain
    private MyDataObject data;

    // this method is only called once for this fragment
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // retain this fragment when activity is re-initialized
        setRetainInstance(true);
    }

    public void setData(MyDataObject data) {
        this.data = data;
    }

    public MyDataObject getData() {
        return data;
    }
}
```

This approach keeps the fragment from being destroyed during the activity lifecycle.  They are instead retained inside the Fragment Manager.  See the Android official docs for more [information](http://developer.android.com/guide/topics/resources/runtime-changes.html#RetainingAnObject).  

Now you can **check to see if the fragment already exists by tag** before creating one and the fragment will retain it's state across configuration changes. See the [Handling Runtime Changes](http://developer.android.com/guide/topics/resources/runtime-changes.html#RetainingAnObject) guide for more details.

## Understanding the Android Application Class

The `Application` class in Android is the base class within an Android app that contains all other components such as activities and services. The Application class, or any subclass of the Application class, is instantiated before any other class when the process for your application/package is created.

<img src="http://i.imgur.com/b4YiAfy.png" />

This class is primarily used for initialization of global state before the first `Activity` is displayed. Note that custom `Application` objects should be used carefully and are often **not needed at all**. 

### Custom Application Classes

In many apps, there's no need to work with an application class directly. However, there are a few acceptable uses of a custom application class:

 * Specialized tasks that need to run before the creation of your first activity
 * Global initialization that needs to be shared across all components (crash reporting, persistence)
 * Static methods for easy access to static immutable data such as a shared network client object

Note that you should **never store mutable shared data** inside the `Application` object since that data might disappear or become invalid at any time. Instead, store any mutable shared data using [[persistence strategies|Persisting-Data-to-the-Device]] such as files, `SharedPreferences` or `SQLite`. 

### Defining Your Application Class

If we do want a custom application class, we start by creating a new class which extends `android.app.Application` as follows:

```java
import android.app.Application;

public class MyCustomApplication extends Application {
        // Called when the application is starting, before any other application objects have been created.
        // Overriding this method is totally optional!
	@Override
	public void onCreate() {
	    super.onCreate();
            // Required initialization logic here!
	}

        // Called by the system when the device configuration changes while your component is running.
        // Overriding this method is totally optional!
	@Override
	public void onConfigurationChanged(Configuration newConfig) {
	    super.onConfigurationChanged(newConfig);
	}

        // This is called when the overall system is running low on memory,
        // and would like actively running processes to tighten their belts.
        // Overriding this method is totally optional!
	@Override
	public void onLowMemory() {
	    super.onLowMemory();
	}
}
```

And specify the `android:name` property in the the `<application>` node in `AndroidManifest.xml`:

```xml
<application
   android:name=".MyCustomApplication"
   android:icon="@drawable/icon"
   android:label="@string/app_name"
   ...>
```

That's all you should need to get started with your custom application.

## Limitations and Warnings

There is always data and information that is needed in many places within your app. This might be a session token, the result of an expensive computation, etc. It might be tempting to use the application instance in order to avoid the overhead of passing objects between activities or keeping those in persistent storage.

However, you should **never store mutable instance data** inside the `Application` object because if you assume that your data will stay there, your application will inevitably crash at some point with a `NullPointerException`. The application object is **not guaranteed to stay in memory forever, it will get killed**. Contrary to popular belief, the app won’t be restarted from scratch. Android will create a new `Application` object and start the activity where the user was before to give the illusion that the application was never killed in the first place.

So how should we store shared application data? We should store shared data in one of the following ways:

 * Explicitly pass the data to the Activity through the intent.
 * Use one of the many ways to persist the data to disk.

**Bottom Line**: Storing data in the `Application` object is error-prone and can crash your app. Prefer storing your global data on disk if it is really needed later or explicitly pass to your activity in the intent’s extras.

## Resources

* <https://developer.android.com/reference/android/app/Application.html>
* <http://stackoverflow.com/questions/18002227/why-extend-an-application-class>
* <http://www.devahead.com/blog/2011/06/extending-the-android-application-class-and-dealing-with-singleton/>
* <https://tausiq.wordpress.com/2013/01/27/android-make-use-of-android-application-class-as-singleton-object/>
* <https://androidresearch.wordpress.com/2012/03/22/defining-global-variables-in-android/>
* <http://www.helloandroid.com/tutorials/maintaining-global-application-state>
* <https://www.mobomo.com/2011/5/how-to-use-application-object-of-android/>
* <http://www.developerphil.com/dont-store-data-in-the-application-object/>
* <http://www.vogella.com/tutorials/AndroidLifeCycle/article.html#managing-the-application-life-cycle>
* <https://github.com/hotchemi/PermissionsDispatcher>
* <http://developer.android.com/guide/topics/security/permissions.html>
* <http://developer.android.com/preview/features/runtime-permissions.html>
* <https://github.com/googlesamples/android-RuntimePermissions>
* <http://www.androidauthority.com/goodbye-menu-button-hello-action-bar-48312/>
* <http://android-developers.blogspot.in/2013/08/actionbarcompat-and-io-2013-app-source.html>
* <http://android-developers.blogspot.com/2014/10/appcompat-v21-material-design-for-pre.html>
* <https://chris.banes.me/2015/04/22/support-libraries-v22-1-0/>
* <http://developer.android.com/guide/topics/resources/runtime-changes.html>
* <http://developer.android.com/training/basics/activity-lifecycle/recreating.html>
* <http://www.vogella.com/tutorials/AndroidLifeCycle/article.html#configurationchange>
* <http://www.androiddesignpatterns.com/2013/04/retaining-objects-across-config-changes.html>
* <http://www.intertech.com/Blog/saving-and-retrieving-android-instance-state-part-1/>
* <http://sunil-android.blogspot.com/2013/03/save-and-restore-instance-state.html>
* <https://medium.com/google-developers/activity-revival-and-the-case-of-the-rotating-device-167e34f9a30d#.nq3b23lxg>
* http://developer.android.com/training/basics/activity-lifecycle/index.html
* https://github.com/xxv/android-lifecycle
* <http://blog.smartlogic.io/2013/07/09/organizing-your-android-development-code-structure>
* <http://stackoverflow.com/questions/5525872/android-project-package-structure>
* <http://www.javapractices.com/topic/TopicAction.do?Id=205>
* <http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure>
* <http://developer.android.com/guide/topics/resources/string-resource.html>
* <http://developer.android.com/guide/topics/resources/accessing-resources.html>
* <http://mobile.tutsplus.com/tutorials/android/android-string/>
* <http://developer.android.com/guide/topics/resources/providing-resources.html>
* <http://developer.android.com/training/multiscreen/screendensities.html>
* <http://www.evoketechnologies.com/blog/effective-ui-design-tips-android-devices/>
* <http://www.androiddesignpatterns.com/2016/08/contextcompat-getcolor-getdrawable.html/>
* <http://developer.android.com/tools/projects/index.html#ApplicationProjects>
* <http://www.codeproject.com/Articles/395614/Basic-structure-of-an-Android-project>
* <http://mobile.tutsplus.com/tutorials/android/android-sdk-app-structure/>
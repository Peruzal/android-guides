description: Android Resources

# Android Resources

## Understanding App Resources

In Android, almost everything is a **resource**. [Defining resources](http://developer.android.com/guide/topics/resources/providing-resources.html) that you can then [access in your app](http://developer.android.com/guide/topics/resources/accessing-resources.html) is an essential part of Android development.

Resources are used for anything from defining colors, images, layouts, menus, and string values. The value of this is that nothing is hardcoded. Everything is defined in these resource files and then can be referenced within your application's code. The simplest of these resources and the most common is using **string resources** to allow for flexible, localized text. 

### Types of Resources

The following are the most common types of resources within Android apps:

| Name                 | Folder   | Description                                     |
| ----                 | ------   | -----------                                     |
| Property Animations  | animator | XML files that define property animations.      |
| Tween Animations     | anim     | XML files that define tween animations.         |
| Drawables            | drawable | Bitmap files or XML files that act as graphics  | 
| Layout               | layout   | XML files that define a user interface layout   |
| Menu                 | menu     | XML files that define menus or action bar items |
| Values               | values   | XML files with values such as strings, integers, and colors. |

In addition, note the following key files stored within the `values` folder mentioned above:

| Name         | File                      | Description                    |
| ----                 | ------   | -----------                                     |
| Colors       | `res/values/colors.xml`   | For [color definitions](http://developer.android.com/guide/topics/resources/more-resources.html#Color) such as text color |
| Dimensions   | `res/values/dimens.xml`   | For [dimension values](http://developer.android.com/guide/topics/resources/more-resources.html#Dimension) such as padding | 
| Strings      | `res/values/strings.xml`  | For [string values](http://developer.android.com/guide/topics/resources/string-resource.html) such as the text for a title     |
| Styles       | `res/values/styles.xml`   | For [style values](http://developer.android.com/guide/topics/resources/style-resource.html) such as color of the AppBar      |

For the full list of resource types, check out the [Providing a Resource](http://developer.android.com/guide/topics/resources/providing-resources.html) guide.

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

## Providing App Resources

### Defining a String Resource

For every piece of text you want to display within your application (i.e the label of a button, or the text inside a TextView), you should first define the text in the `res/values/strings.xml` file. Each entry is a key (representing the id of the text) and a value (the text itself). For example, if I want a button to display "Submit", I might add the following string resource to `res/values/strings.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello!</string>
    <string name="submit_label">Submit</string>
</resources>
```

Now if I ever reference the string resource for `submit_label`, the default will be for "Submit" to be displayed. Later though, you could create **qualified resource files** that change this value for different countries or between devices. We can also store more complex strings (with html or special characters) by using CDATA to escape the string such as:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="feedback_label">
  <![CDATA[
    Please <a href="http://highlight.com">let us know</a> if you have feedback on this or if 
    you would like to log in with another identity service. Thanks! This is a longer string!  
  ]]>
  </string>
</resources>
```

For more details on defining string resources, [check this guide](http://developer.android.com/guide/topics/resources/string-resource.html). You can also refer to [this guide for style resources](http://developer.android.com/guide/topics/resources/style-resource.html) and [this guide for other types](http://developer.android.com/guide/topics/resources/more-resources.html).

### Referencing an App Resource

Now that we have defined our string resource, we can access that resource in either our Java code or our XML layouts at any time. To access, the resource in the XML Layout file, simply use the `@` syntax used to access any resource:

```xml
<Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@string/submit_label" />
```

To access the resource directly in your Java code, simply use the `getResources.getString` or `getString` methods to access the value given the resource id:

```java
String submitText = getResources().getString(R.string.submit_label)
```

And the string value will be retrieved. This similar pattern works for almost any resource from images (drawables) to colors. The `getResources()` method returns a [Resources](http://developer.android.com/reference/android/content/res/Resources.html) object with many resource fetching methods. Each resource is defined within different folders and files within the `res` directory depending on their type.

### Defining Color Resources

In addition to string resources shown above, the following common resource types can be found below. First, let's take a look at the colors file which is used to define all colors used within an application. Colors should be defined within `res/values/colors.xml` and the XML file looks like the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
   <color name="white">#FFFFFF</color>
   <color name="yellow">#FFFF00</color>
   <color name="fuchsia">#FF00FF</color>
</resources>
```

The colors can be accessed in Java code with:

```java
// getResources().getColor() is now deprecated
// Resources res = getResources();
// int color = res.getColor(R.color.yellow); 

// Use ContextCompatResources instead of getColor()
int color = ContextCompat.getColor(context, R.color.yellow);
```

It is important to note that the most current way of accessing color resources (since API 24) requires providing context in order to resolve any custom [[theme|Styles and Themes]] attributes.  See [this article](http://www.androiddesignpatterns.com/2016/08/contextcompat-getcolor-getdrawable.html) for more context.

and referenced within any view in the XML using:

```xml
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:textColor="@color/fuchsia"
    android:text="Hello"/>
```

That's all you need for color resources. Be sure to keep hardcoded colors out of your layout files. 

### Defining Dimension Resources

Next, let's take a look at the dimensions file which is used to define all size dimensions used within an app. A dimension is specified with a number followed by a unit of measure. For example: `10px`, `5sp`.  Dimensions should be defined within `res/values/dimens.xml` and the XML file looks like the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="textview_height">25dp</dimen>
    <dimen name="textview_width">150dp</dimen>
    <dimen name="ball_radius">30dp</dimen>
    <dimen name="font_size">16sp</dimen>
</resources>
```

The dimensions can be accessed in Java code with:

```java
Resources res = getResources();
float fontSize = res.getDimension(R.dimen.font_size);
```

and referenced within any view in the XML layouts using:

```xml
<TextView
    android:layout_height="@dimen/textview_height"
    android:layout_width="@dimen/textview_width"
    android:textSize="@dimen/font_size"/>
```

That's all you need for dimension resources. Be sure to use this to keep hardcoded font sizes, padding and margin values out of your layout files. There are [many other resource types to explore](http://developer.android.com/guide/topics/resources/more-resources.html#Color). 

### Dynamic Resource Retrieval

In certain cases, you might want to dynamically retrieve resources using just the key name rather than by "hardcoding" the resource id. For example, suppose I wanted to retrieve the "submit_label" string based on just that key name alone. This can be achieved using the `getIdentifier` method within an Activity:

```java
public String getStringValue(String key) {
    // Retrieve the resource id
    String packageName = getBaseContext().getPackageName();
    Resources resources = getBaseContext().getResources();
    int stringId = resources.getIdentifier(key, "string", packageName);
    if (stringId == 0) { return null; }
    // Return the string value based on the res id
    return resources.getString(stringId);
}
```

Now you can reference your string resources dynamically using:

```java
public String myKey = "submit_label"; // Maps to R.string.submit_label
public String myStringValue = getStringValue(myKey); // Returns string text
```

This can be done similarly for other types of resources as well. For example, for dynamically retrieving a view by a string ID:

```java
// getViewById("tvTest");
public View getViewById(String id) {
    // Retrieve the resource id
    String packageName = getBaseContext().getPackageName();
    Resources resources = getBaseContext().getResources();
    int viewId = resources.getIdentifier(id, "id", packageName);
    if (viewId == 0) { return null; }
    // Return the string value based on the res id
    return findViewById(viewId);
}
```

Check out the [getResources](http://developer.android.com/reference/android/content/res/Resources.html) object and <a href="http://developer.android.com/reference/android/content/res/Resources.html#getIdentifier\(java.lang.String, java.lang.String, java.lang.String\)">getIdentifier</a> for more details.


## More Resource Types

### Bool

A boolean value defined in XML.

> Note: A bool is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine bool resources with other simple resources in the one XML file, under one `<resources>` element.

### File Location

The file should be in `res/values/filename.xml`. The filename is arbitrary. The <bool> element's name will be used as the resource ID.

### Resource Reference

In Java: `R.bool.bool_name`

In XML: `@[package:]bool/bool_name`

### Syntax

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <bool
        name="bool_name"
        >[true | false]</bool>
</resources>
```

### Example

XML file saved at `res/values-small/bools.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <bool name="screen_small">true</bool>
    <bool name="adjust_view_bounds">true</bool>
</resources>
```

This application code retrieves the boolean:

```java
Resources res = getResources();
boolean screenIsSmall = res.getBoolean(R.bool.screen_small);
```

This layout XML uses the boolean for an attribute:

```xml
<ImageView
    android:layout_height="fill_parent"
    android:layout_width="fill_parent"
    android:src="@drawable/logo"
    android:adjustViewBounds="@bool/adjust_view_bounds" />
```

### Color

A color value defined in XML. The color is specified with an RGB value and alpha channel. You can use a color resource any place that accepts a hexadecimal color value. You can also use a color resource when a drawable resource is expected in XML (for example, `android:drawable="@color/green"`).

The value always begins with a pound (#) character and then followed by the Alpha-Red-Green-Blue information in one of the following formats:

* `#RGB`
* `#ARGB`
* `#RRGGBB`
* `#AARRGGBB`

!!! note 
    A color is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine color resources with other simple resources in the one XML file, under one `<resources>` element.

File Location - `res/values/colors.xml`. The filename is arbitrary. The `<color>` element's name will be used as the resource ID.

### Resource Reference

In Java: `R.color.color_name`
In XML: `@[package:]color/color_name`

### Syntax

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color
        name="color_name"
        >hex_color</color>
</resources>
```

### Example

XML file saved at `res/values/colors.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
   <color name="opaque_red">#f00</color>
   <color name="translucent_red">#80ff0000</color>
</resources>
```

This application code retrieves the color resource:

```java
Resources res = getResources();
int color = res.getColor(R.color.opaque_red);
```

This layout XML applies the color to an attribute:

```xml
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:textColor="@color/translucent_red"
    android:text="Hello"/>
```

### Dimension

A dimension value defined in XML. A dimension is specified with a number followed by a unit of measure. For example: 10px, 2in, 5sp. The following units of measure are supported by Android:

- **dp - Density-independent Pixels** - An abstract unit that is based on the physical density of the screen. These units are relative to a 160 dpi (dots per inch) screen, on which 1dp is roughly equal to 1px. When running on a higher density screen, the number of pixels used to draw 1dp is scaled up by a factor appropriate for the screen's dpi. Likewise, when on a lower density screen, the number of pixels used for 1dp is scaled down. The ratio of dp-to-pixel will change with the screen density, but not necessarily in direct proportion. Using dp units (instead of px units) is a simple solution to making the view dimensions in your layout resize properly for different screen densities. In other words, it provides consistency for the real-world sizes of your UI elements across different devices.

- **sp - Scale-independent Pixels** - This is like the dp unit, but it is also scaled by the user's font size preference. It is recommend you use this unit when specifying font sizes, so they will be adjusted for both the screen density and the user's preference.

- **pt - Points** - 1/72 of an inch based on the physical size of the screen, assuming a 72dpi density screen.

- **px - Pixels** - Corresponds to actual pixels on the screen. This unit of measure is not recommended because the actual representation can vary across devices; each devices may have a different number of pixels per inch and may have more or fewer total pixels available on the screen.

- **mm - Millimeters** - Based on the physical size of the screen.

- **in - Inches** - Based on the physical size of the screen.


> Note: A dimension is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine dimension resources with other simple resources in the one XML file, under one `<resources>` element.

**File Location** - `res/values/filename.xml`. The filename is arbitrary. The <dimen> element's name will be used as the resource ID.

### Resource Reference

In Java: `R.dimen.dimension_name`

In XML: `@[package:]dimen/dimension_name`

### Syntax

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen
        name="dimension_name"
        >dimension</dimen>
</resources>
```

### Example

XML file saved at res/values/dimens.xml:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="textview_height">25dp</dimen>
    <dimen name="textview_width">150dp</dimen>
    <dimen name="ball_radius">30dp</dimen>
    <dimen name="font_size">16sp</dimen>
</resources>
```

This application code retrieves a dimension:

```java
Resources res = getResources();
float fontSize = res.getDimension(R.dimen.font_size);
```

This layout XML applies dimensions to attributes:

```xml
<TextView
    android:layout_height="@dimen/textview_height"
    android:layout_width="@dimen/textview_width"
    android:textSize="@dimen/font_size"/>
```


### ID

A unique resource ID defined in XML. Using the name you provide in the `<item>` element, the Android developer tools create a unique integer in your project's R.java class, which you can use as an identifier for an application resources (for example, a View in your UI layout) or a unique integer for use in your application code (for example, as an ID for a dialog or a result code).

> Note: An ID is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine ID resources with other simple resources in the one XML file, under one `<resources>` element. Also, remember that an ID resources does not reference an actual resource item; it is simply a unique ID that you can attach to other resources or use as a unique integer in your application.

**File location** - `res/values/filename.xml`. The filename is arbitrary.

**Resource reference**

In Java: `R.id.name`

In XML: `@[package:]id/name`

**Syntax**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item
        type="id"
        name="id_name" />
</resources>
```

### Example

XML file saved at `res/values/ids.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item type="id" name="button_ok" />
    <item type="id" name="dialog_exit" />
</resources>
```

Then, this layout snippet uses the "button_ok" ID for a Button widget:

```xml
<Button android:id="@id/button_ok"
    style="@style/button_style" />
```

Notice that the `android:id` value does not include the plus sign in the ID reference, because the ID already exists, as defined in the ids.xml example above. (When you specify an ID to an XML resource using the plus sign—in the format `android:id="@+id/name"` — it means that the "name" ID does not exist and should be created.)

As another example, the following code snippet uses the "dialog_exit" ID as a unique identifier for a dialog:

```java
showDialog(R.id.dialog_exit);
```

In the same application, the "dialog_exit" ID is compared when creating a dialog:

```java
protected Dialog onCreateDialog(int id) {
    Dialog dialog;
    switch(id) {
    case R.id.dialog_exit:
        ...
        break;
    default:
        dialog = null;
    }
    return dialog;
}
```

### Integer

An integer defined in XML.

> Note: An integer is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine integer resources with other simple resources in the one XML file, under one `<resources>` element.

**File location** - `res/values/filename.xml` The filename is arbitrary. The `<integer>` element's name will be used as the resource ID.

**Resource Reference**

In Java: `R.integer.integer_name`

In XML: `@[package:]integer/integer_name`

**Syntax**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <integer
        name="integer_name"
        >integer</integer>
</resources>
```


**Example**

XML file saved at `res/values/integers.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <integer name="max_speed">75</integer>
    <integer name="min_speed">5</integer>
</resources>
```

This application code retrieves an integer:

```java
Resources res = getResources();
int maxSpeed = res.getInteger(R.integer.max_speed);
```


### Integer Array

An array of integers defined in XML.

> Note: An integer array is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine integer array resources with other simple resources in the one XML file, under one `<resources>` element.

File location - `res/values/filename.xml`. The filename is arbitrary. The `<integer-array>` element's name will be used as the resource ID.

**Compiled resource data type** -  Resource pointer to an array of integers.

**Resource reference**

In Java: `R.array.integer_array_name`

In XML: `@[package:]array.integer_array_name`

**Syntax**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <integer-array
        name="integer_array_name">
        <item
            >integer</item>
    </integer-array>
</resources>
```

**Example**

XML file saved at `res/values/integers.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <integer-array name="bits">
        <item>4</item>
        <item>8</item>
        <item>16</item>
        <item>32</item>
    </integer-array>
</resources>
```

This application code retrieves the integer array:

```java
Resources res = getResources();
int[] bits = res.getIntArray(R.array.bits);
```

### Typed Array

A [TypedArray](https://developer.android.com/reference/android/content/res/TypedArray.html) defined in XML. You can use this to create an array of other resources, such as drawables. Note that the array is not required to be homogeneous, so you can create an array of mixed resource types, but you must be aware of what and where the data types are in the array so that you can properly obtain each item with the TypedArray's get...() methods.

> Note: A typed array is a simple resource that is referenced using the value provided in the name attribute (not the name of the XML file). As such, you can combine typed array resources with other simple resources in the one XML file, under one `<resources>` element.

**File location **- `res/values/filename.xml`. The filename is arbitrary. The `<array>` element's name will be used as the resource ID.

**Compiled resource data type** - Resource pointer to a `TypedArray`.

**Resource reference**

In Java: `R.array.array_name`

In XML: `@[package:]array.array_name`

**Syntax**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array
        name="integer_array_name">
        <item>resource</item>
    </array>
</resources>
```

**Example**

XML file saved at `res/values/arrays.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="icons">
        <item>@drawable/home</item>
        <item>@drawable/settings</item>
        <item>@drawable/logout</item>
    </array>
    <array name="colors">
        <item>#FFFF0000</item>
        <item>#FF00FF00</item>
        <item>#FF0000FF</item>
    </array>
</resources>
```

This application code retrieves each array and then obtains the first entry in each array:

```java
Resources res = getResources();
TypedArray icons = res.obtainTypedArray(R.array.icons);
Drawable drawable = icons.getDrawable(0);

TypedArray colors = res.obtainTypedArray(R.array.colors);
int color = colors.getColor(0,0);
```

## Providing Alternate Resources

### Responsive Design

In order to create an outstanding UI design, it is essential for app developers to create apps that work properly across a wide variety of devices. To do this, we first divide android mobile devices into various categories (buckets) based on screen size and display as shown below:

<img src="http://i.imgur.com/bLOO9e3.png" width="600" />

Apps must be designed to work across any different screen densities as well as screen sizes. This can be done leveraging a variety of systems provided by the Android framework.

### Introducing Alternate Resources

One of the most powerful tools available to developers is the option to provide "alternate resources" based on specific qualifiers such as phone size, language, density, and others. Common uses for alternate resources include:

 * Alternative layout files for different form factors (i.e phone vs tablet)
 * Alternative string resources for different languages (i.e English vs Italian)
 * Alternative drawable resources for different screen densities  (shown below)
 * Alternate style resources for different platform versions (Holo vs Material)
 * Alternate layout files for different screen orientations (i.e portrait vs landscape)

To specify configuration-specific alternatives for a set of resources, we create a new directory in `res` in the form of `[resource]-[qualifiers]`. For example, one best practice is to ensure that all images are provided for [[multiple screen densities|Working-with-the-ImageView#supporting-multiple-densities]]. 

![Densities](http://developer.android.com/images/screens_support/screens-densities.png)

This is achieved by having  `res/drawable-hdpi`, `res/drawable-xhdpi`, and `res/drawable-xxhdpi` folders with different versions of the same image. The correct resource is then selected automatically by the system based on the device density. The directory listing might look like the following:

```
res/
    drawable/   
        icon.png
        background.png    
    drawable-hdpi/  
        icon.png
        background.png
```

Note the resource files need to all have the same name within the different folders. This system works for **any type of resource** with a wide variety of qualifiers.

### Understanding Qualifiers

Android supports several configuration qualifiers and you can add multiple qualifiers to one directory name, by separating each qualifier with a dash. The most common qualifiers are listed below:

| Configuration       | Examples              | Description                                           |
| ------------        | ------                | ----------------------------------------------------- |
| Language            | `en`, `fr`            | Language code selected on the device                  |
| Screen size         | `sw480dp`,`sw600dp`   | Minimum width of the screen's height or width.        |
| Screen orientation  | `port`, `land`        | Screen is in portrait or landscape mode.              |
| Screen density      | `hdpi`, `xhdpi`       | Screen density often used for alternate images.       |
| Platform version    | `v7`, `v11`, `v21`    | Platform version often used for styles.               |

You can specify multiple qualifiers for a single set of resources, separated by dashes. For example, `drawable-en-sw600dp-land` applies to English tablets in landscape orientation. Note that if you use multiple qualifiers for a resource directory, you must add them to the directory name **in the order they are listed** in the table above. See the official docs for the [complete set of qualifiers available](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).

### Creating Alternate Resources

In Android Studio, the easiest way to create alternate resources is to **right-click on a resource subfolder** (i.e `layout`) in the Android project sidebar and then use the `New => Layout resource file` wizard to specify the qualifiers you'd like (i.e `orientation`):

<img src="http://i.imgur.com/cxeH3J1.gif" width="650" />

This will create **two versions of the layout file**, one for "portrait" mode (vertical) and one for "landscape" (horizontal). If you were to [add a different label into the second version of the layout](http://i.imgur.com/5kMibe3.gif) then you would see this effect **automatically** when the screen is rotated:

<img src="http://i.imgur.com/bRUzYLI.gif" width="350" />

To summarize, you can create as many versions of a resource file as is needed for different situations and then the most appropriate version of the resource file is selected automatically by the system.

### Determining Configuration at Runtime

When the app is running, we can always check the current configuration (orientation, screen size, etc) by accessing the [Configuration](https://developer.android.com/reference/android/content/res/Configuration.html) object through `getResources().getConfiguration()` within an activity or context object. For example, to determine the orientation (portrait or landscape) inside an activity, we can do:

```java
String image;
int orientation = getResources().getConfiguration().orientation;
if (orientation == Configuration.ORIENTATION_PORTRAIT) {
   image = "image_portrait.png";
   // ...
} else if (orientation == Configuration.ORIENTATION_LANDSCAPE) {
   image = "image_landscape.png";
   // ...
}
```

We can similarly access this within any object by getting access to a [[Context object|Using-Context]]. For example, within an `ArrayAdapter` by using `getContext().getResources().getConfiguration()` to access the configurations.

### Alternate Layout Files

Often alternative resources are used to specify different layout files for phones and tablets. This can be done using the "smallest width" qualifier of `sw`. The folder structure might be set up as follows:

```
res/
    layout/   
        activity_main.xml
        item_photo.xml    
    layout-sw600dp/ 
        activity_main.xml
    layout-sw600dp-land/
        activity_main.xml 
    layout-sw720dp/ 
        activity_main.xml
        item_photo.xml
    layout-land/
        activity_main.xml
        item_photo.xml
```

Generally speaking phones and phablets are between `sw240` and `sw480`. 7" tablets are `sw600` and 10" tablets are `sw720`. You can also simply add qualifiers such as `layout-land` to be applied for all devices in landscape mode.  Here’s an illustrated example of the above procedure:

<img src="http://i.imgur.com/KS5xs0n.png" width="600" />

For a detailed tutorial on how to manage responsive layouts for tablets, review our [[Flexible User Interfaces]] guide. You can also review this [article on UI design best practices](http://www.evoketechnologies.com/blog/effective-ui-design-tips-android-devices/) and [this official doc on resources](http://developer.android.com/guide/topics/resources/providing-resources.html) for more details.

There's also a list of Android phone screens and dimensions each one use that you can find [here](https://material.io/devices/)

### Layout Best Practices

Here is a quick checklist about how you can ensure that your application displays properly on different screens:

* Avoid using hard coded pixel values in your application code
* Use `RelativeLayout` properly and never use `AbsoluteLayout` 
* Use `wrap_content`, `match_parent`, or `dp` units when specifying dimensions
* Use alternate layouts and drawables to ensure a responsive design when needed

Review the rest of the [best practices for screen independence](http://developer.android.com/guide/practices/screens_support.html#screen-independence) on the official guide.

### Alias Resources

When you have a resource that you'd like to use for more than one device configuration, you do not need to put the same resource in more than one alternative resource directory. Instead, you can [create an alternative resource](http://developer.android.com/guide/topics/resources/providing-resources.html#AliasResources) that acts as an alias for a resource saved in your default resource directory.

### Best Resource Match

When you request a resource for which you provide alternatives, Android selects which alternative resource to use at runtime, depending on the current device configuration. Read the [official resource guide](http://developer.android.com/guide/topics/resources/providing-resources.html#BestMatch) for a detailed overview of how the match is selected.

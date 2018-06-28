description: Navigation between views is an important part of any application. There are several ways to setup navigation in an Android application, swipe views, navigation drawer, toolbar, and bottom navigation bar.
## Common Navigation Paradigms

Navigation between views is an important part of any application. There are several ways to setup navigation in an Android application:

 * [Swipe Views](http://developer.android.com/training/implementing-navigation/lateral.html) - Allow paging between views using a swipe gesture
 * [Navigation Drawer](http://developer.android.com/training/implementing-navigation/nav-drawer.html) - Displays a vertical menu that slides in to allow navigation between views 
 * [ActionBar](http://developer.android.com/design/patterns/actionbar.html) - Using the ActionBar and/or ActionBar tabs to switch between different views
 * [Screen Map](http://developer.android.com/training/design-navigation/descendant-lateral.html#buttons) - Providing a series of buttons on screen that can be pressed to visit different views

These four represent the most common navigation paradigms in Android applications. The specifics for how to implement these can be found in the various links above.

### Swiping Views

We can use a `ViewPager` with Fragments to display tabs on top of a swiping view.

### Navigation Drawer

To create a basic navigation drawer that toggles between displaying different fragments. For more details about creating a custom drawer check out the [Creating a Navigation Drawer](http://developer.android.com/training/implementing-navigation/nav-drawer.html#top) docs.  

### ActionBar Tabs

Check out the ActionBar Tabs with Fragments#fragments-and-tabsguide for more details of how to add ActionBar tabs to your Activity. Note that **ActionBar Tabs is deprecated** since Android API 21.


## Tabbed Navigation

Tabs are now best implemented by leveraging the ViewPager with a custom "tab indicator" on top. In this guide, we will be using Google's new [TabLayout](https://developer.android.com/reference/android/support/design/widget/TabLayout.html) included in the support design library release for Android "M".

Prior to Android "M", the easiest way to setup tabs with Fragments was to use ActionBar Tabs as described in [[ActionBar Tabs with Fragments|ActionBar-Tabs-with-Fragments]] guide. However, all methods related to navigation modes in the ActionBar class (such as `setNavigationMode()`, `addTab()`, `selectTab()`, etc.) are now deprecated.

### Design Support Library

To implement Google Play style sliding tabs, make sure to follow the Design Support Library first.

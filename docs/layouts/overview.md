description: Views and Layouts

# Layouts Overview

View Layouts are a type of View class whose primary purpose is to organize and position other view controls. These layout classes (LinearLayout, RelativeLayout, etc.) are used to display child controls, such as text controls or buttons on the screen. 

Android activities (screens) use layouts as a container for view controls, and layouts can actually contain other nested layouts as well. Nearly all Android activities have layout containers similar to the way that most HTML documents use "divs" to contain other content.

There are a few very commonly used layouts and then many more specialized layouts that are used in only very particular cases. The bread and butter layouts are **LinearLayout**, **RelativeLayout**, and **FrameLayout**.

It's important to note the class hierarchy of these View Layouts.  Each of them subclass `ViewGroup`, which itself subclasses `View`.  That means it is perfectly legal to pass a layout such as `LinearLayout` as an argument for something that takes `View` as a parameter.  `ViewGroup` also contains the nested static class `LayoutParams` which is used for creating or editing layouts in code.  Keep in mind that each subclass of `ViewGroup`, such as `LinearLayout`, has its own nested static class `LayoutParams` that's a subclass of `ViewGroup.LayoutParams`.  When creating View Layouts in code, beginners will often confuse the many different available `LayoutParams` classes and run into hard to catch problems.
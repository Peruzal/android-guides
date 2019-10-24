description: To optimize layout performance, minimize the number of instantiated layouts and especially minimize deep nested layouts whenever possible.

# Optimizing Layout Performance

To optimize layout performance, minimize the number of instantiated layouts and especially minimize deep nested layouts whenever possible. This is why you should generally use a `RelativeLayout` whenever possible instead of nested `LinearLayout`. A few layout tips are included below:

 * Using nested instances of `LinearLayout` can lead to an excessively deep view hierarchy and can be quite expensive especially expensive as each child needs to be measured twice. This is particularly important when the layout is inflated repeatedly such as in a list. 
 * Layout performance slows down due to a nested LinearLayout and the performance can be improved by flattening the layout, making the layout shallow and wide rather than narrow and deep. A `RelativeLayout` as the root node allows for such layouts. So, when this design is converted to use `RelativeLayout`, the view hierarchy can be flattened significantly. 
 * Sometimes your layout might require complex views that are rarely used. Whether they are item details, progress indicators, or undo messages, you can reduce memory usage and speed up rendering by [loading the views only when they are needed](https://developer.android.com/training/improving-layouts/loading-ondemand.html).

Review the following references for more detail on optimizing your view hierarchy:

- [Optimizing Layouts](http://developer.android.com/training/improving-layouts/optimizing-layout.html)
- [Android Layout Tricks](http://android-developers.blogspot.ca/2009/02/android-layout-tricks-1.html)
- [Layout Optimization](http://code.tutsplus.com/tutorials/android-sdk-tools-layout-optimization--mobile-5245)

This is just the beginning. Refer to our [[profiling apps guide|Debugging-and-Profiling-Apps]] for more resources. 

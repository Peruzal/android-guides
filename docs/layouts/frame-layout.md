description: In a frame layout, the children are displayed with a z-index in the order of how they appear.  Put simply, the last child added to a FrameLayout will be drawn on top of all the previous children. 

# FrameLayout

In a frame layout, the children are displayed with a z-index in the order of how they appear.  Put simply, the last child added to a `FrameLayout` will be drawn on top of all the previous children.  Think of it like a stack of items, the item last put on the stack will be drawn on top of the items below it.  This layout makes it very easy to draw on top of other layouts, especially for tasks such as button placement. 

To arrange the children inside of a `FrameLayout` use the `android:layout_gravity` attribute along with whatever `android:padding` and `android:margin` you need. 

Example of FrameLayout snippet:
```xml
<FrameLayout
    android:id="@+id/frame_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <!-- Child1 is drawn first -->
    <ImageView
        android:id="@+id/child1"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:contentDescription="Image"
        android:src="@drawable/icon" />
    <!-- Child2 is drawn over Child1 -->
    <TextView
        android:id="@+id/child2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Child 2"
        android:layout_gravity="top|left" />
    <!-- Child3 is drawn over Child1 and Child2 -->
    <TextView
        android:id="@+id/child3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Child 3"
        android:layout_gravity="top|right" />
</FrameLayout>
```

In this example, an `ImageView` is set to the full size of the `FrameLayout`.  We then draw two `TextView`'s over it.

## Understanding View Layers

### Layering Introduction

You may notice that when two views overlap on screen, that one view will become hidden behind the other. Views are drawn in layers by default **based on the order they appear in the XML**. In other words, **the view at the bottom of a container is drawn on screen last** covering all previously drawn views.

This is described in the [official view docs](https://developer.android.com/reference/android/view/View.html#Drawing) and in the [How Android Draws guide](https://developer.android.com/guide/topics/ui/how-android-draws.html) with:

> The tree is largely recorded and drawn in order, with parents drawn before (i.e., behind) their children, with siblings drawn in the order they appear in the tree. If you set a background drawable for a View, then the View will draw it before calling back to its `onDraw()` method. The child drawing order can be overridden with [custom child drawing order](https://developer.android.com/reference/android/view/ViewGroup.html#setChildrenDrawingOrderEnabled(boolean)) in a ViewGroup, and with [setZ(float)](https://developer.android.com/reference/android/view/View.html#setZ(float)) custom Z values} set on Views.

In other words, the easiest way to layer is to **pay close attention to the order** in which the Views are added to your XML file within their container. **Lower down in the file means higher up in the Z-axis**.

### Elevation

In Android starting from API level 21, items in the layout file get their Z-order **both from how they are ordered within the file** as well as from their ["elevation"](https://developer.android.com/reference/android/view/View.html#attr_android:elevation) with a higher elevation value meaning the item gets a higher Z order. The value of `android:elevation` must be a dimension value such as `10dp`. We can also use the `translationZ` property to provide the same effect. Read more [about elevation here on the official guide](https://developer.android.com/training/material/shadows-clipping.html). 

### Overlapping Two Views

If we want to overlap two views on top of each other, we can do so using either a `RelativeLayout` or a `FrameLayout`. Suppose we have two images: a [background image](http://i.imgur.com/PztAiKY.jpg) and a [foreground image](http://i.imgur.com/TB2gCgz.png) and we want to place them on top of one another. The code for this can be achieved with a `RelativeLayout` such as shown below:

```xml
<RelativeLayout
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <!-- Back view should be first to be drawn first! -->
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
        android:scaleType="fitXY"
        android:src="@drawable/back_image_beach"
        />

    <!-- Front view should be last to be drawn on top! -->
    <!-- Use `centerInParent` to center the image in the container -->
    <!-- Use `elevation` to ensure placement on top (not required) -->
    <ImageView
        android:layout_width="250dp"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
        android:elevation="10dp"
        android:layout_centerInParent="true"
        android:scaleType="fitXY"
        android:src="@drawable/airbnb_logo_front"
        />
</RelativeLayout>
```

This results in the following:

<img src="http://i.imgur.com/kdFda8z.png" width="400" />

### Forcing View to the Front

We can force a view to the front of the stack to become visible using:

```java
myView.bringToFront();
myView.invalidate(); 
```

**Note:** You must be sure to call `bringToFront()` and `invalidate()` method on the highest-level view under your root view. See a [more detailed example here](http://stackoverflow.com/a/23669036). 

With these methods outlined above, we can easily control the draw order of our views. 

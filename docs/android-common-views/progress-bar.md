description: ProgressBar

# ProgressBar

Loading spinner, used to show that something is running.

```xml
<ProgressBar
    android:id="@+id/loading_spinner"
    style="?android:progressBarStyle"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Can also change the style to use a horizontal progressbar, use the style style="?android:progressBarStyleHorizontal"

```xml
<ProgressBar
    android:id="@+id/progress_bar"
    style="?android:progressBarStyleHorizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:indeterminate="false"
    android:max="100"
    android:progress="40"/>
```

description: SeekBar

# SeekBar

Displays progress and allows you to drag the handle anywhere in the bar e.g for music or video.

```xml
<SeekBar
    android:id="@+id/seek_bar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100"
    android:progress="20" />
```

### SeekBar Events

You handle the Progress event to get notified when the progress of the SeekBar changes.

```cs
var seekBar = FindViewById<SeekBar>(Resource.Id.seek_bar);
seekBar.ProgressChanged += (object sender, SeekBar.ProgressChangedEventArgs e) => { 
    Console.WriteLine($"Progress is now {e.Progress}");
};
```

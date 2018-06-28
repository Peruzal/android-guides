description: Switch

# Switch

On/off switch that can you drag to the right or left or just tap to toggle.  `SwitchCompat` is a version of the Switch widget which runs on devices back to API 7

```xml
<Switch
    android:id="@+id/backup_photos_switch"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/auto_backup_photos"
    android:textAppearance="?android:textAppearanceSmall" />
```

For backwards compatibility use `SwitchCompat`. On older devices, the control is called a `Togglebutton`.

```xml
<android.support.v7.widget.SwitchCompat
    android:checked="true"
    android:id="@+id/backup_photos_switch"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Backup photos automatically to the cloud when on wifi"
    android:textAppearance="?android:textAppearanceSmall" />
```


### Responding to events

```cs
var backupPhotosSwitch = FindViewById<Switch>(Resource.Id.backup_photos_switch);
backupPhotosSwitch.CheckedChange +=  (object sender, CompoundButton.CheckedChangeEventArgs e) => { 
    Console.WriteLine($"Switch is {e.IsChecked}");
};
```
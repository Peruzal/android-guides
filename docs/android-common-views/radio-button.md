description: RadioButton

## RadioButton

Radio buttons allow the user to select one option from a set. You should use radio buttons for optional sets that are mutually exclusive if you think that the user needs to see all available options side-by-side. If it's not necessary to show all options side-by-side, use a spinner instead.

![Radio Buttons](images/radio-buttons.png)


To create each radio button option, create a RadioButton in your layout. However, because radio buttons are mutually exclusive, you must group them together inside a RadioGroup. By grouping them together, the system ensures that only one radio button can be selected at a time.


```xml
<RadioGroup
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical">
    <RadioButton
        android:id="@+id/yes_radio_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Yes"
        android:checked="true" />
    <RadioButton
        android:id="@+id/no_radio_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="No"
        android:textAppearance="?android:textAppearanceMedium" />
    <RadioButton
        android:id="@+id/maybe_radio_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Maybe"
        android:textAppearance="?android:textAppearanceSmall" />
</RadioGroup>
```


### Attaching Click Event

The radio button have a `onClick` event. We can attach the event to the buttons in code as follows : 

```java
public void onRadioButtonClicked(View view) {
    // Is the button now checked?
    boolean checked = ((RadioButton) view).isChecked();
    
    // Check which radio button was clicked
    switch(view.getId()) {
        case R.id.radio_pirates:
            if (checked)
                // Pirates are the best
            break;
        case R.id.radio_ninjas:
            if (checked)
                // Ninjas rule
            break;
    }
}
```


### Changing the State

We can change the state of the radio button by using the `toggle()` method.

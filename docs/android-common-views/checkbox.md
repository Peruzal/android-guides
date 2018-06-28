description: CheckBox

# CheckBox

Checkboxes allow the user to select one or more options from a set. Typically, you should present each checkbox option in a vertical list.

```xml
<CheckBox
    android:id="@+id/notify_me_checkbox"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/notify_me"
    android:textAppearance="?android:textAppearanceMedium" />
```

### Responding to Events

We can access whether or not a checkbox is checked with:

```java
CheckBox checkCheese = (CheckBox) findViewById(R.id.checkbox_cheese);
// Check if the checkbox is checked
boolean isChecked = checkCheese.isChecked();
// Set the checkbox as checked
checkCheese.setChecked(true);
```

and in our activity, we can manage checkboxes using a checked listener with `OnCheckedChangeListener` as below

```java
// Defines a listener for every time a checkbox is checked or unchecked
CompoundButton.OnCheckedChangeListener checkListener = new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton view, boolean checked) {
        // compoundButton is the checkbox
        // boolean is whether or not checkbox is checked
        // Check which checkbox was clicked
        switch(view.getId()) {
            case R.id.checkbox_meat:
                if (checked) {
                    // Put some meat on the sandwich
                } else {
                    // Remove the meat
                }
                break;
            case R.id.checkbox_cheese:
                if (checked) {
                    // Cheese me
                } else {
                    // I'm lactose intolerant
                }
                break;
          }
    }
};

// This actually applies the listener to the checkboxes 
// by calling `setOnCheckedChangeListener` on each one
public void setupCheckboxes() {
    Checkbox checkCheese = (Checkbox) findViewById(R.id.checkbox_cheese);
    Checkbox checkMeat = (Checkbox) findViewById(R.id.checkbox_meat);
    checkCheese.setOnCheckedChangeListener(checkListener);
    checkMeat.setOnCheckedChangeListener(checkListener);
}
```
description: RatingBar

# RatingBar

```xml
<RatingBar
    android:id="@+id/rating_bar"
    style="?android:attr/ratingBarStyleSmall"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:numStars="5"
    android:rating="2.5"
    android:stepSize="0.5" />
```

### RatingBar Events

You can use the RatingBarChange event to subscribe to changes in the RatingBar

```cs
var ratingBar = FindViewById<RatingBar>(Resource.Id.rating_bar);
ratingBar.RatingBarChange += (object sender, RatingBar.RatingBarChangeEventArgs e) => { 
    Console.WriteLine($"Rating is {e.Rating}");
};
```
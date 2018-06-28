description: RecyclerView

# RecyclerView

# RecyclerView

Often when you rotate the screen, the app will lose the scroll position and other state of any lists on screen. To properly retain this state for `RecyclerView`, you can store the instance state `onPause` and restore `onViewCreated` as shown below:

```java
// YourActivity.java
public final static int LIST_STATE_KEY = "recycler_list_state";
Parcelable listState;

protected void onSaveInstanceState(Bundle state) {
     super.onSaveInstanceState(state);
     // Save list state
     listState = mLayoutManager.onSaveInstanceState();
     state.putParcelable(LIST_STATE_KEY, mListState);
}

protected void onRestoreInstanceState(Bundle state) {
    super.onRestoreInstanceState(state);
    // Retrieve list state and list/item positions
    if(state != null)
        listState = state.getParcelable(LIST_STATE_KEY);
}

@Override
protected void onResume() {
    super.onResume();
    if (listState != null) {
        mLayoutManager.onRestoreInstanceState(listState);
    }
}
```

Check out this [blog post](http://panavtec.me/retain-restore-recycler-view-scroll-position) and [stackoverflow post](http://stackoverflow.com/a/28262885) for more details. 

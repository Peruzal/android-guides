description: ListView

### ListView

Often when you rotate the screen, the app will lose the scroll position and other state of any lists on screen. To properly retain this state for `ListView`, you can store the instance state `onPause` and restore `onViewCreated` as shown below:

```java
// YourActivity.java
private static final String LIST_STATE = "listState";
private Parcelable mListState = null;

// Write list state to bundle
@Override
protected void onSaveInstanceState(Bundle state) {
    super.onSaveInstanceState(state);
    mListState = getListView().onSaveInstanceState();
    state.putParcelable(LIST_STATE, mListState);
}

// Restore list state from bundle
@Override
protected void onRestoreInstanceState(Bundle state) {
    super.onRestoreInstanceState(state);
    mListState = state.getParcelable(LIST_STATE);
}


@Override
protected void onResume() {
    super.onResume();
    loadData(); // make sure data has been reloaded into adapter first
    // ONLY call this part once the data items have been loaded back into the adapter
    // for example, inside a success callback from the network
    if (mListState != null) {
        myListView.onRestoreInstanceState(mListState);
        mListState = null;
    }
}
```

Check out this [blog post](https://futurestud.io/tutorials/how-to-save-and-restore-the-scroll-position-and-state-of-a-android-listview) and [stackoverflow post](http://stackoverflow.com/a/5688490) for more details. 

Note that **you must load the items back into the adapter first** before calling `onRestoreInstanceState`. In other words, don't call `onRestoreInstanceState` on the ListView until after the items are loaded back in from the network or the database. 

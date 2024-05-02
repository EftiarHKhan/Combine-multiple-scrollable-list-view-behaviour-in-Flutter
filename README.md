# Combine-multiple-scrollable-list-view-behaviour-in-Flutter

## To achieve this, you can use a `ScrollController` to listen to the scroll events of the first `ListView` and apply the same scroll offset to the second `ListView`. Here's how you can do it:

1. Create a `ScrollController` instance.
2. Assign this `ScrollController` to the first `ListView`.
3. Add a listener to the `ScrollController` that sets the scroll offset of the second `ListView` to match the first one.

### Here's the code in Dart:

```dart
// Step 1: Create a ScrollController instance
ScrollController _scrollController = ScrollController();

@override
void initState() {
  super.initState();

  // Step 3: Add a listener to the ScrollController
  _scrollController.addListener(() {
    // Get the current scroll position of the first ListView
    double scrollPosition = _scrollController.position.pixels;

    // Scroll the second ListView to match the first ListView's position
    _secondScrollController.jumpTo(scrollPosition);
  });
}

@override
Widget build(BuildContext context) {
  return Row(
    children: <Widget>[
      // First ListView with the ScrollController
      Expanded(
        child: ListView.builder(
          controller: _scrollController, // Step 2: Assign the ScrollController to the first ListView
          itemCount: items.length,
          itemBuilder: (context, index) {
            return ListTile(title: Text('Item ${items[index]}'));
          },
        ),
      ),

      // Second ListView
      Expanded(
        child: ListView.builder(
          controller: _secondScrollController,
          itemCount: items.length,
          itemBuilder: (context, index) {
            return ListTile(title: Text('Item ${items[index]}'));
          },
        ),
      ),
    ],
  );
}
```

In this code, `_scrollController` is the `ScrollController` for the first `ListView`, and `_secondScrollController` is the `ScrollController` for the second `ListView`. When the first `ListView` is scrolled, the listener updates the scroll position of the second `ListView` to match.

### Please replace `items` with your actual data source. Also, make sure to dispose of the `ScrollController` in the `dispose` method to avoid memory leaks:

```dart
@override
void dispose() {
  _scrollController.dispose();
  _secondScrollController.dispose();
  super.dispose();
}
```

This will ensure that both `ListViews` scroll in sync.

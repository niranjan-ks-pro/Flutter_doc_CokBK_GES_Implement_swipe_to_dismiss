# Flutter_doc_CokBK_GES_Implement_swipe_to_dismiss
 https://docs.flutter.dev/cookbook/gestures/dismissible
Implement swipe to dismiss
==========================

1.  [Cookbook](https://docs.flutter.dev/cookbook)
2.  [Gestures](https://docs.flutter.dev/cookbook/gestures)
3.  [Implement swipe to dismiss](https://docs.flutter.dev/cookbook/gestures/dismissible)

The "swipe to dismiss" pattern is common in many mobile apps. For example, when writing an email app, you might want to allow a user to swipe away email messages to delete them from a list.

Flutter makes this task easy by providing the [`Dismissible`](https://api.flutter.dev/flutter/widgets/Dismissible-class.html) widget. Learn how to implement swipe to dismiss with the following steps:

1.  Create a list of items.
2.  Wrap each item in a `Dismissible` widget.
3.  Provide "leave behind" indicators.

[](https://docs.flutter.dev/cookbook/gestures/dismissible#1-create-a-list-of-items)1\. Create a list of items
-------------------------------------------------------------------------------------------------------------

First, create a list of items. For detailed instructions on how to create a list, follow the [Working with long lists](https://docs.flutter.dev/cookbook/lists/long-lists) recipe.

### [](https://docs.flutter.dev/cookbook/gestures/dismissible#create-a-data-source)Create a data source

In this example, you want 20 sample items to work with. To keep it simple, generate a list of strings.

content_copy

```
final items = List<String>.generate(20, (i) => 'Item ${i + 1}');
```

### [](https://docs.flutter.dev/cookbook/gestures/dismissible#convert-the-data-source-into-a-list)Convert the data source into a list

Display each item in the list on screen. Users won't be able to swipe these items away just yet.

content_copy

```
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(items[index]),
    );
  },
)
```

[](https://docs.flutter.dev/cookbook/gestures/dismissible#2-wrap-each-item-in-a-dismissible-widget)2\. Wrap each item in a Dismissible widget
---------------------------------------------------------------------------------------------------------------------------------------------

In this step, give users the ability to swipe an item off the list by using the [`Dismissible`](https://api.flutter.dev/flutter/widgets/Dismissible-class.html) widget.

After the user has swiped away the item, remove the item from the list and display a snackbar. In a real app, you might need to perform more complex logic, such as removing the item from a web service or database.

Update the `itemBuilder()` function to return a `Dismissible` widget:

content_copy

```
itemBuilder: (context, index) {
  final item = items[index];
  return Dismissible(
    // Each Dismissible must contain a Key. Keys allow Flutter to
    // uniquely identify widgets.
    key: Key(item),
    // Provide a function that tells the app
    // what to do after an item has been swiped away.
    onDismissed: (direction) {
      // Remove the item from the data source.
      setState(() {
        items.removeAt(index);
      });

      // Then show a snackbar.
      ScaffoldMessenger.of(context)
          .showSnackBar(SnackBar(content: Text('$item dismissed')));
    },
    child: ListTile(
      title: Text(item),
    ),
  );
},
```

[](https://docs.flutter.dev/cookbook/gestures/dismissible#3-provide-leave-behind-indicators)3\. Provide "leave behind" indicators
---------------------------------------------------------------------------------------------------------------------------------

As it stands, the app allows users to swipe items off the list, but it doesn't give a visual indication of what happens when they do. To provide a cue that items are removed, display a "leave behind" indicator as they swipe the item off the screen. In this case, the indicator is a red background.

To add the indicator, provide a `background` parameter to the `Dismissible`.

lib/{step2.dart (Dismissible) → main.dart (Dismissible)} Viewed



 |

   ),

 |

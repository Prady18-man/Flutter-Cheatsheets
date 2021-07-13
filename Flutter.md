#Navigation for flutter

#Navigate to a different page
#Navigate back from a page programatically
#Get a result after a page is closed
#Override the back button on a page


#Navigate to a different page
Flutter provides us with a Navigator that we have access to. You provide it with your current BuildContext and ask it to perform some navigations.

Navigator.push(context, new MaterialPageRoute(
  builder: (context) => Page2()
));
This will push page2 onto your navigation stack.

#Navigate back from a page programatically
To remove the current page from the stack you can use the pop call on the navigator.

// Removes the current view
Navigator.pop(context);

#Get result after a page is closed
In one of the questions that I answered the dev wanted to know how to re-run a function on the calling page (The page where the navigation came from) when the newly pushed page is closed.

Luckily for us the Navigator calls are all Futures, so we can await a result from them. The way we return a result is by passing a value to the .pop method. So how you handle this is

await on your navigation call
Pass value to the .pop function
When await completes check your value matches the value passed in 2
 // In your calling widget where you want to navigate from, await your navigation result
 var navigationResult = await Navigator.push(
        context, new MaterialPageRoute(builder: (context) => Page2()));

 // Check your navigation result
 if(navigationResult == 'my_value') {
  print('I have received results from the navigation');
 }

 // Where you perform your pop call
 Navigator.pop(context, 'my_value');

#Override the back button on a page If you don’t want the back button to navigate away from your current view you can use a widget called WillPopScope. Surround your scaffold widget with it, and return a false value to the onWillPop call. False tells the system they don’t have to handle the scope pop call.

Note: You should not surround your entire app with this (Material App). You should be using this per page widget that you want the functionality to run on. Surround your Scaffold for your page.

@override
Widget build(BuildContext context) {
  return WillPopScope(
    onWillPop: () => Future.value(false),
    child: Scaffold(
      body: Container(
        child: Center(
          child: Text('Page 2',
              style: TextStyle(fontSize: 30.0, fontWeight: FontWeight.bold)),
        ),
      ),
    ),
  );
}
If you to still want to return a custom value when the app navigates back you can perform the pop call before you return false.

WillPopScope(
      onWillPop: () async {
          // You can await in the calling widget for my_value and handle when complete.
          Navigator.pop(context, 'my_value');
          return false;
        },
        ...
);

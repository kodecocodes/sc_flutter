# Screencast Metadata

## Screencast Title

Getting Started with Flutter in Android Studio

## Screencast Description

Dive into the Flutter framework for building iOS and Android apps in a single codebase, by writing a cross-platform app using Android Studio.

## Language, Editor and Platform versions used in this screencast:

* **Language:** Dart
* **Platform:** iOS and Android
* **Editor**: Android Studio 3


Hey everybody! Joe here, and in this screencast we're going to see how to get started with Flutter, a cross-platform framework from Google for iOS and Android development.

[Slide 1 - Cross-Platform Tools]

Since the iOS and Android platforms exploded onto the scene a decade ago, **cross-platform development** has been a goal across the mobile development world. Each toolset has pros and cons and they each have met with varying degrees of success in the mobile industry.

[Slide 2 - Flutter]

**Flutter** from Google is a more recent cross-platform framework, and is now in beta. Flutter features **fast development cycles**, **fast UI rendering** and **unique UI design**, and **native app performance** on both platforms. Flutter allows for a **reactive** and **declarative** style of programming. Flutter apps are written using the **Dart** programming language. Dart shares many of the same features as other modern languages such as Kotlin and Swift.  Dart uses both **Ahead-Of-Time** or AOT compilation, which leads to fast performance, and also **Just-In-Time** or JIT compilation which improves the development workflow by allowing **hot reload** capability to refresh the UI during development.

[Slide 4 - What you'll learn]

In this screencast, we'll build a Flutter app that queries the **GitHub API** for team members in a GitHub organization and displays the team member information in a scrollable list. We'll develop the app using both the iOS Simulator and an Android emulator. As we build out the app, you'll learn a number of Flutter fundamentals and techniques. You'll also learn a little Dart along the way! :]

## Scripted Demo

Flutter development can be done on macOS, Linux, or Windows. While you can use any editor with the Flutter toolchain, there are IDE plugins for both **Android Studio** and **Visual Studio Code** that can ease the development process. We'll use Android Studio on macOS for this screencast.

### Setting up your development environment

Instructions for setting up your development machine with the Flutter framework can be found at flutter.io. The steps vary a bit by platform, but for the most part are:

* Clone the Flutter git repository
* Add the Flutter bin directory to your path
* Run the `flutter doctor` command, which installs the Flutter framework including Dart and alerts you to any missing dependencies
* Install missing dependencies
* Set up your IDE with a Flutter plugin/extension
* Test drive an app

The instructions provided on the Flutter web site are very well done and allow you to easily setup a development environment on your platform of choice.

### Creating a new project

In Android Studio with the Flutter and Dart plugins installed, we choose **Start a new Flutter project** from the **Welcome to Android Studio** window.

We pick Flutter Application and hit Next.

We'll name the project ghflutter, choose a location to save the project files, and hit Next.

We enter our company domain or can also use something like example.com and then hit Finish.

Android Studio then creates the template project and gets any necessary packages.

You need to make sure your project panel is set to Project and not Android to see the project structure. There are folders for iOS and Android, as well as a **lib** folder that contains **main.dart** and other code that applies to both platforms. We'll be working in the lib folder only in this tutorial.

We'll start by replacing the template code in main.dart with a simple app:

```dart
import 'package:flutter/material.dart';

void main() => runApp(new GHFlutterApp());


class GHFlutterApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'GHFlutter',
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('GHFlutter'),
        ),
        body: new Center(
          child: new Text('GHFlutter'),
        ),
      ),
    );
  }
}
```

The `main()` function near the top uses the arrow syntax for a single line function to run the app. We have one class for the app named `GHFlutterApp`.

You see here that the app itself is a `StatelessWidget`. Most entities in a Flutter app are **widgets**, either stateless or stateful. You override the widget `build()` method to create your app widget. We're using the **MaterialApp** widget that provides a number of components needed for apps that follow Material Design.

For this getting started screentest, we'll remove the test file **widget_test.dart** in the `test` folder from the project by selecting it and hitting the Delete key.

We have both the iOS Simulator and an Android emulator already running.

We make sure the Simulator is chosen in the Flutter device selection box and hit the run button to build and run the app. You'll see the Run panel open and, you'll see Xcode being used to build the project. When running on an Android emulator, you'll see Gradle being invoked to do the build.

The app runs in the iOS simulator, right from Android Studio.

Switching to the Android emulator, we can run the app again and see the Android version.

The slow mode banner you see indicates that the app is running in a debug mode.

### Hot Reload

One of the coolest aspects of Flutter development is being able to **hot reload** your app as you make changes. This is similar to something like Android Studio's **Instant Run**.

Without stopping the running app, we change the app bar string to something else:

```dart
appBar: new AppBar(
  title: new Text('GHFlutter App'),
),
```

Now we click the hot reload button on the toolbar.

Within a second or two you see the change reflected in the running app.

Since Flutter is in beta, the hot reload feature may not always work, but overall it's a great time saver when you're building out your UI.

### Importing a File

Rather than keep all your Dart code in the single main.dart file, you'll want to be able to import code from other classes you create. 

Create a file named **strings.dart** in the lib folder by right-clicking on **lib** and choosing **New Dart File.**

We'll add a Strings class to the new file.

```dart
class Strings {
  static String appTitle = "GHFlutter";
}
```

We add the following import to the top of **main.dart**

```dart
import 'strings.dart';
```

And then change the hardcoded strings to use the new strings file.

```dart
class GHFlutterApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: Strings.appTitle,
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text(Strings.appTitle),
        ),
        body: new Center(
          child: new Text(Strings.appTitle),
        ),
      ),
    );
  }
}
```

We build and run the app, and should see no change, but we're now using strings from the strings file, which will help if we localize the app later on.

## Interlude

[Slide 5 - Widgets]

Elements of a Flutter UI are called **widgets**. Widgets are designed to be **immutable**, since using immutable widgets helps keep the app UI lightweight. There are two fundamental types of widgets you will use: **Stateless** widgets that depend only upon their own configuration info, such as a static image in an image view, and **Stateful** widgets that need to maintain dynamic information and do so by interacting with a **State** object. Both stateless and stateful widgets redraw in Flutter apps on every frame, with stateful widgets delegating their configuration to a **State** object.

[Slide 6 - ListView]

Dart provides a **ListView** widget that will let you show the data in a list. ListView acts like a **RecyclerView** on Android and a **UITableView** on iOS, recycling views as the user scrolls through the list to achieve smooth scrolling performance.

## Scripted Demo

To get started with making our own widgets, we create a new file name ghflutterwidget.dart and add a GHFlutterWidget class:

```dart
class GHFlutterWidget extends StatefulWidget {
  @override
  createState() => new GHFlutterState();
}
```

We can hit Option+Return to pull in the correct imports. We've made a `StatefulWidget` subclass and are overriding the `createState()` method to create its state object. Now we add a `GHFlutterState` class below `GHFlutterWidget`:

```dart
class GHFlutterState extends State<GHFlutterWidget> {
}
```

`GHFlutterState` extends `State` with a parameter of `GHFlutterWidget`.

The primary task you have when making a new widget is to override the `build()` method that gets called when the widget is rendered to the screen.

We add a `build()` override to `GHFlutterState` by hitting Ctrl+I:

```dart
@override
Widget build(BuildContext context) {   
}
```

And then we fill out `build()` as follows:

```dart
@override
Widget build(BuildContext context) {
  return new Scaffold (
    appBar: new AppBar(
      title: new Text(Strings.appTitle),
    ),
    body: new Text(Strings.appTitle),
  );
}
```

A **Scaffold** is a container for material design widgets. It acts as the root of a widget hierarchy. We've added an `AppBar` and a `body` to the scaffold, and each contains a `Text` widget.

We update `GHFlutterApp` so that it uses the new `GHFlutterWidget`  as its `home` attribute, instead of building a scaffold of it's own:

```dart
class GHFlutterApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: Strings.appTitle,
      home: new GHFlutterWidget(),
    );
  }
}
```

We add the necessary imports and build and run the app, and see the new widget in action.

Not much has changed yet, but now we're setup to build out the new widget.

### Making Network Calls

Earlier we imported our own strings.dart file into main.dart. You can similarly import other packages that are part of the Flutter framework and Dart.

We'll use packages available in the framework to make an HTTP network call and also parse the resulting response JSON into Dart objects. We add two new imports at the top of **ghflutterwidget.dart**:

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

import 'strings.dart';
```

We see an indicator on the imports that are currently unused.

Dart apps are single-threaded, but Dart provides support for running code on other threads as well as running asynchrounous code that does not block the UI thread using an **async/await** pattern.

We're going to make an asynchronous network call to retrieve a list of GitHub teams members. We add an empty list as a property at the top of `GHFlutterState`, and also add a property to hold a text style:

```dart
var _members = [];

final _biggerFont = const TextStyle(fontSize: 18.0);
```

The underscores at the beginning of the names make the members of the class **private**.

To make the asynchronous HTTP call, we add a method `_loadData()` to `GHFlutterState`:

```dart
_loadData() async {
  String dataURL = "https://api.github.com/orgs/raywenderlich/members";
  http.Response response = await http.get(dataURL);
  setState(() {
    _members = JSON.decode(response.body);
  });
}
```

We've added the `async` keyword onto `_loadData()` to tell Dart that it's asynchronous, and also the `await` keyword on the `http.get()` call that is blocking. We're using a `dataUrl` value that is set to the GitHub API endpoint that retrieves members for a GitHub organization. Feel free to change the organization to your own if you're following along.

When the HTTP call completes, we pass a callback to `setState()` that runs synchronously on the UI thread. In this case, we're decoding the JSON response and assigning it to the `_members` list.

Add an `initState()` override to `GHFlutterState` that calls `_loadData()` when the state is initialized:

```dart
@override
void initState() {
  super.initState();

  _loadData();
}
```

### Using a ListView

Now that we have a Dart list of members, we need a way to display them in a list in the UI. 

We add a `_buildRow()` method to `GhHFlutterState`:

```dart
Widget _buildRow(int i) {
  return new ListTile(
    title: new Text("${_members[i]["login"]}", style: _biggerFont)
  );
}
```

We're returning a **ListTile** widget that shows the "login" value parsed from the JSON for the ith member, and also using the text style we created before.

We update the build method to have it's body be a **ListView.builder**:

```dart
body: new ListView.builder(
  padding: const EdgeInsets.all(16.0),
  itemCount: _members.length,
  itemBuilder: (BuildContext context, int position) {
    return _buildRow(position);
  }),
```

We've added padding, set the `itemCount` to the number of members, and set the `itemBuilder` using `_buildRow()` for a given position.

We can try a hot reload, but may get a "Full restart may be required" message. If so, we hit the run button to build and run the app:

That's just how easy it is to make a network call, parse the data, and show the results in a list!

## Parsing to Custom Types

In the previous section, the JSON parser took each member in the JSON response and added it to the `_members` list as a Dart **Map** type, the equivalent of a **Map** in Kotlin or a **Dictionary** in Swift.

However, you'll also want to be able to use your own custom types.

We add a new `Member` type to the a new file **member.dart** file:

```dart
class Member {
  final String login;
  final String avatarUrl;

  Member(this.login, this.avatarUrl);
}
```

A member has a `login` property, an `avatarUrl` property, and a constructor.

We update the `_members` declaration in `GHFlutterState` so that it is a list of `Member` objects:

```dart
var _members = <Member>[];
```

We update `_buildRow()` to use the `login` property on the `Member` object instead of using the "login" key on the map, and also add a leading attribute that will use a CircleAvatar and NetworkImage to show the member image:

```dart
Widget _buildRow(int i) {
  return new ListTile(
    title: new Text("${_members[i].login}", style: _biggerFont),
    leading: new CircleAvatar(
      backgroundColor: Colors.green,
      backgroundImage: new NetworkImage(_members[i].avatarUrl)
    ),
  );
}
```

Now we update the callback sent to `setState()` in `_loadData()` to turn the decoded map into a `Member` object and add it to the list of members:

```dart
setState(() {
  final membersJSON = JSON.decode(response.body);

  for (var memberJSON in membersJSON) {
    final member = new Member(memberJSON["login"], memberJSON["avatar_url"]);
    _members.add(member);
  }
});
```

You'll likely see an error if you try a hot reload, but if we build and run the app and we should see the list of members and their avatars.

### Adding a Theme

We can easily add a theme to the app by adding a **theme** attribute to the `MaterialApp` we create in **main.dart**:

```dart
class GHFlutterApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: Strings.appTitle,
      theme: new ThemeData(primaryColor: Colors.green.shade800),
      home: new GHFlutterWidget(),
    );
  }
}  
```

We're using a shade of green as a Material Design color value for the theme.

We build and run the app to see the new theme in action.

We've mostly been working in the Android emulator. We can also run the final themed app in the iOS Simulator.

Now that's what I call cross-platform! :]

At this point, you should have a good feel for making a simple app for both iOS and Android using the Flutter framework. We've covered setting up your Flutter dev environment, creating a new project, creating your own widgets, making network calls, and showing lists of items in a ListView.

You can learn more about Flutter at the official site flutter.io, and stay tuned for many more Flutter screencasts and tutorials at raywenderlich.com. Thanks for watching!

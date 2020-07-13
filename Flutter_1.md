## Welcome to the Basics of Flutter

This series will give a complete description: how to get started with Flutter development, how to quickly develop a complete Flutter app from 0, supporting the high-completion Flutter open source project [A_Complete_Guide_To_Flutter] (), providing the development of Flutter Skills and problem handling, then in-depth source code and actual combat for you to fully analyze Flutter.

## 1. Basics

* This article mainly deals with: environment construction, Dart language, the foundation of Flutter. *

### 1. Environment construction

Flutter's environment is very worry-free, especially for Android developers, only in Android Stuido
Install the plug-in on the GitHub Clone Flutter project and execute the flutter doctor command to complete the configuration. In fact, the Chinese website [Build Futter Development Environment] (flutter development environment) is already very Intimate and detailed, you won't encounter any problems from the beginning of the platform.

It is mainly necessary to pay attention here, because of some force majeure reasons, domestic users sometimes need to configure Flutter agents, and domestic users also search in https://pub.flutter-io.cn when searching for Flutter third-party packages , Below is the address that needs to be configured to the environment variable. *(It is also quite interesting to run IOS under and Android Studio)*

```
///win can be directly configured to the environment for editing, mac is configured to bash_profile or zsh
export PUB_HOSTED_URL=https://pub.flutter-io.cn //Domestic users need to set
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn //Domestic users need to set
```
### 2. Flutter in Dart

Today, the cross-platform development field is dominated by JS, the emergence of Dart language is undoubtedly a clear stream. As a latecomer, the Dart language has many shadows of Java, Kotlin, and JS, so it is undoubtedly very friendly to Android native developers and front-end developers.

The official also provides documentation including iOS, React Native and other developers migrating to Flutter, so please don't worry, the Dart language will not be the threshold for you to master Flutter, even as a developer, you can look at the code even if you don't understand Dart Fumble.

Come on, the following mainly by comparison, briefly talk about some of the characteristics of Dart, mainly related to the use of Flutter.


#### 2.1 Basic types

-var can define variables, such as `var tag = "666"`, which is similar to JS, Kotlin and other languages. At the same time, Dart is also a semi-dynamically typed language and supports closures.

-`Dart` belongs to a **strongly typed language**, but you can use `var` to declare variables, `Dart` will **self-infer data types**, so `var` is actually a compile-time "syntax sugar". **`dynamic` means dynamic type**. After being compiled, it is actually an `object` type. No type checking is performed during compilation, but type checking is performed at runtime.

-The number type in Dart is divided into `int` and `double`, where long in java corresponds to the int type in Dart, and there is no float type in Dart.

-Only bool type in Dart can be used for if and other judgments. Unlike JS, it is illegal to use `var g = "null"; if(g){}`.

-In Dart, switch supports String type.

#### 2.2, variables

-Dart does not need to set `setter getter` method for variables, which is similar to kotlin and other languages. All the basic types and classes in Dart inherit Object. The default value is NULL, with its own getter and setter. If it is final or const, then it has only one getter method.

-In Dart, final and const represent constants, such as `final name ='GSY'; const value = 1000000; `At the same time, the combination of `static const` represents a static constant, where the value of const is determined at compile time, and the final value must be run Only then.

-The value under Dart needs to be specified explicitly when used as a character string. For example: `int i = 0; print("aaaa" + i);` This is not supported, you need to use `print("aaaa" + i.toString());`, which is different from Java and JS, **So when using dynamic types, you need to be careful not to use the number type as a String. **

-Arrays in Dart are equal to lists, so `var list = [];` and `List list = new List()` can simply be seen as the same.

#### 2.3 Method

-Under Dart, `??` and `??=` are operators, for example: `AA ?? "999"` means if AA is empty, return 999; `AA ??= "999"` means if AA is empty , Set AA to 999.

-Dart method can set *parameter default value* and *specify name*. For example: `getDetail(Sting userName, reposName, {branch = "master"}){} `method, if the branch is not set here, the default is "master". *Parameter type* can be specified or not specified. Call effect: `getRepositoryDetailDao("aaa", "bbbb", branch: "dev");`

-Dart is not like Java, there are no keywords public, private and other modifiers, `_` under the horizontal directly represents private, but there is `@protected` annotation.

-Multiple constructors in Dart can be realized by the following code. There can only be one default constructor, and a model with empty parameters can be created through the `Model.empty()` method. In fact, the method name is as you like, and when the variable is initialized, you only need to pass `this.name` in the constructor Can be specified in:

```
class ModelA {
   String name;
   String tag;
  
   //The default constructor, assigned to name and tag
   ModelA(this.name, this.tag);

   //Return an empty ModelA
   ModelA.empty();
  
   //Return a ModelA with name set
   ModelA.forName(this.name);
}

``` 
#### 2.4, Flutter

Flutter supports `async`/`await`, **as shown in the following code**, `async`/`await` is actually just syntactic sugar, and it will eventually be compiled into a "Future" object returned from Flutter, and then through the then` Go to the next step. If it returns `Future`, then you can use `then().then.()` streaming.

```
  ///The simulation waits for two seconds and returns OK
  request() async {
    await Future.delayed(Duration(seconds: 1));
    return "ok!";
  }

  ///After getting "ok!", modify "ok!" to "ok from request"
  doSomeThing() async {
    String data = await request();
    data = "ok from request";
    return data;
  }

  ///Print result
  renderSome() {
    doSomeThing().then((value) {
      print(value);
      ///Output ok from request
    });
  }
```

-`SetState` in Flutter is very visual of React Native, and Flutter also manages the state of the data through the state across frames, which will be discussed in detail later.

-Everything is rendered by Widget in Flutter, and the Widget is returned through the `build` method. This is the same mode as in React Native, which returns the component to be rendered through the `render` function.
-The async* / yield corresponding to Stream can also be used for asynchronous, which will be described later.

### 3. Flutter Widget

In Flutter, all displays are Widgets, and Widgets are the foundation of everything, and use responsive mode for rendering.

We can modify the data, and then set the data with `setState`, Flutter will automatically update the Widget with the bound data, **So all you need to do is implement the Widget interface, and bind it with the data**.

Widgets are divided into *stateful* and *stateless*. In Flutter, each page is a frame, and statelessness is maintained at that frame. Stateful widgets actually create new ones when the data is updated. Widget, but State realizes the synchronous saving of data across frames.

&emsp;

> Here is a small tip. When you enter `stl` in the code box, you can automatically pop up the template option for creating stateless controls, and when you enter `stf`, you will pop up the template option for creating stateful widgets.
>
>When the code is formatted, the commas inside and outside the brackets will affect the position of the line break during formatting.
>
>If you feel that the default line break is too short, you can set the value you accept in Settings-Editor-Code Style-Dart-Wrapping and Braces-Hard wrap at.

&emsp;  

#### 3.1, StatelessWidget

Go directly to the topic, as shown in the following code is a simple implementation of stateless widget. **Inherit StatelessWidget and return a laid out widget through the `build` method**. Maybe you are still not familiar with Flutter's built-in controls, but **Don't worry, take it easy**, we will introduce it in detail later, you just need to know that a stateless Widget is so simple.


Widget and Widget are nested by `child:`. Some of the widgets can only have one child, such as the `Container` below; some widgets can have multiple children, that is, `children`, such as `Column layout, the code below is the Container Widget nested Text Widget.

```
import'package:flutter/material.dart';

class DEMOWidget extends StatelessWidget {
  final String text;

  //Data can be passed in through the constructor
  DEMOWidget(this.text);

  @override
  Widget build(BuildContext context) {
    //Return the control you need here
    //There is no comma at the end here, which is different for formatting code.
    return Container(
      //White background
      color: Colors.white,
      //Dart syntax, ?? means that if the text is empty, it returns the content after the trailing number.
      child: Text(text ?? "This is stateless DMEO"),
    );
  }
}

```


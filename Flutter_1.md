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


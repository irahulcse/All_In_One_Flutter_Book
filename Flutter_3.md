##  Flutter Packing and Flutter Development Process:

As the third in a series of articles, this article will focus on showing you: the packaging process of the Flutter development process, the comparison of APP packages, detailed skills and problem handling, the packaging of Flutter described in this article, and the various encounters during the development process Such questions and details can be regarded as a complement to the previous two articles.

Article summary address:

1. Flutter complete combat actual combat series article column

2. Flutter's World Column Series


### Flutter App Packing and Building in VS Code:

1. Packing
First, let's look at the results first. As shown in the following table, it is a vertical and horizontal comparison of Flutter and React Native, iOS and Android.

From the table above we can see:

Fluuter's apk will be smaller than ipa, part of the reason is that the Skia used by Flutter comes on Android.

Comparing React Native horizontally, although the project is not exactly the same, but most of the functions are consistent, Flutter's Apk is indeed smaller. Here is another detail. The ipa package of rn is much smaller, which is actually because javascriptcore is built into ios.

Those who are interested in the above content can take a look at "In-depth analysis of cross-platform mobile development".


1. Android

//check1.jpg

In the packaging of Android, the author has basically encountered no problems. In the android/app/build.grade file, configure applicationId, versionCode, versionName and signature information, and finally complete the compilation through flutter build app. The successful programming package is under build/app/outputs/apk/release.


## Some Widgets in Flutter are as follows:


### 1. AppBar

In Flutter, AppBar is a commonly used Widget, and AppBar is not only used as a title bar, but the leading and bottom on AppBar are also useful functions.

* AppBar's bottom supports TabBar by default, which is the common top Tab effect. This is actually because TabBar implements the preferredSize of PreferredSizeWidget. So as long as your control implements preferredSize, it can be used in the bottom of the AppBar. For example, the search bar in the following figure, this is the page under TabView and the AppBar is practical.

//flutterimage.jpg

* `leading` ：通常是左侧按键，不设置时一般是 Drawer 的图标或者返回按钮。

* `flexibleSpace` ：位于 `bottom ` 和 `leading ` 之间。

### 2. Button

Buttons in Flutter, such as `FlatButton` have margins and minimum size by default. So if you want the key effect without *padding, margin, border, default size*, etc., one of the ways is as follows:

```
///
new RawMaterialButton(
        materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        padding: padding ?? const EdgeInsets.all(0.0),
        constraints: const BoxConstraints(minWidth: 0.0, minHeight: 0.0),
        child: child,
        onPressed: onPressed);
```

If you go to Flex again, as shown below, a controllable fill button will come out.

```
new RawMaterialButton(
        materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        padding: padding ?? const EdgeInsets.all(0.0),
        constraints: const BoxConstraints(minWidth: 0.0, minHeight: 0.0),
        ///flex
        child: new Flex(
          mainAxisAlignment: mainAxisAlignment,
          direction: Axis.horizontal,
          children: <Widget>[],
        ),
        onPressed: onPressed);
```


If you go to Flex again, as shown below, a controllable fill button will come out.

```
new RawMaterialButton(
        materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        padding: padding ?? const EdgeInsets.all(0.0),
        constraints: const BoxConstraints(minWidth: 0.0, minHeight: 0.0),
        ///flex
        child: new Flex(
          mainAxisAlignment: mainAxisAlignment,
          direction: Axis.horizontal,
          children: <Widget>[],
        ),
        onPressed: onPressed);
```

### 3. Assignment of StatefulWidget

Here we take the active assignment of `TextField` as an example. In fact, in Flutter, the state or data is passed to the stateful widget, generally through various controllers. Such as the active assignment of `TextField`, as shown in the following code:

```

 final TextEditingController controller = new TextEditingController();

 @override
 void didChangeDependencies() {
    super.didChangeDependencies();
    ///By creating a new TextEditingValue for the value of the controller
    controller.value = new TextEditingValue(text: "Fill in the input box with parameters");
 }

 @override
  Widget build(BuildContext context) {
    return new TextField(
     ///controller
      controller: controller,
      onChanged: onChanged,
      obscureText: obscureText,
      decoration: new InputDecoration(
        hintText: hintText,
        icon: iconData == null? null: new Icon(iconData),
      ),
    );
  }
```

In fact, `TextEditingValue` is `ValueNotifier`, in which the setter method of `value` is overloaded and the `notifyListeners` method will be triggered once it is changed. In `TextEditingController`, the data changes are monitored by calling `addListener`, so that the UI is updated.

Of course, there is a simpler and more crude way of assigning: **pass an object class A object, use the variable of object A.b inside the control to bind the control, and update it externally via setState({ A.b = b2})**.

### 4, GlobalKey

In Flutter, to actively change the state of child controls, you can also use `GlobalKey`. For example, you need to actively call `RefreshIndicator` to display the refresh status, as shown in the following code.

```

 GlobalKey<RefreshIndicatorState> refreshIndicatorKey;
  
 showForRefresh() {
    ///Display refresh
    refreshIndicatorKey.currentState.show();
  }

  @override
  Widget build(BuildContext context) {
    refreshIndicatorKey = new GlobalKey<RefreshIndicatorState>();
    return new RefreshIndicator(
      key: refreshIndicatorKey,
      onRefresh: onRefresh,
      child: new ListView.builder(
        ///·····
      ),
    );
  }
```
### 5. Hotload and Package

Flutter is in *JIT* and *AOT* modes under Debug and Release, respectively, and supports hotload under DEBUG, and is very silky. However, it should be noted that: **If a new third-party package is installed during the development process, and if the new third-party package contains native code, you need to stop and re-run it. **

Under the `pubspec.yaml` file is our package dependency directory, where `^` stands for greater than or equal to, under normal circumstances, `upgrade` and `get` can both achieve the role of downloading the package. But: **upgrade will update the version of the package under the `pubspec.lock` file when the package is updated**.


## Three, problem handling in Flutter:


* `Waiting for another flutter command to release the startup lock`: If you encounter this problem:

```
  1. Open the flutter installation directory /bin/cache/
  2. Delete the lockfile file
  3. Restart AndroidStudio
```

* Yellow line under dialog
[yellow-lines-under-text-widgets-in-flutter] (https://stackoverflow.com/questions/47114639/yellow-lines-under-text-widgets-in-flutter): In showDialog, it is not used by default Scaffold, this time the text has a yellow overflow line prompt, you can use Material to wrap a layer of processing.

* Problems with TabBar + TabView + KeepAlive
It can be solved by TabBar + PageView, which can be seen in [Chapter 2] (https://www.jianshu.com/p/5768a999790d).


>Since then, the third chapter is finally over! (///▽///)

### Resource recommendation

* Github: [https://github.com/CarGuo/](https://github.com/CarGuo)
* **Open source Flutter complete project: https://github.com/CarGuo/GSYGithubAppFlutter**
* **Open source Flutter multi-case learning project: https://github.com/CarGuo/GSYFlutterDemo**
* **Open source Fluttre combat e-book project: https://github.com/CarGuo/GSYFlutterBook**

##### Complete open source project recommendation:

* [GSYGithubAppWeex](https://github.com/CarGuo/GSYGithubAppWeex)
* [GSYGithubApp React Native](https://github.com/CarGuo/GSYGithubApp)


<!-- ![Will we see you again? ](http://img.cdn.guoshuyu.cn/20190604_Flutter-3/image9) -->

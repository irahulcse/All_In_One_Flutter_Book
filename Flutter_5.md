As the fifth in a series of articles, this article mainly explores some interesting principles in Flutter to help us better understand and develop.

## Article summary address:

> [Flutter complete combat actual combat series article column] (https://juejin.im/collection/5db25bcff265da06a19a304e)
>
> [Flutter World Series Article Column] (https://juejin.im/collection/5db25d706fb9a069f422c374)

#### 1. Mixins

Mix it in (￣.￣)! Yes, Flutter uses Dart to support Mixin, and Mixin can better solve the problems that often occur in **multiple inheritance**, such as: **method priority is confused, parameter conflicts, and class structure becomes complicated* *and many more.

The definition of Mixin will be more complicated to explain, let's get the code out of it directly. As shown in the following code, in Dart `with` is used for mixins. It can be seen that `class G extends B with A, A2`, and after executing the a, b, and c methods of G, it outputs `A2.a(), A.b(), B.c()`. So to put it simply, **the same method is overwritten, and after the with will overwrite the previous **.

```
class A {
  a() {
    print("A.a()");
  }

  b() {
    print("A.b()");
  }
}

class A2 {
  a() {
    print("A2.a()");
  }
}

class B {
  a() {
    print("B.a()");
  }

  b() {
    print("B.b()");
  }

  c() {
    print("B.c()");
  }
}

class G extends B with A, A2 {

}


testMixins() {
  G t = new G();
  t.a();
  t.b();
  t.c();
}

/// *********************** Output***********************
///I/flutter (13627): A2.a()
///I/flutter (13627): A.b()
///I/flutter (13627): B.c()

```

Next we continue to modify the code. As shown below, we define an abstract class of `Base`, and `A, A2, B` all inherit it, and then execute `super()` operation after `print`.

From the final input, we can see that all the methods in `A, A2, B` have been executed and executed only once, and the order of execution is also related to the order of with**. If you remove the super class A.a() method in the code below, you will not see the output of `B.a()` and `base a()`.

```

abstract class Base {
  a() {
    print("base a()");
  }

  b() {
    print("base b()");
  }

  c() {
    print("base c()");
  }
}

class A extends Base {
  a() {
    print("A.a()");
    super.a();
  }

  b() {
    print("A.b()");
    super.b();
  }
}

class A2 extends Base {
  a() {
    print("A2.a()");
    super.a();
  }
}

class B extends Base {
  a() {
    print("B.a()");
    super.a();
  }

  b() {
    print("B.b()");
    super.b();
  }

  c() {
    print("B.c()");
    super.c();
  }
}

class G extends B with A, A2 {

}

testMixins() {
  G t = new G();
  t.a();
  t.b();
  t.c();
}

///I/flutter (13627): A2.a()
///I/flutter (13627): A.a()
///I/flutter (13627): B.a()
///I/flutter (13627): base a()
///I/flutter (13627): A.b()
///I/flutter (13627): B.b()
///I/flutter (13627): base b()
///I/flutter (13627): B.c()
///I/flutter (13627): base c()

```

#### 2. WidgetsFlutterBinding

Having said all that, what use is Mixins in Flutter? At this time we have to look at the "glue class" in Flutter: `WidgetsFlutterBinding`.

**WidgetsFlutterBinding** RunApp will be called when Flutter starts. As the entrance to the App, it will definitely need to undertake all kinds of initialization and function configuration. In this case, the role of Mixins is reflected.

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image1)

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image2)

From the above picture, we can see that **WidgetsFlutterBinding** itself does not have any code, mainly inherits `BindingBase`, and then sticks to various types of **Binding** through with, these **Binding** are also all Inherited `BindingBase`.

Can you see it? Here, each **Binding** can be used alone, or it can be "glued" to **WidgetsFlutterBinding**. Is the effect of doing so compared to the structure inherited from the first level? Is it clearer?

Finally, we print the execution order, as shown in the figure below, so as expected (￣▽￣)ﾉ.

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image3)


### Second, InheritedWidget

**InheritedWidget** is an abstract class that plays a very important role in Flutter, or you have not used it directly, but you must have used the encapsulation associated with it.

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image4)

As shown in the figure above, **InheritedWidget** mainly implements two methods:

* Created ʻInheritedElement`, the **Element** belongs to a special Element, mainly adding itself to the mapping table **`_inheritedWidgets`** [Note 1], which is convenient for children and grandchildren to obtain; meanwhile, through `notifyClients` Method to update dependencies.

* Added `updateShouldNotify` method, when the method returns true, then the instances that depend on the Widget will be updated.

So we can simply understand: **InheritedWidget through the `InheritedElement` to achieve support for bottom-up search (because it is added to `_inheritedWidgets`), and also has the ability to update its descendants. **

> Note 1: Each Element has a `_inheritedWidgets`, which is a `HashMap<Type, InheritedElement>`, which saves the mapping relationship between **InheritedWidget** appearing in the upper node and its corresponding element.


![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image5)

Then we look at **BuildContext**, as shown above, **BuildContext** is actually just an interface, and **Element** implements it. `InheritedElement` is a subclass of **Element**, so **Every InheritedElement instance is a BuildContext instance**. At the same time, the BuildContext passed in our daily use is also an Element.


So when we encounter the need to share State, if it will be too troublesome to pass state layer by layer to achieve sharing, what about the **InheritedWidget** above?

Is it possible to put all the State to be shared in an InheritedWidget and then directly use it in the widget used? The answer is yes! So the following types of code: usually *focus, theme color, multi-language, user information*, etc. belong to the global shared data in the App, they will be obtained through BuildContext (InheritedElement).

```
///Fold away the keyboard
FocusScope.of(context).requestFocus(new FocusNode());

/// Theme color
Theme.of(context).primaryColor

/// multi-language
Localizations.of(context, GSYLocalizations)
 
/// Get user information through Redux
StoreProvider.of(context).userInfo

/// Get user information through Redux
StoreProvider.of(context).userInfo

/// Obtain user information through Scope Model
ScopedModel.of<UserInfo>(context).userInfo

```

In summary, we start with `Theme`.

As shown in the code below, by setting the theme data to `MaterialApp`, you can get the theme data and bind it through `Theme.of(context)`. When the theme data of `MaterialApp` changes, the corresponding Widget color will also change. Why? (ｷ｀ﾟДﾟ´)!!?


```
  ///Add topic
  new MaterialApp(
      theme: ThemeData.dark()
  );
  
  ///Use the theme color
  new Container( color: Theme.of(context).primaryColor,
```

Through layer-by-layer searching of the source code, you can find such nesting: `MaterialApp -> AnimatedTheme -> Theme -> _InheritedTheme extends InheritedWidget`, so the entry through `MaterialApp` is actually nested under **InheritedWidget**.

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image6)

As shown in the figure above, the theme data obtained through `Theme.of(context)` is actually obtained through `context.inheritFromWidgetOfExactType(_InheritedTheme)`, and **BuildContext** is implemented in **Element** The `inheritFromWidgetOfExactType` method is as follows:

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image7)

So, remember **`_inheritedWidgets`** mentioned above? Now that `InheritedElement` already exists in _inheritedWidgets, it’s right to use it.

> Previous: `InheritedElement` in InheritedWidget, which is a special Element, mainly adding itself to the mapping table _inheritedWidgets

Finally, as shown in the figure below, in **InheritedElement**, `notifyClients` determines whether it is updated through the `UpdateShouldNotify` method of `InheritedWidget`, for example, in _Theher** the `_InheritedTheme` is:

```
bool updateShouldNotify(_InheritedTheme old) => theme.data != old.theme.data;
```

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image8)

> **So essentially, the core of Theme, Redux, Scope Model, Localizations are all `InheritedWidget`. **


### 3. Memory

Recently, Xianyu Technology released ["Flutter's Zen Memory Optimization"] (https://yq.aliyun.com/articles/651005). In this article, I made an in-depth exploration of Flutter's memory, and one of the very interesting findings Yes:

> * ImageCache in Flutter caches ImageStream objects, that is, caches an image object that is loaded asynchronously.
> * Until the picture is loaded and decoded, it is impossible to know how much memory will be consumed.
> * Therefore, it is easy to generate a large number of IO operations, resulting in excessive peak memory.

![Image from Xianyu Technology] (http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image9)

As shown in the figure above, it is the process related to picture caching, and the current constraint processing is through:

* There is no need to send extra pictures when the page is not visible
* Limit the number of cached pictures
* CG when appropriate

For more detailed content, you can read the article body. Why is this mentioned here? This is because the item `Limit the number of cached pictures`.

Remember the glue class "WidgetsFlutterBinding"? Among them, Mixins has `PaintingBinding`. As shown in the figure below, the binding that is "sticked" is responsible for image caching

![](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image10)

There is an `ImageCache` object in `PaintingBinding`, which is a singleton globally and used by `ImageProvider` when the image is loaded, so set the image cache size as follows:

```
//Number of caches 100
PaintingBinding.instance.imageCache.maximumSize=100;
//Cache size 50m
PaintingBinding.instance.imageCache.maximumSizeBytes = 50 << 20;
```

### 4. Thread

In [In-depth understanding of Flutter Platform Channel] (https://www.jianshu.com/p/39575a90e820) of Xianyu Technology, there are: ** There are four major threads in Flutter, Platform Task Runner, UI Task Runner, GPU Task Runner and IO Task Runner. **

Among them, `Platform Task Runner` is the main thread of Android and iOS, and `UI Task Runner` is the UI thread of Flutter.

As shown in the following figure, if you have done communication between Dart and the native side in Flutter, you should know that the two ends of the communication through the `Platform Channel` are `Platform Task Runner` and `UI Task Runner`, which are mainly summarized here:

* Because the Platform Task Runner is originally a native main thread, try not to perform time-consuming operations on the Platform side.

* Because Platform Channel is not thread-safe, when the message processing result is returned to the Flutter side, you need to ensure that the callback function is executed in the Platform Thread (that is, the main thread of Android and iOS).

![Picture from Alibiaba Technology] (http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image11)


### 5. Hot Update

*Inescapable demand. *

* 1. First of all, we know that Flutter is still an **iOS/Android** project.

* 2. Flutter generates and embeds **App.framework** and **Flutter.framework** into IOS by adding a shell (xcode_backend.sh) in BuildPhase.

* 3. Flutter refers to **flutter.jar** through Gradle and adds the compiled **binary file** to Android.

The compiled binary file of Android exists under `data/data/package name/app_flutter/flutter_assets/`. Those who have done Android should know that this path can be easily updated, so you know ￣ω￣=.

> **⚠️ Note that in versions after 1.7.8, Flutter under Android has been compiled into pure so files. **

IOS? As far as I understand, it seems that references such as the dynamic library framework cannot be updated with hot unless you do not need to review!


>Since then, the fifth chapter is finally over! (///▽///)


### Resource recommendation

* Github: [https://github.com/CarGuo/](https://github.com/CarGuo)
* **Open source Flutter complete project: https://github.com/CarGuo/GSYGithubAppFlutter**
* **Open source Flutter multi-case learning project: https://github.com/CarGuo/GSYFlutterDemo**
* **Open source Fluttre combat e-book project: https://github.com/CarGuo/GSYFlutterBook**

##### Complete open source project recommendation:

* [GSYGithubAppWeex](https://github.com/CarGuo/GSYGithubAppWeex)
* [GSYGithubApp React Native](https://github.com/CarGuo/GSYGithubApp)

![Will we see you again? ](http://img.cdn.guoshuyu.cn/20190604_Flutter-5/image12)
## Flutter Related Articles and Summary:


### Foreword
The content structure of this article is as follows, which is mainly divided into three parts: basic control, data module and other functions. In addition to the function implementation involved, the small modules in each large block will elaborate on the problems encountered by the author during the implementation process. The ultimate goal of this series is: to make you feel the pleasure of Flutter! Then let us start down happily!

### Basic Controls/Foundation related to Flutter:

The so-called foundation is probably the power of cutting wood!

1. Tabbar control implementation:

Tabbar pages are often required, and in Flutter: Scaffold + AppBar + Tabbar + TabbarView is the simplest implementation of Tabbar pages, but after adding AutomaticKeepAliveClientMixin to the page keepAlive, early problems such as #11895 began to become the culprit of Crash , Until the flutter v0.5.7 sdk version is fixed, the problem is still not completely resolved, so helplessly finally modified the implementation plan. (Fixed in 1.9.1 stable)

At present, the author uses Scaffold + Appbar + Tabbar + PageView to combine the effects to solve the above problems. Let's go directly to the code below, first as a Tabbar Widget, it must be a StatefulWidget, so we first implement its State:
            
As shown in the code above, this is the effect of a page with a bottom TabBar. TabBar and PageView through _pageController and _tabController to achieve Tab and page synchronization, through SingleTickerProviderStateMixin to achieve Tab animation switching effect (ps If you need multiple nested animation effects, you may need TickerProviderStateMixin), from the code we can see To:

When manually sliding the PageView left and right, call _tabController.animateTo(index); through the onPageChanged callback to synchronize the TabBar state.

In _tabItems, monitor the click of each TabBarItem, and synchronize the state of PageView through _pageController.

The above code is still missing the TabBarItem click, because this piece is put into the external implementation. Of course, you can also encapsulate the control directly inside, directly pass the configuration data display, this can be encapsulated according to personal needs.

The external calling code is as follows: when each Tabbar is clicked, jump to the page through pageController.jumpTo, each page needs to jump to the coordinates: the current screen size multiplied by the index index.
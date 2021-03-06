---
layout: post
title: How to Make Nibless iOS App
tags: [iOS, Objective-C]
share: true
---

##Intro

A “nibbles app” is an app that doesn’t use .xib files. As you probably know .xib files contain XML generated by Interface Builder and they are compiled to .nib when the application is built.

##Getting Started

First, open XCode (for this demo I use XCode 4.5.2). Create a new “Empty Application”.

![]({{ site.url }}/images/figures/xcode-empty-prj.png)

Then open `Supporting Files/main.m`.

![]({{ site.url }}/images/figures/xcode-prj-files.png)

The file should have the following content:

{% highlight objc %}
#import <UIKit/UIKit.h>
#import "AppDelegate.h"

int main(int argc, char *argv[])
{
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
{% endhighlight %}

In AppDelegate.m the method application:didFinishLaunchingWithOptions: should look like that:

{% highlight objc %}
@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // Init window
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
  
    // Customize the window of you want
    self.window.backgroundColor = [UIColor whiteColor];
  
    // Create you root view-controller
    MainViewController *root = [[MainViewController alloc] initWithNibName:nil bundle:nil];
  
    // Set *root as RootViewController
    [self.window setRootViewController:root];

    // Set as main window and display it in front of other windows
    [self.window makeKeyAndVisible];
    return YES;
}

@end
{% endhighlight %}

(Note: MainViewController is just a class derived from UIViewController)
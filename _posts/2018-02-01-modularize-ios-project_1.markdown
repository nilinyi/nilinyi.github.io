---
    author: Linyi Ni
    comments: true
    date: 2018-02-01 17:42:32+00:00
    layout: post
    title: "Tools to Modularize iOS Project (1): Target"
    categories:
    - Work
    - iOS
    tags:
    - iOS
    - Architecture
---

## Build module via Target

You might have known that `Targets` are usually used for different distributions of the same project. One example is that using `Target` to ship an iPad version out of your iOS project. Another example is the `Test Target` in your project if you perform Unit Test or UI Test. In short, `Targets` are the different managers of all the shared source code in that they determine which files to compile, how to compile and what things to ship out, etc.

Therefore, one way to modularize an iOS project is to ship different modules via `Targets` and let `Main Target` depend on these targets to build the whole project. Technically, we are just using Target to build **framework** that could be used as individual **module**.

## Setup the environment
The following setup is under a `workspace` built by CocoaPods, however, it really doesn't matter whether you are in a CocoaPods environment or not. You can still follow the same procedure because all of the steps are under the `Main Project/Target`.  

1. Tap `+` button  in `TARGETS` section.

    ![1.tiff](/assets/modularize-ios-project/1.jpg)

2. Choose `Cocoa Touch Framework` in the poped window.

    ![1.tiff](/assets/modularize-ios-project/2.jpg)

3. Make sure `Project` and `Embed in Application` both target at your `Main Project/Target`.

    ![1.tiff](/assets/modularize-ios-project/3.jpg)

4. You could see that a new folder is created and a new target is added in the `TARGETS` section.

    ![1.tiff](/assets/modularize-ios-project/4.jpg)

5. In your `Main Project/Target`, check if the `Target Dependencies` is set up correctly. If you created multiple targets and wanted to manage dependencies among them, here will be the right place for you. The `Target Dependencies` means that the targets listed here, a.k.a. child targets, will get compiled first until the parent target get compiled.

	![1.tiff](/assets/modularize-ios-project/5.jpg)

6. Now you can start writing code inside this target. For example, I created a class named `TMADirDemoViewController` and some private helper classes named `TMALittleSecret` and `TMAReallySecret`. (These are very very bad name, though. Don't use them in your project.)

	![1.tiff](/assets/modularize-ios-project/6.jpg)

7. If you want `TMADirDemoViewController` in this target to be exposed to the outside, here are what you should do:
    - Move `TMADirViewController.h` to `Public Headers` in `Build Phases`
    - Import `TMADirViewController.h` in header `TMADirTarget.h`

    ![1.tiff](/assets/modularize-ios-project/7.jpg)
    ![1.tiff](/assets/modularize-ios-project/8.jpg)

    You might have noticed that we could choose three levels of headers, `Public`,  `Private` and `Project`. In fact, they could control what classes will be exposed outside the target.

    - `Public` means that these interfaces are **exposed** for clients to use and will not be changed frequently.
    - `Private` **still means these interfaces are exposed**, however, it means for clients to JUST see them rather than use them because these interfaces might be under development that will be changed frequently or for some other whatever reasons.
    - `Project` means they will **not be exposed**. These interfaces could only be used inside this target.

## How to use this module in other modules?
Looks familiar, right?
```objc
#import <TMADirTarget/TMADirTarget.h>
```
![1.tiff](/assets/modularize-ios-project/9.jpg)

## How to compile and test it separately?
For compile, just change the `Scheme` and then compile. Most of the time, you don't need to do that because when you compile `Main Project/Target`, all its depending targets will be compiled before it does.  
For test, however, you will need another target to test this target. Just the way that you test your `Main Project/Target`. But an easier way is that you could select `Unit Test` when you created the target.

![1.tiff](/assets/modularize-ios-project/10.jpg)

## Working with CocoaPods
If you want this new target to use some Cocoapods libraries, you can just set it in the `Podfile`. 

![1.tiff](/assets/modularize-ios-project/11.jpg)

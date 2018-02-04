<!-- ---
    author: Linyi Ni
    comments: true
    date: 2017-12-25 10:12:26+00:00
    layout: post
    title: Understand CocoaPods
    categories:
    - Work
    - iOS
    tags:
    - iOS
    - CocoaPods
---

# Understand Cocoapods

When project becomes bigger, it’s inevitable for the team to build different modules so that code could be reused and engineers could work concurrently on different modules. In iOS particularly, we use static library, or framework(Cocoa Touch Library) to aggregate codes into a module. However, integration is always a headache. CocoaPods is born to fix this problem. This article is talking about what is CocoaPods, why we should use CocoaPods and how to create CocoaPods.

### What is CocoaPods?
I think introduction is somewhat redundant because nearly every iOS engineer knows and used this before. In short, CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It can help you scale your projects elegantly.

### Why should we use CocoaPods?
Let’s take a step back.

##### Why people don’t like static library?
In short, it’s not user friendly. Let’s recap how to integrate a static library into your Xcode project.
1. Drag it into your project (Copied into Group if necessary)
2. Add headers of this static library (In the Target page of this static library)

  You think that’s the end? No!
  Almost every iOS engineer will come into this well-known `linker flag` bug. That’s, you will get crash like this
  ```objc
  - [SomeStaticLibrary foo]: unrecognized selector sent to instance xxx
  ```
  The reason for this bug is that Objective-C needs to know which object files (*.o) needed to be linked when it uses static library. By doing it, Objective-C checks `__.SYMDEF SORTED` (a.k.a. [standard archive header]), which is generated automatically, inside the .a file. However, this header does NOT contain all object files! It only contains the object files that define the class. If you have a category inside the static library, the object file of this category will not be linked. Therefore, to force it link category object file, you need to do this,

3. Add `-Objc` for `other linker flag` in `Build Settings`

  You thinks that’s the end? Still No! What if there aren't even class in the static library? Like only category files in the library?

4. Add `-force_load` for `other linker flag` in `Build Settings`

  It should be the end... However, if there any dynamic libraries referenced in the static library, you need to import them into Xcode too! Because static library does not support recursive reference!

5. Add any `dynamic libraries(.dylib or .framework)` that this static library referenced

So ugly, right? That's why Apple introduced `.framework(Cocoa Touch Framework)` to us. You could treat `.framework` as `.a + .h + some_dependency_settings`. One thing worth to know is that `.framework` could be either static or dynamic. However, only system framework is dynamic, the library we built are just static.

However, managing dependency is another headache.

##### Why should we use CocoaPods?
Simple.
For users, just specify what Pod(library) you want to use in `Podfile`. Forget about any dependency management.
For library developers, `podspec` is the only place you will need to manage. I will cover this in the next article.

### How CocoaPods works?
Todo

##### How to manage dependency?
It uses `Xcode Workspace` where `Pods.xcodeproj` is a subproject which includes all the libraries that each of them wrapped in a single `Target` and all the dependency settings leveraged by `Target`. (Thanks to Xcode, it's really easy to set dependency for each targets.) At the same time, in your project, `libPod.a` will be set as the linked static library, which is generated from `Pods.xcodeproj`. Therefore, every time you compile your code, it's also guaranteed that the compile is in the right order---`Pods.xcodeproj` will first get compiled based on the dependency order it sets inside and a static library `libPod.a` will be generated afterwards to participate your project's compile.

##### What if dependency conflict happens?
Todo

##### Where do these Pods(libraries) get fetched?
Todo



[standard archive header]: http://www.manpagez.com/man/5/ranlib/ -->

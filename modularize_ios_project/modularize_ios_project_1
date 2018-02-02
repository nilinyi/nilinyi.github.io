# Tools to Modularize iOS Project (1)

## Build module using `Target`

`Targets` are usually used for different distributions of the same project. For example, if you want to ship a iPad version, using `Target` could help you a lot. You might also see the `Test Target` in your project if you do Unit Test or UI Test. Anyway, in my perspective, `Targets` are the different managers of all the shared source code in that they determine which files to compile, how to compile and what thing to ship out, etc.

Therefore, to modularize our project, we could ship different modules via `Targets` and let `Main Target` depend on those targets to build up the whole project.


1.	Tap `+` button  in `Targets`
![1.tiff](./Assets/1.tiff)

	0.	Choose `Cocoa Touch Framework` in the poped window.


	0.	Make sure `Project` and `Embed in Application` is correctly configured.

	0.	You could see that a new folder is created and a new target is added in the `TARGETS` section.

	0.	In your main target, check the `Target Dependencies` is set up correctly. If you created multiple targets and wanted to manage dependencies among them, this will be the right place for you.

	0.	Now you start writing code inside this target. For example, you created a class named `TMADirDemoViewController` and some private helper classes named `TMALittleSecret` and `TMAReallySecret`.

And you only want `TMADirDemoViewController` to be exposed to the clients (public). Here is what you should do:
	⁃	Move `TMADirViewController.h` to `Public Headers` in `Build Phases`
	⁃	Import `TMADirViewController.h` in header `TMADirTarget.h`


You might notice that actually we could choose three levels of headers, `Public`,  `Private` and `Project`.
	⁃	`Public` means that these interfaces are exposed for clients to use and will not change frequently
	⁃	`Private` still means that these interfaces are exposed, however, they are either under development that will be changed frequently, or they are not supposed to used by clients at all (That’s why it’s called private).
	⁃	`Project` means they will not be exposed. These interfaces could only be used inside this library(target).

7.  How client uses this?
Import this library as you did for other libraries
```objc
#import <TMADirTarget/TMADirTarget.h>
```


	0.	How to compile and test it separately? For compile, just change the `Scheme` and then compile. For test, however, you will need another target to test this target. But an easier way is that you could select `Unit Test` when you created the target.


	0.	If you want this new target to use some Cocoapods libraries, you can set it in the `Podfile`.

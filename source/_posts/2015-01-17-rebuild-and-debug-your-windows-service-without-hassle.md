---
layout: post
title: "Rebuild and debug your Windows Services without hassle"
description: ""
category: Build
tags: [build, debug]
---
Creating a windows service isn't that complicated as there is a Visual Studio template and a [documentation] (http://msdn.microsoft.com/de-de/library/zt39148a%28v=vs.110%29.aspx) available. If you want to start and debug the service then you have to install and start the service first. On a rebuild you have to stop the service everytime as the files are in use. To get rid of this annoying task you can use the pre-build and post-build events. You can add those events in your project `properties` -> `Build events`.
 
In the pre-build event you just stop the service. In case of errors you just don't care about it e.g. if the service already stopped.
 
    net stop NameOfMyService
    Exit /b 0
 
In the post-build event you have to do a little bit more than just starting the service. On the first run you have to register the service first. But you cannot use the target/output directory directly. Visual Studio cleans the build folder on a rebuild and if you want to stop the service your service files cannot be found. You have to copy the files to a different location first. Then you can register the service with the installutil.exe. Place the installutil.exe within your repository. So your collegues can start right away. The last step is to start the service. Again don't care about errors as the registration is just required once.
 
    "%systemroot%\System32\xcopy" "$(TargetDir)*.*" "$(ProjectDir)..\..\..\Build" /Y /E
    "$(ProjectDir)..\..\..\Tools\InstallUtil\installutil.exe"  "$(ProjectDir)..\..\..\Build\$(TargetFileName)"
    net start NameOfMyService
    Exit /b 0
 
Everyone can checkout your repository now and rebuild the solution without hassle.
Notice that I said rebuild and not start. You cannot use F5 in Visual Studio to start and debug the service. You have to attach the debugger to your service manually. The rebuild will just start the post-build event to register and start the service. Then open `Debug` -> `Attach to process…` and check `Show processes from all users`. Select your service process and `Attach`. Now you can set your breakpoints as required.

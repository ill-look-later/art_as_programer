# 启动过程描述

xcode 开发ios程序帮我们隐藏了太多的东西， 包括系统的初始化，资源的加载，程序的入口等等都帮我们在背后完成了， 如果我们创建single view application， 暴露给我们的就是app的一个delegate 和一个 viewcontroler的入口，

在appdelegate中 我们会得到
- didFinishLaunchingWithOptions
- applicationWillResignActive //由active 状态变成 inactive 时触发
- applicationDidEnterBackground
- applicationWillEnterForeground
- applicationDidBecomeActive
- applicationWillTerminate
- ..... 等等这一系列的消息
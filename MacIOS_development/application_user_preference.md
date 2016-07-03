# Application User Preference

IOS应用程序中, 在系统设置页面里面包含了每一个安装了的应用的配置选项, 程序中, 我们只需要将一些设置写入用户首选项的配置里, 这些信息就会和我们应用一直存在;

在Foundation基础框架中, 我们通过**NSUserDefaults** 对象来设置和访问我们的应用的首选项数据; **NSUserDefaults**类维护了一系列的方法用来访问和设置数据;下面是我从类中截取的一些方法

```swift
    /*!
     +standardUserDefaults returns a global instance of NSUserDefaults configured to search the current application's search list.
     */
    public class func standardUserDefaults() -> NSUserDefaults
    
    /// +resetStandardUserDefaults releases the standardUserDefaults and sets it to nil. A new standardUserDefaults will be created the next time it's accessed. The only visible effect this has is that all KVO observers of the previous standardUserDefaults will no longer be observing it.
    public class func resetStandardUserDefaults()

```
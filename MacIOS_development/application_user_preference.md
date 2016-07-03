# Application User Preference

IOS应用程序中, 在系统设置页面里面包含了每一个安装了的应用的配置选项, 程序中, 我们只需要将一些设置写入用户首选项的配置里, 这些信息就会和我们应用一直存在;

在cocoa touch的框架中, 我们通过**NSUserDefaults** 对象来设置和访问我们的应用的首选项数据;
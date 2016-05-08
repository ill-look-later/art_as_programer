# UI的跳转和页面之间的数据传递



页面跳转其实就两种方式啦， 新的和旧的，当然了， 原理都是一样一样的嘛
先来看看效果：
            ![](页面跳转和数据传递.gif)

ios中得页面衔接有好几种， 之前我们用过通过push的方法，将子view 放到一个view stack中， ios默认会在界面上边存在一个返回箭头的；
- 使用导航栏压进新的控制器（push），
- 模态的加载视图控制器（modal），
- 自定义（custom） 没用过。

好了，到了这里，简单说一下storyboard下，利用segue界面跳转一共有两种方式：
第一种就是以上我的例子，利用button组件，拖拽添加segue，直接运行就可以用。 【手势滑到切换】
第二种是利用ViewController与ViewController之间，拖拽添加segue。不过，这种方法就需要在相应需要跳转的方法内写入代码，手动去设置它的跳转。


这里我们主要说得是设计方式上，一个是老的xib，一个是新的storyboard 其实都是一模一样的啦；
先看两个函数的说明
```swift
/*
      The next two methods are replacements for presentModalViewController:animated and
      dismissModalViewControllerAnimated: The completion handler, if provided, will be invoked after the presented
      controllers viewDidAppear: callback is invoked.
    */
    @available(iOS 5.0, *)
    public func presentViewController(viewControllerToPresent: UIViewController, animated flag: Bool, completion: (() -> Void)?)
    // The completion handler, if provided, will be invoked after the dismissed controller's viewDidDisappear: callback is invoked.
    @available(iOS 5.0, *)
    public func dismissViewControllerAnimated(flag: Bool, completion: (() -> Void)?)
```
##主要的步骤：

### 在storyboard中设置跳转的方法一：
1. 新建singleview 工程
2. 在mainstoryboard中默认额view中拖入一个button
3. 再拖入一个新的uiviewcontroller， 并在一个新的ui界面放入一个label来表示这是第二个页面
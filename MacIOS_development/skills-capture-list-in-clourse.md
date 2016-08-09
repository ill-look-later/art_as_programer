# 闭包中的捕获列表

官方文档中已经有说明“无论您将函数或闭包赋值给一个常量还是变量，您实际上都是将常量或变量的值设置为对应函数或闭包的引用”； 是个引用类型， 那么按照swift的ARC机制， 就会在产生在一个闭包中比较隐晦的捕获了外面的对象，通常都是类对象；比如`this`对象；

##### 注意:

> Swift 有如下要求：只要在闭包内使用`self`的成员，就要用`self.someProperty`或者`self.someMethod`\(而不只是`someProperty`或`someMethod`\)。这提醒你可能会不小心就捕获了`self`。

要在闭包中解决循环引用的问题，官方给出的方法就是在闭包中使用捕获列表，指明是`unowner`或者`weak`;从而来解决闭包内对外部的对象产生强引用关系；捕获列表中的每个元素都是由`weak`或者`unowned`关键字和实例的引用\(如`self`或`someInstance`\)成对组成。每一对都在方括号中，通过逗号分开。如果闭包没有参数；在捕获列表和`closure block`之间使用关键字`in` 分割开来；

```swift
@lazy var someClosure: () -> String = {
    [unowned self] in
    // closure body goes here
}
```

如果是存在闭包参数，那么就变成了下面这个样子了：

```swift
doSomeThingAsync(request: req, handle: {
  [unowner self, weak ohterInstance] (arg1, arg2...) in 

  //closure block, your code here
  ...
});
```

其中有几点在开发中依旧需要注意；

* 当闭包和捕获的实例总是互相引用时并且总是同时销毁时，将闭包内的捕获定义为`无主引用`。
* 当捕获引用有时可能会是`nil`时，将闭包内的捕获定义为`弱引用(weak)`。弱引用总是可选类型，并且当引用的实例被销毁后，弱引用的值会自动置为`nil`。这使我们可以在闭包内检查它们是否存在。
  > 如果捕获的引用绝对不会置为nil，应该用无主引用，而不是弱引用。


![引用关系](./img/closureReference2x.png)


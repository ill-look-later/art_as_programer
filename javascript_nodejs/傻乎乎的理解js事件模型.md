# 傻乎乎的理解事件模型

基本上一看博客，技术书籍，上面讲到 **JavaScript**是以单线程的方式运行的， 而从cpu的角度， 线程是cpu的执行代码的最小单元；按照我们这种c++ coder来说，那么如果我一个定义了一个函数或者说任务执行花了很多很多时间， 你后边的任务哪怕再着急，也不会被执行到的；

看下面这个代码：

```javascript
function sleep(ms) {
    ms += new Date().getTime();
    while(new Date() < ms){
        ;
    }
}
```


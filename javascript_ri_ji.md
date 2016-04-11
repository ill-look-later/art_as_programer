# javascript 日记


- 如果一个函数被定义为用于创建对象的**构造函数**， 使用时一定要用new， 
> 在strict模式下，this.name = name将报错，因为this绑定为undefined，在非strict模式下，this.name = name不报错，因为this绑定为window，于是无意间创建了全局变量name，并且返回undefined，这个结果更糟糕。
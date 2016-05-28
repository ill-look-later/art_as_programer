# express.use 的作用



在express的官方手册上写着是：

> app.use([path], function)
Use the given middleware function, with optional mount path, defaulting to "/".

翻译过来就是 对 .... 使用 ....

填写完整就是: 对 [path] 这个路径的请求, 使用 [function] 这个中间件

但是不是很理解意思。这里的function 是在什么场合调用的？
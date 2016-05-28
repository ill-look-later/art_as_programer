# express.use 的作用



在express的官方手册上写着是：

> app.use([path], function)
Use the given middleware function, with optional mount path, defaulting to "/".

翻译过来就是 对 .... 使用 ....

填写完整就是: 对 [path] 这个路径的请求, 使用 [function] 这个中间件, 说更直白点, 就是当用户在浏览器发起一个[path]路径的请求时, 调用[function这个处理函数]';

但是不是很理解意思。这里的function 是在什么场合调用的？
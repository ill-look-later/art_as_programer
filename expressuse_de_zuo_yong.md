# express.use 的作用



在express的官方手册上写着是：

> app.use([path], function)
Use the given middleware function, with optional mount path, defaulting to "/".

翻译过来就是 对 .... 使用 ....

填写完整就是: 对 [path] 这个路径的请求, 使用 [function] 这个中间件, 说更直白点, 就是当用户在浏览器发起一个[path]路径的请求时, 调用[function这个处理函数]; 所以我们可以想象的到得处理函数一个是这样的
```javascript
function(req,res){//...}
```

但是, 你会发现,实际上函数是长成下面这样子的
```javascript
function(req,res, next){//...}
```

这个next，是指下一个函数。 可以这么认为，在express内部，有一个函数的数组，暂时叫这个数组tasks，每来一个请求express内部会依次执行这个数组中的函数（这里说依次并不严谨，每个函数必须满足一定条件才行，这个后面说）


后面我们会写一些试验来体验一下这个next，先总结一下：

但是不是很理解意思。这里的function 是在什么场合调用的？
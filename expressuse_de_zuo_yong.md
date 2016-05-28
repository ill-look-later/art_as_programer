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

这个next，是指下一个函数。 可以这么认为，在express内部，有一个函数的数组[队列]，暂时叫这个tasks队列，每来一个请求express内部会依次执行这个数组中的函数（这里说依次并不严谨，每个函数必须满足一定条件才行，这个后面说）


大致是下面这个样子的

- 1.导入相关模块
- 2.执行过 var app = express() 后，
- 使用app.set 设置express内部的一些参数（options）
- 使用app.use 来注册函数，可以简单的认为是向那个tasks的数组进行push操作

```javascript
app.use(function(req,res,next){
    console.log('pos 00000001');
    next();
    console.log('pos 00000002');
});

app.use(function(req,res,next){
    console.log("pos 0000003");
    next();
});
```

也就是说, 如果我们在浏览器中发送一个'HTTP://hostname:port/' 这样一个'/' 路径的请求之后, 我们会看到下面这样的输出:
- pos 00000001
- pos 00000003
- pos 00000002


至于说这里这种use的实现. 其实也不难, {ps: 其实对于我这种弱智来说还是挺难的. 不过看到了之后就不难了../wx}, 下面是网上别人写的一个demo实现, 就能很容易实现use的这种逻辑

```javascript
var http = require('http');
function express(){
    var funcs = [];

    var expr = function(req,res){
        var i = 0;
        function next(){            
            var task = funcs[i++];
            if(!task) return;
            task(req,res,next);
        }
        next();
    }
    expr.use=function(f){
        funcs.push(f);
    }
    return expr;
}
var app = express();

app.use(function(req,res,next){
    console.log('haha');
    next();
});
app.use(function(req,res,next){
    console.log('hehe');
    next();
});
app.use(function(req,res){
    res.end("there is nothing happened");
});

http.createServer(app).listen('3000', function(){
  console.log('Express server listening on port 3000');
});
```

看完之后是不是瞬间就感觉很easy呢? 呵呵


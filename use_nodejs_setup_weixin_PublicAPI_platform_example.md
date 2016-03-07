# 用Nodejs搭建weixin公众平台入门例子



http://my.oschina.net/u/2275217/blog/630770

- MTM内部维护的状态和UI的View对应不上在某些状态下
  
  例如： 通过APM[或者client端]创建两个tab， close第二个tab， 这时候ui端

我们目前代码上ui上的页面触发的条件是设置webcontent到webview当中去的时候， 如果一个页面




基本原理

　　用nodejs怎样来实现对微信公众平台的开发呢？

　　别的就不多说了，先来简单介绍微信公众平台的基本原理。

　　微信服务器就相当于一个转发服务器，终端（手机、Pad等）发起请求至微信服务器，微信服务器，然后将请求转发给自定义服务（这里就是我们的具体实现）。服务处理完毕，然后转发给微信服务器，微信服务器再将具体响应回复到终端；通信协议为：HTTP；数据格式为：XML。 　　具体的流程如下图所示：

hubwiz

　　其实，我们需要做的事情，就是对HTTP请求，做出响应。具体的请求内容，我们按照特定的XML格式去解析，处理完毕后，也要按照特定的XML格式返回。

平台注册

　　要想完成对微信公众平台的开发，我们需要注册一个微信公众平台帐号。注册步骤如下： 　　打开微信公共平台的官网，https://mp.weixin.qq.com/，点击“立即注册”。

　　然后根据提示，填写基本信息，邮箱激活，选择类型，信息登记，公众号信息，完成注册。

　　在注册完成以后，我们要对公众号进行一些基本的设置。登录公众号，找到【公众号设置】，然后设置头像以及其它信息。

nodejs环境搭建

　　我们需要在公网上找一台服务器，以便可以启动我们的nodejs的环境，启动环境后通过设置访问地址，我们就可以接收微信服务器发送的消息了，并且我们也可以向微信服务器发送消息了。

　　在公网的服务器中安装完成nodejs以后，我们还需要安装一些nodejs所用到的模块，如：express，node-xml，jssha等模块。可以通过npm命令进行安装。

　　我们通过nodejs来实现向微信服务器消息的发送与接收，以及与微信服务器的签名认证。

　　在我们右面的编辑环境中已经为同学们安装了nodejs环境。我们在接下来内容中就为同学们来实现微信服务器的签名认证。

创建express框架

　　我们在前面的课程中已经安装了express模块，并且在我们右面的环境中已经创建了一个名为app.js的文件。现在我们就在这个文件中完成express框架。如下代码：

var express = require("express");
var path=require('path');
var app = express();
server  = require('http').Server(app);
app.set('views',__dirname);    // 设置视图 
app.set('view engine', 'html'); 
app.engine( '.html', require( 'ejs' ).__express );
require('./index')(app);      //路由配置文件
server.listen(80,function(){
console.log('App start,port 80.');
});
然后再添加一个名为test.html的文件。写入以下内容

<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>汇智网</title>
</head>
<body>
<div><%=issuccess%></div>
</body>
</html>
　　我们还要添加一个名为index.js的文件，来实现我们的路由。点击编辑环境中的添加文件按钮，添加文件，然后我们写入以下代码，其中GET请求用来验证配置的URL合法性，POST请求用来处理微信消息。

module.exports = function(app){
app.get('/',function(req,res){
res.render('test',{issuccess:"success"})
});
app.get('/interface',function(req,res){});
app.post('/interface',function(req,res){});
}
这样我们需要的express框架就完成了，当然我们还可以添加public公共文件夹以及我们要用到的中间件。保存文件，点击【提交运行】，然后点击【访问测试】，去试试吧。记下访问测试的地址，我们将在下一节中会用到该地址。

微信服务器配置

　　我们登录微信公众平台，在开发者模式下面找到基本配置，然后修改服务器配置。如图所示：

hubwiz

　　首先URL要填写公网上我们安装nodejs接收与发送数据的路径。我们可以填写上节中【访问测试】的地址，然后加上对应的路由就可以了。

http://724515db515222a9efffd6b092aa955d.me.hubwiz.com/interface
上面代码是我的访问测试的地址，然后加上前面课程中的路由，同学们要根据自己的访问测试地址与路由来填写。

　　Token要与我们自定义服务器端的token一致。填写完成以后，就可以点击提交了，在提交以前，我们启动app.js（点击【提交运行】）。这样根据我们的路由匹配就可以验证签名是否有效了。

　　当配置完成以后，一定要启用配置。

hubwiz

网址接入

　　公众平台用户提交信息后，微信服务器将发送GET请求到填写的URL上，并且带上四个参数：

  参数                     描述
  signature               微信加密签名
  timestamp               时间戳
  nonce                   随机数
  echostr               随机字符串
　　开发者通过检验signature对请求进行校验（下面有校验方式）。若确认此次GET请求来自微信服务器，请原样返回echostr参数内容，则接入生效，否则接入失败。

　　signature结合了开发者填写的token参数和请求中的timestamp参数、nonce参数。

　　加密/校验流程：

将token、timestamp、nonce三个参数进行字典序排序；
将三个参数字符串拼接成一个字符串进行sha1加密；
开发者获得加密后的字符串可与signature对比，标识该请求来源于微信。
参数排序

　　首先我们确认请求是来自微信服务器的get请求，那么就可以在index.js文件中进行添加代码了。然后在app.get('/interface',function(req,res){});的function中进行添加。

　　先来获取各个参数的值，如下代码：

var token="weixin";
var signature = req.query.signature;
var timestamp = req.query.timestamp;
var echostr   = req.query.echostr;
var nonce     = req.query.nonce;
我们在这里对token进行设置，让其与微信服务器中设置的token一致。

　　然后对其中的token、timestamp、nonce进行排序，如下代码：

var oriArray = new Array();
oriArray[0] = nonce;
oriArray[1] = timestamp;
oriArray[2] = token;
oriArray.sort();
这样我们就完成了排序。

参数加密

　　在上节中我们已经对参数进行了排序，然后我们在这一节中要将参数组成一个字符串，进行SH-1加密。在加密以前要用到jssha模块，在我们的文件中要引用该模块。

var jsSHA = require('jssha');
在上一节课中我们已经对参数排序完成，并存放在数组中，我们可以通过join方法来生成一个字符串，如下代码：

var original = oriArray.join('');
最后对该数据进行加密，如下代码：

var jsSHA = require('jssha');
var shaObj = new jsSHA(original, 'TEXT');
var scyptoString=shaObj.getHash('SHA-1', 'HEX'); 
好了这样就生成了我们需要的签名字符串scyptoString。

签名对比

　　我们已经得到了我们想要的签名字符串scyptoString，然后我们就可以与来自微信服务器的签名进行对比了,对比通过，则我们就可以接收与发送消息了。

 if(signature == scyptoString){
 //验证成功
 } else {
 //验证失败
 }
本参考了如下网站，更多内容也请访问： http://www.hubwiz.com/course/569dc7fdacf9a45a69b051cd/
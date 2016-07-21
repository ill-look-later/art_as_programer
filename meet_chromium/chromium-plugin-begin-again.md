# Plugin Begin Again

在begin again之前，先搞清楚两个概念** chrome extensions** & **web Plugin**

## chrome extensions

official infomation：[extensions](https://developer.chrome.com/extensions)
>  Extensions are small software programs that can modify and enhance the functionality of the Chrome browser. You write them using web technologies such as HTML, JavaScript, and CSS. 

也就是说， 扩展在chrome上通常是用htm、javascript、css编写的用来修改或者增强浏览器功能的一个应用；比如：一个字典扩展可以通过访问clip Board来里面复制的内容来翻译成不同的语言，让后通过一个小的浮动页面（类似气泡弹窗）来显示结果;

Plugins
---
说道这个词， 大家可能最容易想到的可能就是flash插件，其实插件就是开发者遵循一定规范的应用程序接口编写出来的程序；而且这个程序是用来扩展页面功能用的；比如说我要支持一个新的音频格式xz，开发一个插件是用来支持“audio/xz”这种content type的plugin、 那么当浏览器在解析html的时候遇到这种类型的内容时，发现默认的浏览器中无法正确的处理渲染这个标签，如果这时候有支持这种type的plugin，那么就会使用相应的插件来支持播放这种xz格式的音频流；
最早plugin是由浏览器的元祖Netscape定义的一个叫做** Netscape Plugin Application Programming Interface (NPAPI) **这样的一个接口规范，默默的被大多数后来的浏览器支持和使用， 但是随着现代化浏览器的发展，它已经显现出明显的不足了，所以各个Browser厂商各种又开发出自己的规范；MS的ActiveX， chrome的PPAPI都算是这个范畴了， 而今天讲的主角PPAPI
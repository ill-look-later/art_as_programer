# Web IDL on chromium


下面两个是chromium项目中关于对webidl（web interface definition language）一些说明
- [webidl](https://www.chromium.org/blink/webidl)
- [web-idl-interfaces](https://www.chromium.org/developers/web-idl-interfaces)

** 在idl中描述的属性和方法一定要按照规则编写，不然的话很容易出错**
正常情况下binding成功之后， 会在/out/Release/gen/blink/bindings/core/v8/V8[IDL名称].cpp[h] 生成相应binding的C/C++的实现。

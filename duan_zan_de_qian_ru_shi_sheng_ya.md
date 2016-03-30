# 短暂的嵌入式生涯

Hi 何工， 郭工，

---
> case 1: 你昨天提供给我们的migaole_example_v2我这边交叉编译出来运行结果直接就返回失败了

- log：
```shell
/tmp/nfs202/gongh # ./imev2.bin
creat msq err
open msg err
```
> case 2：而运行image固件中/Customer/main这个demo程序可以看到和imgage说明中的log
- log:
```
    /Customer # ./main
    now have a test
    open msg ok
```
@何工，

    - case 1 中的代码你们在你们那边的image上是否有测试过， 具体是一个什么现象，如果测试过，能否提供一个测试的log；  
      
    - case 2 中image自带的这个case的代码是否可以提供源码供我们这边进行参考；
    
    - 不知你们那边对migaole 输入法engine是否有过成功调试的测试例子。如果有，是否可以提供参考，以便我们尽快推进集成到browsesr当中。

@郭工，
  
你好， 除了提供的 “输入法SERVICE介绍.xls" 中调用流程和cmd的介绍之外， 是否还有更详细的说明；

> 你们那边是否有简单的集成案例（ps: 包括数据解析，使用等等...）,如果有的话，能否在这方面提供一下代码供我们这边参考。


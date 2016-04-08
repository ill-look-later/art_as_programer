# test

####Hi 何工，
  
  我们这边在集成的过程中，取出来的数据有些问题；给我们的demo代码本身就也有这个问题
> case 1例子代码的log：
> 从log上看，取出来的数据，从rownumber=243开始数据就出错了；

```ssh
client rev a msg
keyboard total num =30
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=243
totalrow 4,rownumber=-2029912064
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
totalrow 4,rownumber=0
```



> case 2集成到我们的代码中后的log： 可以看到从rownumber=243之后数据就不正确了， UTF8 和 UTF16结果都是这样的

```
keyboard total num =30
totalrow 4,rownumber=0

The keyboard keychar is:[q]
totalrow 4,rownumber=0

The keyboard keychar is:[w]
totalrow 4,rownumber=0

The keyboard keychar is:[e]
totalrow 4,rownumber=0

The keyboard keychar is:[r]
totalrow 4,rownumber=243

The keyboard keychar is:[]
```
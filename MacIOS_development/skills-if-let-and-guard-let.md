# swift 中if let 与 guard let的技巧

swift的中optional 变量的判断和解析虽然带来了便利， 但是同样也带了了不少麻烦的地方， 假设有个json文件是夏眠这样的

```json
{
  "product": {
    "subclass": {
      "subclass2": {
        "subclass3": {
          "key": value,
        }
      }
    }
  }
}
```

当解析的时可能由于技巧不足，就可能写成下面这个样子

```swift
if let jsonDic = json as? NSDictionary {
  if let product = jsonDic["product"] as? String {
    if let subclass = product["subclass"] as? ... {
      if let .... {
        ...
        if let key = ... {
          print("value:\(key)")
        }
      }
    }  
  }
}
```

其实和很多其它语言一样， 解析规则是满足前后顺序的，就像｀c语言中if｀ if(a && a->isbool && a->f()) 一样当a == NULL 时，后面的条件就不会去访问；所以在swift中我们可以写成下面这两种情况来使代码看起来更优雅一点；👀下面；


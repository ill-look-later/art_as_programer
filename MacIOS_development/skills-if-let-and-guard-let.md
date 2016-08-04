# swift 中if let 与 guard let的技巧

swift的中optional 变量的判断和解析虽然带来了便利， 但是同样也带了了不少麻烦的地方， 假设有个json文件是夏眠这样的

```json
{
  "product": {
    "subclass": {
      "subclass2": {
        "subclass3": {
          "key": value;
        }
      }
    }
  }
}
```
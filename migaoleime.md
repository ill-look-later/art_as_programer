# migaole_ime

js端需要实现的接口：
```js
/*这个函数在初始化完ime时会被调用， 请在函数内根据layout信息
  完成键盘ui的布局
  arg in： arraylist是一个数组；
  例子： onUpdateKeyboardLayout(["q","w","\r","x"]);
  */
srafwebchannel.onUpdateKeyboardLayout(arraylist);


/*这个函数在用户点击一个“字符按键”发送到ime时触发，参数是你现有输入的内容，
  arg in： str是一个字符串；
  栗子： 中文输入法下， 连续点击完'nihao' 但没有选择候选词的时候，这个函数会返回给你'nihao',如过继续输入'm', 函数参数会变成'nihaom';
  例子： onUpdateKeyboardLayout("nihao");
  */
srafwebchannel.onUpdateInputedData(str);



```


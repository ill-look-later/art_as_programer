# migaole_ime

js端需要实现的接口：
```javascript
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


/*这个函数在用户点击一个功能键"shift"后触发，参数是键盘每个按钮上的字母列表，通常长度是和先前获取到键盘layout的按键数是一样的。
  arg in： arraylist；
  栗子： EN输入法下， 按了功能键'shift'后触发；这个函数会返回给你['Q' 'W',"E".....],
  例子： onUpdateKeyboardLayout(["Q","W","E","R","T".....]);
  */
srafwebchannel.onUpdateKeyBtCharacterValue(arraylist);

/*这个函数在用户点击一个功能键"enter"后触发，参数提交到输入框的内容
  arg in： arraylist；
  栗子： 中文输入法下， 连续点击完'nihao' 但没有选择候选词的时候 按下回车【enter】后触发，这个函数会返回给你'nihao'，候选词这时候应该放弃掉
  例子： onUpdateKeyboardLayout(["Q","W","E","R","T".....]);
  */
srafwebchannel.onUpdateTargetData(str)

/*这个函数在有智能联想提示的语言中比如说中文【英语状态下这种没有候选词，测试看来这个函数从不触发】，参数是一个候选词的列表， 这个列表最长50个；
  arg in： candidateList；
  栗子： 中文输入法下， 连续点击完'nh'， 这个函数会返回给你
  ["你好","你会","弄花","女孩","您好","你还","男孩","内核"......]，
  例子： onUpdateKeyboardLayout(["Q","W","E","R","T".....]);
  */
srafwebchannel.onUpdateCandidateList(candidateList);

/*  但与ime交互的过程中发生严重错误的时候触发， 参数是错误信息，str */
srafwebchannel.onImeCriticalError(str)

```




# Node.js

node 碎碎念...


现在srafwebchannel提供了五个接口给js keyboard使用，五个接口全部是js主动调用请求ime服务的接口；

请求ime提供连接服务的接口
- boolean connectImeService(); 

主动请求和ime断开连接，不调用也能没有关系；
- void requestCloseIme()；

请求ime将输入的部分数据清空；
- void clearImeCache();

发送用户的按键给ime
- void sendCodeToIme(int id);

请求ime设置新的语言 暂时这部分语言，无论你传什么字符串都是设置成英语，Migaole那边设置语言之后有问题，暂时无法调用
- void setImeLanguage(Domstring language)


### 整个输入法工作流程 -针对 srafbrowser来说的流程
  用户发生点击或者call键盘的动作 --> 触发js层的显示输入法的接口 --> js层发起ime的连接请求（connectImeService） -->  C层面接到这个请求后会和migaoleIme建立好连接。做好相关的设置后会拿到一个默认的layout. 并通过接口调用js层面的函数srafwebchannel.onUpdateKeyboardLayout(arraylist); --> js层在onUpdateKeyboardLayout(arraylist)函数中根据布局的参数arraylist来完成布局，并显示键盘 --> 等待用户输入....... -->  用户按下按键 -> 如果是IMElayout中的键，通过调用sendCodeToIme(id) 将按键发给ime， 如果是非imeLayout中的功能键『切换语言的键， 或者其他功能键』js层面自己处理完成相关的功能。 --> c层面响应sendCodeToIme(id) 请求migaole IMe给出结果 --> （migaole根据按键id给出不同的反馈【eg： 更新键盘的现实切换字符，更新候选词， 更新目标输入等等】）通过C层面执行调用函数让js执行相对应的函数，--> js层在这些响应函数中根据参数信息完成ui显示的更新展示。
  
  
  MIGAOLE具体完整使用流程请参考附件中的表格中【流程图】和 【工作流程】这两个表





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


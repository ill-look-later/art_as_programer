# The startup progress of  Any APP based on Chromium


main(int argc, const char**argv)
    //由argv 构造一个content::ContentMainParams 类型的param 对象；传递给ContentMain()
    content::ContentMain(params);
    
content/app/content_main.cc
```Cpp
ContentMain(const ContentMainParams& params) {
  // 构造一个ContentMainRunner的对象
  scoped_ptr<ContentMainRunner> main_runner(ContentMainRunner::Create());
  // 初始化这个main_runner
  int exit_code = main_runner->Initialize(params);      
  if (exit_code >= 0)
    return exit_code;
  // start message_loop， 开始开启事件循环
  exit_code = main_runner->Run();
  
  // 
  // 上面的事件循环结束后[message_loop->DeleteSoon]， 运行main_runner 的shutdown 函数， 退出程序
  main_runner->ShutDown();
}
```

ContentMainRunner 的具体实现对象是 ConentMainRunnerImpl 类，构造函数完成了类的实例化； 将所有成员变量设置初始值；所有其他初始化的的动作放在Initliaze中；





# The startup progress of  Any APP based on Chromium


main(int argc, const char**argv)
    //由argv 构造一个content::ContentMainParams 类型的param 对象；传递给ContentMain()
    content::ContentMain(params);
    
content/app/content_main.cc
ContentMain(const ContentMainParams& params) {
  //构造一个ContentMainRunner的对象
  scoped_ptr<ContentMainRunner> main_runner(ContentMainRunner::Create());
  // 初始化这个main_runner
  int exit_code = main_runner->Initialize(params);
  if (exit_code >= 0)
    return exit_code;
  // start message_loop， 开始开启事件循环
}

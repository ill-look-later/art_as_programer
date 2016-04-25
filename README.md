猿名：HuanGong
=======

All of those just for note some information i recv



## client <<==>> SrafBrowser

##cmd list

client端的“控制”相关的接口基本调用顺序为： lib_browser_client.cc  ====》 mas_client.cc <====发送ipc==ipc server端转发到mas.cc===> mas.cc中解析相应的cmd命令 ========> 通过sraf_mtm/mtm_impl.cc ===> 从mtm中访问各种浏览器内部的功能；

#### createw setfocus isfocus load reload close closeall 等命令的大致调用
```c
  sraf_clients/lib_browser_client.cc
    sraf_browser_client_create();
   sraf_mas/mas_client.cc 
    sraf_mas_client_create();
      call: send_mas_command_sync
  ----- ipc to Browser
  sraf_mas/mas.cc
    IpcServerCallback()
      call: OnCreateNewTab();
    OnCreateNewTab();
      mtm_instance->CreateTab();
 sraf_mtm/mtm_impl.cc
   CreateTab();
     .....
```

#### setfocus
```c
  sraf_clients/lib_browser_client.cc
    sraf_browser_client_set_focus();
   sraf_mas/mas_client.cc 
    sraf_mas_client_set_focus();
      call: send_mas_command()
  ----- ipc to Browser
  sraf_mas/mas.cc
    IpcServerCallback()
      call: OnChangeTabActiveStatus();
    OnChangeTabActiveStatus();
      active_status ? mtm_instance->ActiveTab(id) : mtm_instance->DeactiveTab(id);
     .....
```

  通常后续都是在MTM中通过访问shell， web_content等对象来操作页面， 对应的几个文件：
- sraf_porting/content/shell/browser/shell_sraf.cc
- src/content/shell/browser/shell.cc
- src/content/browser/web_contents/web_contents_impl.cc
- src/content/browser/renderer_host/render_XXX.cc
同完成相应的功能


#### setrect、resolution、opacity 等命令的大致调用流程；
以setrect && none toolkitiew 为例
```
sraf_browser_client_set_rect() file:sraf_clients/lib_browser_client.cc
sraf_mas_client_set_rect file: sraf_mas/mas_client.cc
send_mas_command() // 发送 Ipc 消息
----Browser 端ipc消息处理
IpcServerCallback file: sraf_mas/mas.cc
OnSetBrowserRect  file: sraf_mas/mas.cc
SetBrowserDisplayRect file: sraf_mtm/mtm_impl.cc
UpdateShellDispalyRect file: sraf_porting/content/shell/browser/shell_sraf.cc
call： platform_->host()->SetBounds(new_rect)
SetBounds() file: sraf_porting/ui/aura/window_tree_host_sraf.cc
  call: platform_window_->SetBounds(bounds);
Setbounds(); file: sraf_porting/ui/platform_window/sraf_window.cc
  call : delegate_->OnBoundsChanged(); && sraf_graphics_window_set_bounds
sraf_graphics_window_set_bounds(...); file: sraf_graphics/directfb/sraf_graphics_adaptor_dfb.c
```

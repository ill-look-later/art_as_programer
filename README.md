猿名：HuanGong
=======

All of those just for note some information i recv



## client <<==>> SrafBrowser

##cmd list
#### createw
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
 sraf_mtm/mtm_impl.cc
   CreateTab();
     .....
```
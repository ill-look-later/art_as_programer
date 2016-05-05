# 捷径

有很多事情， 鲁莽行不通， 但是转个弯， 就可轻易取得.

问题分析和代码
目前按照jiuzhou发过来的dfb samplecode 改动之后， 在测试过程中发现
- dfb的移动背后的dfb内部buffer的更新和binding 存在问题 见视频00：00：04 ～ 00：00：05秒， 函数调用为:
```
  tmp_window->Move(tmp_window, move_x, move_y);
```

- video 00：12 ～ 00： 15秒出， browser的dfb window显示出来， 但其实在此之前『我们的这个unittest run起来之前，browser的dfb window已经创建并显示，但视觉上看上去 browser没有显示出来』， 由这里我们这边的推断也是 dfb库内部维护的surface buffer管理上存在相互的影响；

- 在这个视频中00：00：55 ~ 00:01:12 中是透明度测试，这个视频中测试是ok的， 但在完全没有改动code的情况下， 存在我们之前所说的调用
```
tmp_window->SetOpacity(tmp_window, opacity);
```
  - 1：没有任何作用，或者
  - 2：越调用画面越透明的问题，好像透明在叠加的情况；
- 在video 00：02：25 出在我们进行dfb surface的测试时， 出现了花屏的现象， 也就是说在layer 0上，dfb 的surface 和 dfb的window之间在这个dfb库中window 和 surface之间的最后合成（binding）的过程中会干扰和影响；

- video 00：02：30 处， 我们在进行下一个surface的测试项的时候，dfb内部出现了错误，结束了程序， 但是在画面中可以看到，我们browser画面也变得非常非常透明。但在整个测试过程中， browser的dfb部分是没有任何代码被执行的；

- 另外在浏览器测试过程中， dfb window的surface 在进行小部分的blit 和 flip的操作会有“重影”而且显示好像和函数调用不是同步的。
  > example： 按下按键“down” log中显示已经更新了dfb window surface的部分画面，但是过了好几秒甚至十几秒后， 画面才会有更新，并且更新出来有重影的现象；

###jiuzhou提供的示例程序的dfbwindow核心初始化部分code
```cpp
     /* Fill the window description. */
     desc.flags  = DWDESC_POSX | DWDESC_POSY |
                   DWDESC_WIDTH | DWDESC_HEIGHT | DWDESC_CAPS;
     desc.posx   = 0;
     desc.posy   = 0;
     desc.width  = 1280;
     desc.height = 720;
     desc.caps   = DWCAPS_ALPHACHANNEL | DWCAPS_NODECORATION;
     layer->CreateWindow( layer, &desc, &window );


     /* Get the window's surface. */
      window->GetSurface( window, &primary );

     /* Add ghost option (behave like an overlay). */
     window->SetOptions( window, DWOP_ALPHACHANNEL | DWOP_GHOST );

     /* Move window to upper stacking class. */
     window->SetStackingClass( window, DWSC_UPPER );

     /* Make it the top most window. */
     window->RaiseToTop( window );
     window->SetOpacity( window, 0x80 );

```

对比测试：
### 对比测试一
-作为对比测试，在mstar的其他几个平台上，整个测试是没有问题的，unittest程序在进行surface测试部分的时候也没有出现crash的情况，

### 对比测试二
- **在不按照jiuzhou提供的示例修改code的情况下**
  - 1： 在我们现有的其他几个平台上测试ok，
  - 2： 在jiuzhou stb平台上测试：
    > 画面更新“重影”
    > 透明度设置不符合预期，有时候会出现几种不同的现象【设置无效， 重影】
    > dfb API LowerToBottom 会将dfbwindow置于video层之下，但通过API RaiseToTop 提到最上层时整个屏幕黑掉，而且无论怎么更新画面都不会再显示东西


我们unittest的代码:
其中我们graphics_adaptor的代码只是对dfb的api做了一个桥接和参数的检测而已没有任何其他的code；所以我们只提供核心参考的几个函数
#####创建dfb window的函数
```cpp

SRAF_ERR sraf_graphics_create_window(SrafGraphicsWindowHandle *window,int width, int height, int pos_x, int pos_y, bool is_cursor_window) {
SRAF_LOCAL_FUNC_ENTER();

  if ( width* height == 0) {
    SRAF_LOCAL_TRACE();
    return SRAF_ERR_INVALID_PARAM;
  }

  IDirectFBDisplayLayer *display_handle = get_display_layer();
  if (NULL == display_handle) {
    SRAF_LOCAL_TRACE_S("SRAF_DFB_INFO:: Get Display Layer Failed, CreateWindow Failed");
    return SRAF_ERR_GENERIC;
  }

  DFBWindowDescription desc;
#if defined(SRAF_CHIP_NAME_NT72562)
  desc.flags = (DFBWindowDescriptionFlags)(DWDESC_WIDTH | DWDESC_HEIGHT |
                                           DWDESC_POSX | DWDESC_POSY | DWDESC_STACKING);
  desc.stacking = DWSC_LOWER;
#else
  desc.flags = (DFBWindowDescriptionFlags)(DWDESC_WIDTH | DWDESC_HEIGHT |
                                           DWDESC_POSX | DWDESC_POSY | DWDESC_CAPS);
#endif
  //desc.surface_caps = DSCAPS_VIDEOONLY/* | DSCAPS_DOUBLE*/;
  desc.caps   = DWCAPS_ALPHACHANNEL | DWCAPS_NODECORATION/* | DWCAPS_DOUBLEBUFFER*/;
  desc.width = width;
  desc.height = height;
  desc.posx = pos_x;
  desc.posy = pos_y;

  SRAF_MUTEX_LOCK();
  display_handle->CreateWindow(display_handle, &desc, (IDirectFBWindow **)window);
  SRAF_MUTEX_UNLOCK();

  IDirectFBWindow *dfb_window = (IDirectFBWindow*)(*window);
  if (dfb_window == NULL) {
    SRAF_LOCAL_TRACE_S("creat dfb window failed");
    return SRAF_ERR_GENERIC;
  }

  dfb_window->SetOptions(dfb_window, DWOP_ALPHACHANNEL | DWOP_GHOST );
  dfb_window->SetStackingClass(dfb_window, DWSC_UPPER);
  dfb_window->RaiseToTop(dfb_window);
  dfb_window->SetOpacity(dfb_window, 0xDD);

#if defined(SRAF_CHIP_NAME_BCM7362)
  IDirectFBSurface *window_surface = NULL;
  dfb_window->GetSurface(dfb_window, &window_surface);
  clear_surface((void*)window_surface, 0, 0, 0, 0xff);
  window_surface->Flip(window_surface,NULL,(DFBSurfaceFlipFlags)0);
  window_surface->Release(window_surface);
#endif

  SRAF_LOCAL_FUNC_LEAVE();
  return SRAF_OK;
}
```

```cpp
/* Copyright (c), 2013, SERAPHIC Information Technology (Shanghai) Co., Ltd.
 * All Right Reserved.
 */

//#define LOG_TAG "DfbTest"

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

#include "sraf_graphics_adaptor.h"
#include "sraf_common.h"

#define SRAF_SHOW_TIMEOUT  (5) //seconds

#define SRAF_LOGE(value) printf("\n%s\n", value)

void Test_FullRect_blit_and_flip(SrafGraphicsWindowHandle, SrafGraphicsSurfaceHandle);
void Test_PartRect_blit_and_flip(SrafGraphicsWindowHandle, SrafGraphicsSurfaceHandle);
void Test_DfbWindow_opacity(SrafGraphicsWindowHandle, SrafGraphicsSurfaceHandle);
void Test_DfbWindow_z_order(SrafGraphicsWindowHandle, SrafGraphicsSurfaceHandle);

int main (int argc, char** argv)
{

  SrafGraphicsSurfaceHandle primary_surface = NULL;
  SrafGraphicsSurfaceHandle sub_surface = NULL;

  SrafGraphicsWindowHandle window = NULL;
  SrafGraphicsSurfaceHandle window_surface = NULL;

  if (SRAF_OK != sraf_graphics_initialize()) {
    SRAF_LOGE("SRAF ERR::::::sraf_graphics_initialize failed");
    return -1;
  }

  SRAF_LOGE("<<===============Start Graphic Window Test=================>>");
  SRAF_LOGE("=============================================================");

  SRAF_LOGE("=======create a 640x800 dfbwindow @ position 0,0==========");
  sraf_graphics_create_window(&window, 640, 480, 0, 0, false);
  sraf_graphics_window_set_opacity(window, 0xff);
  if (NULL == window)  {
    SRAF_LOGE("SRAF ERR::::::sraf_graphics_create_window failed");
    sraf_graphics_finalize();
    return -1;
  }

  window_surface = sraf_graphics_window_get_surface(window);
  if (NULL == window_surface) {
    SRAF_LOGE("SRAF ERR::::::sraf_graphics_window_get_surface failed");
    sraf_graphics_destroy_window(window);
    window = NULL;
    sraf_graphics_finalize();
    return -1;
  }

  SRAF_LOGE("=======clear window surface to color red      ==========");
  sraf_graphics_clear_surface(window_surface, 0, 255, 0, 0xff);
  sraf_graphics_flip(window_surface, NULL);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=======move dfb windowt to postion 200,200    ==========");
  sraf_graphics_window_move_to(window, 200, 200);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=======change dfbwindow size to 1280x720      ==========");
  sraf_graphics_set_window_size(window, 1280, 720);
  sraf_graphics_clear_surface(window_surface, 0, 255, 0, 0xff);
  sraf_graphics_flip(window_surface, NULL); //if not flip after change window size, it will be black window
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=======move dfb windowt to postion 0,0        ==========");
  sraf_graphics_window_move_to(window, 0, 0);
  sraf_graphics_clear_surface(window_surface, 0, 255, 0, 0xff);
  sraf_graphics_flip(window_surface, NULL); //if not flip after change window size, it will be black window
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=======clear window_surface to color 255,255,0==========");
  sraf_graphics_clear_surface(window_surface, 255, 255, 0, 0xff);
  sraf_graphics_flip(window_surface, NULL);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=======full rect blit and flip test          ==========");
  Test_FullRect_blit_and_flip(window, window_surface);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("====== graphiics window opacity test ==========");
  Test_DfbWindow_opacity(window, window_surface);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=======partly rect blit and partly flip test ==========");
  Test_PartRect_blit_and_flip(window, window_surface);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("========= z order test test ==========================");
  Test_DfbWindow_z_order(window, window_surface);
  sleep(SRAF_SHOW_TIMEOUT);

  sraf_graphics_destroy_surface(window_surface);
  sraf_graphics_destroy_window(window);
  window = NULL;

  sraf_graphics_release_layer();

  SRAF_LOGE("\n\x1b[31m <<====Start Graphic Surface Test=====>> \x1b[0m \n");
  SRAF_LOGE("=============================================================");
  sleep(2);

  SrafRect rect_primary = {0, 0, 0, 0};
  if (SRAF_OK != sraf_graphics_create_surface(&rect_primary, 1, 0xFFFF0000, &primary_surface)) {
    SRAF_LOGE("sraf_graphics_create_surface() error!\n");
    sraf_graphics_finalize();
    return -1;
  }

  SrafRect rect_sub = {0, 0, 60, 60};
  if (SRAF_OK != sraf_graphics_create_surface(&rect_sub, false, 0xFF0000FF, &sub_surface)) {
    sraf_graphics_destroy_surface(primary_surface);
    primary_surface = NULL;
    sraf_graphics_finalize();
    SRAF_LOGE("sraf_graphics_create_surface() error!\n");
    return -1;
  }

  if (SRAF_OK != sraf_graphics_bitblt(sub_surface, &rect_sub, primary_surface, 0, 0, 1)) {
    sraf_graphics_destroy_surface(primary_surface);
    primary_surface = NULL;

    sraf_graphics_destroy_surface(sub_surface);
    sub_surface = NULL;
    sraf_graphics_finalize();
    SRAF_LOGE("sraf_graphics_bitblt() error!\n");
    return -1;
  }

  if (SRAF_OK != sraf_graphics_flip(primary_surface,NULL)) {
    sraf_graphics_destroy_surface(primary_surface);
    primary_surface = NULL;

    sraf_graphics_destroy_surface(sub_surface);
    sub_surface = NULL;
    sraf_graphics_finalize();
    SRAF_LOGE("sraf_graphics_flip) error!\n");
    return -1;
  }

  sleep(SRAF_SHOW_TIMEOUT);

  sraf_graphics_destroy_surface(sub_surface);
  sub_surface = NULL;

  SrafRect imageRect = {0, 0, 120, 160};
  unsigned char *pImage = (unsigned char *)malloc (imageRect.w*imageRect.h*4);
  memset(pImage, 0xF0, (imageRect.w*imageRect.h*4));

  if (SRAF_OK != sraf_graphics_attach_bitmap_to_surface(pImage, &imageRect, &sub_surface)) {
    sraf_graphics_destroy_surface(primary_surface);
    primary_surface = NULL;

    sraf_graphics_finalize();
    SRAF_LOGE("sraf_graphics_bitblt() error!\n");
    return -1;
  }

  if (SRAF_OK != sraf_graphics_bitblt(sub_surface, &imageRect, primary_surface, 0, 0, 1)) {
    sraf_graphics_destroy_surface(primary_surface);
    primary_surface = NULL;

    sraf_graphics_destroy_surface(sub_surface);
    sub_surface = NULL;
    sraf_graphics_finalize();
    SRAF_LOGE("sraf_graphics_bitblt() error!\n");
    return -1;
  }

  if (SRAF_OK != sraf_graphics_flip(primary_surface,NULL)) {
    sraf_graphics_destroy_surface(primary_surface);
    primary_surface = NULL;

    sraf_graphics_destroy_surface(sub_surface);
    sub_surface = NULL;
    sraf_graphics_finalize();
    SRAF_LOGE("sraf_graphics_flip) error!\n");
    return -1;
  }

  sleep(SRAF_SHOW_TIMEOUT);

  if (pImage) {
    free (pImage);
    pImage = NULL;
  }

  sraf_graphics_destroy_surface(primary_surface);
  primary_surface = NULL;

  sraf_graphics_destroy_surface(sub_surface);
  sub_surface = NULL;

  sraf_graphics_finalize();

  return 0;
}


void Test_FullRect_blit_and_flip(SrafGraphicsWindowHandle window,
                                 SrafGraphicsSurfaceHandle surface) {
  SrafGraphicsSurfaceHandle sub_surface = NULL;
  SrafRect rect = {0, 0, 1280, 720};

  SRAF_LOGE("=========== show a colored 'red' full rect===============");
  sraf_graphics_create_surface(&rect, false, 0xFFFF0000, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, NULL, surface, 0, 0, 0xff);
  sraf_graphics_flip(surface, NULL);
  sraf_graphics_destroy_surface(sub_surface);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=========== show a colored 'green' full rect===============");
  sraf_graphics_create_surface(&rect, false, 0xFF00FF00, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, NULL, surface, 0, 0, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);

  SRAF_LOGE("=========== show a colored 'FF00FF' full recti with alpha 0xDD=");
  sraf_graphics_create_surface(&rect, false, 0xDDFF00FF, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, NULL, surface, 0, 0, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);
}

void Test_PartRect_blit_and_flip(SrafGraphicsWindowHandle window,
                                 SrafGraphicsSurfaceHandle surface) {
  SrafGraphicsSurfaceHandle sub_surface = NULL;
  SrafRect rect = {0, 0, 320, 240};

  SRAF_LOGE("=========== show a  120x120 'red' rect at 0,0 =========");
  sraf_graphics_create_surface(&rect, false, 0xFFFF0000, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, &rect, surface, 0, 0, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=========== show a 120x120 'green' rect at 200, 200 with alpha 0xEE====");
  sraf_graphics_create_surface(&rect, false, 0xEE00FF00, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, &rect, surface, 200, 200, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=========== show a 120x120 'green' rect at 120, 0  with alpha 0xDD ====");
  sraf_graphics_create_surface(&rect, false, 0xDD0000FF, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, &rect, surface, 420, 0, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);
  sleep(SRAF_SHOW_TIMEOUT);

  SRAF_LOGE("=========== show a 120x120 'green' rect at 380,330  with alpha 0xCC====");
  sraf_graphics_create_surface(&rect, false, 0xCC00EEFF, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, &rect, surface, 380, 330, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);
  sleep(SRAF_SHOW_TIMEOUT);
}

void Test_DfbWindow_opacity(SrafGraphicsWindowHandle window, SrafGraphicsSurfaceHandle surface) {
  SrafGraphicsSurfaceHandle sub_surface = NULL;
  SrafRect rect = {0, 0, 1280, 720};
  sraf_graphics_create_surface(&rect, false, 0xFF00FF00, &sub_surface);
  if (sub_surface == NULL) {
    SRAF_LOGE("=========== create dfb surface failed ===============");
  }
  sraf_graphics_bitblt(sub_surface, NULL, surface, 0, 0, 0xff);
  sraf_graphics_destroy_surface(sub_surface);
  sraf_graphics_flip(surface, NULL);

  SRAF_LOGE("=========== start opacity test ===============");
  int opacity = 0x00;
  printf("\nOpacity: [0xff]");
  while(opacity < 256) {
    printf("->[%d]", opacity);
    sraf_graphics_window_set_opacity(window, opacity);
    opacity += 0x0f;
    sleep(1);
  }

  opacity = 256;
  while(opacity > 0) {
    printf("->[%d]", opacity);
    sraf_graphics_window_set_opacity(window, opacity);
    opacity -= 0x0f;
    sleep(1);
  }

  SRAF_LOGE("=========== start opacity Test End=============");
  sraf_graphics_window_set_opacity(window, 0xff);
}

void Test_DfbWindow_z_order(SrafGraphicsWindowHandle window, SrafGraphicsSurfaceHandle surface) {
  SRAF_LOGE("=========== start window Z order test ===============");
  int times = 0;
  while (times++ < 3) {
    SRAF_LOGE("=========== raise to top ===============");
    sraf_graphics_window_raise_to_top(window);
    sleep(SRAF_SHOW_TIMEOUT);

    SRAF_LOGE("===========lower to bottom =============");
    sraf_graphics_window_lower_to_bottom(window);
    sleep(SRAF_SHOW_TIMEOUT);
  }

  SRAF_LOGE("===========z order test END =============");
  sraf_graphics_window_raise_to_top(window);
}

```
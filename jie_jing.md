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
# 捷径

有很多事情， 鲁莽行不通， 但是转个弯， 就可轻易取得.

问题分析和代码
目前按照jiuzhou发过来的dfb samplecode 改动之后， 在测试过程中发现
- dfb的移动背后的dfb内部buffer的更新和binding 存在问题 见视频00：00：04 ～ 00：00：05秒， 函数调用为
```
  tmp_window->Move(tmp_window, move_x, move_y);
```
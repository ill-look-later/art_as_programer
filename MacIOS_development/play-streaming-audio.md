# 播放音频流媒体文件

在原来的教程中, 使用的是 MPMoviePlayerController 来播放音频文件; 在最新的系统上尝试那些代码的时候却给出了NS\_DEPRECATED\_IOS\(2\_0, 9\_0, "Use AVPlayerViewController in AVKit."\) 提示, 也就是说,这个代码从ios9开始就被废弃了. 提示说用AVkt框架中得AVPlayerViewController 来替代, 通过文档, 发现它集成子UIViewController, 而我并不打算呈现一个默认的UI界面. 而通过AVPlayerViewController中得成员, 可以看出它的内部是通过AVPlayer来实现的; 这样就很自然而然的找到了解决办法,

从代码的相关声明中可以明显的搞定如何使用, 下面看一个最简单的实例:

仅仅播放一个流媒体:

```cpp
var audioplayer: AVPlayer! = nil
audioplayer = AVPlayer();
audioplayer.replaceCurrentItemWithPlayerItem(AVPlayerItem(URL: NSURL(string: "http://x.com/xxxx/xxx.mp3")!))
audioplayer.play(); 
```

这里我使用了空参数的构造函数, 事实上, AVPlayer还提供了另外的两个构造函数,

* public init\(URL: NSURL\)

* public init\(playerItem item: AVPlayerItem\)


他们分别从一个URL

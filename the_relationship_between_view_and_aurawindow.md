# The Relationship Between View && aura::window


aura::window 里面会通过LayerOwner维护着一个layer， 在Init() 函数调用的时候创建， 并且通过Setdelegate 将aura::window 作为layer的delegate

webcontent 维护着一个webcontentview的对象，
    // The WebContentsView is an interface that is implemented by the platform-
    // dependent web contents views. The WebContents uses this interface to talk to
    // them.
nativeview ———aura::window

# The Relationship Between View && aura::window


aura::window 里面会维护着一个layer， 在Init() 函数调用的时候创建， 并且通过Setdelegate 将aura::window 作为layer的delegate

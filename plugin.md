# Plugin

###plugin 的创建


```cpp
//content/renderer/render_frame_impl.cc
RenderFrameImpl::CreatePlugin
  new WebPluginImpl(frame, params, info.path, render_view_, this);
```


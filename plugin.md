# Plugin

###plugin 的创建


```cpp
//content/renderer/render_frame_impl.cc
RenderFrameImpl::CreatePlugin(RenderFrameImpl::CreatePlugin(),const WebPluginInfo& info, const blink::WebPluginParams& params)
  new WebPluginImpl(frame, params, info.path, render_view_, this);
```


# Plugin


现在chromium浏览器中有NPAPI 和 新的PPAPI 插件， 两种插件区分是通过GetInfoForPlugin（）函数获得一个plugin_info的对来来判断是创建PPAPI 还是 NPAPI 插件， 这里我们要区分看在chromium中的扩展 和 插件的区别， 我们这里说明的是插件plugin，而非扩展extension


创建分为两个大的步骤， 创建整个关系链 + 插件instance的Init

###plugin 的创建


第一个过程： 创建plugin整个类的关系链
---

core/html/HTMLPlugInElement.cpp:599: 
    call: widget = frame->loader().client()->createPlugin(this, url, paramNames, paramValues, mimeType, loadManually, policy);

third_party/WebKit/Source/web/FrameLoaderClientImpl.cpp
  call: WebPlugin* webPlugin = m_webFrame->client()->createPlugin(m_webFrame, params);
  call: **if (!webPlugin->initialize(container.get()))**    (#第二条线，创建后plugin的初始化)

content/renderer/render_frame_impl.cc ： 继承blink里面的WebFrameClient.h
blink::WebPlugin* RenderFrameImpl::createPlugin
  call: CreatePlugin(frame, info, params_to_use);
  
blink::WebPlugin* RenderFrameImpl::CreatePlugin
    call: return new WebPluginImpl(frame, params, info.path, render_view_, this);

在创建NPAPI的这个webpluginImpl类的构造函数中， 仅仅是将params存起来而已，真的是啥都没干，不行你看
```cpp
```



第二个过程： 创建plugin整个类的关系链
---

```cpp
//content/renderer/render_frame_impl.cc
RenderFrameImpl::CreatePlugin(RenderFrameImpl::CreatePlugin(),
                               const WebPluginInfo& info,
                               const blink::WebPluginParams& params)
  new WebPluginImpl(frame, params, info.path, render_view_, this);
 
```
content/renderer/npapi/webplugin_impl.cc

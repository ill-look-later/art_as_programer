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

WebPluginImpl::WebPluginImpl(
    WebFrame* webframe,
    const WebPluginParams& params,
    const base::FilePath& file_path,
    const base::WeakPtr<RenderViewImpl>& render_view,
    RenderFrameImpl* render_frame)
    : windowless_(false),
      window_(gfx::kNullPluginWindow),
      accepts_input_events_(false),
      render_frame_(render_frame),
      render_view_(render_view),
      webframe_(webframe),
      delegate_(NULL),
      container_(NULL),
      npp_(NULL),
      plugin_url_(params.url),
      load_manually_(params.loadManually),
      first_geometry_update_(true),
      ignore_response_error_(false),
      file_path_(file_path),
      mime_type_(base::UTF16ToASCII(params.mimeType)),
      loader_client_(this),
      weak_factory_(this) {
  DCHECK_EQ(params.attributeNames.size(), params.attributeValues.size());
  base::StringToLowerASCII(&mime_type_);

  for (size_t i = 0; i < params.attributeNames.size(); ++i) {
    arg_names_.push_back(params.attributeNames[i].utf8());
    arg_values_.push_back(params.attributeValues[i].utf8());
  }

  // Set subresource URL for crash reporting.
  base::debug::SetCrashKeyValue("subresource_url", plugin_url_.spec());
}
```

第二个过程： 创建plugin整个类的关系链
---
至此， 我们整个render进程中， plugin 从一个html的element一步步的创建出一系列相关类的对象， 因为创建可能失败， 所以我们需要确保创建成功， 一旦创建成功， 我们就可以开始初始化我们真正的plugininstance， 在这个第二过程中， 会完成plugin进程中相应的object的创建；
从我们上面的分析中
third_party/WebKit/Source/web/FrameLoaderClientImpl.cpp
  call: WebPlugin* webPlugin = m_webFrame->client()->createPlugin(m_webFrame, params);
  call: **if (!webPlugin->initialize(container.get()))**    (#第二条线，创建后plugin的初始化)
```cpp
//content/renderer/render_frame_impl.cc
RenderFrameImpl::CreatePlugin(RenderFrameImpl::CreatePlugin(),
                               const WebPluginInfo& info,
                               const blink::WebPluginParams& params)
  new WebPluginImpl(frame, params, info.path, render_view_, this);
 
```
content/renderer/npapi/webplugin_impl.cc

# What Happend When js Call window.close

分析这个close或者说window.open并不是因为他们有多重要；而是这只是一个普遍的过程，抛砖引玉而已，整个设计的结构就是这样的，只要弄清一个，就可以理会其他的实现；我们在js中调用`window.close()`；首先我们应该明确，js本身而言，它没有window这个全局对象，也不具备文件访问能力；所以也没有所谓的`window.close()`方法，那么我们之所以能调用，肯定是有一个什么东西告诉js的执行引擎“嘿，伙计，待会有人如果在.js文件中写了个`window.close()`你就到某个地方找到某个对象，执行某段代码就好了”；不管是webcore还是v8，都是这样子的；在blink中所以肯定是有一个地方，有某段代码或向v8运行的context中注入了一些对象；

首先我们来看一些概念：
1.  IDL：接口定义语言，详细解释可见http://trac.webkit.org/wiki/WebKitIDL
2.  Bindings：WebKit动态生成与其他框架（如JavascriptCore, V8）整合的代码

v8 只是负责解析js的写出来的一句句语句，它并不能帮我们完成我们想要实现的功能，想像一下，如果这些所有的功能都是v8来完成，v8得有多臃肿庞大，那将是多么的恐怖。而idl和bindings就是来辅助完成这样的一种由js代码到我们真正的执行环境的一种方法；web IDL是针对web定义的一些列接口规范；而bindings则是通过一定的规则解析idl描述出来的接口，通过特定语言的codegenerator代码生成器生成面向特定语言的代码；而每个js解析器的接口和注册方法、机制不一样，所以就要针对不同的解析器生成不同的代码；v8中，使用V8 Bindings Generator来生成代码；一个python写的idl解析程序；[引用一个博客](http://blog.csdn.net/cutesource/article/details/8862287)上面的一张图；
![idl-bindings](/meet_chromium/img/webkit_v8_webidl_bindings.png)

在`third_party/WebKit/Source/core/frame/Window.idl`中， 我们为通过webIDL（web Interface description language）为窗口绑定了open close方法；

    [DoNotCheckSecurity, CallWith=ExecutionContext] void close();

根据chormium[这篇文档中的描述](https://www.chromium.org/developers/web-idl-interfaces)在解析idl的时候，bindings相关的代码会假定我们在idl中描述的接口，类，变量等等每一个属性都是实现好的；而在v47版本当中，它的实现是`third_party/WebKit/Source/core/frame/LocalDOMWindow.cpp`文件中实现的；

- 通过idl的编译解析工具idl_compiler.py解析之后生成`out/Release/gen/blink/bindings/core/v8/V8Window.cpp[h]`文件
- 在V8Window.cpp[h]中一方面是实现了对LocalDOMWindow各个方法属性的代理和jsapi到各个包装API的映射表，到时候通过调用installV8WindowFunction等静态函数将所有的属性与方法注册进去;
- 当新解析一个页面的时候向已初始化v8解析环境中进入这些属性和方法；blink将V8Window.cpp中静态函数将映射表中所有的属性和方法都注册到v8的context中；
- 当V8解析到js代码`window.close()`时通过映射表找到对应方法在包装对象V8Window.cpp中的实现`closeMethodCallback`; 下面的代码可以看出，通过调用开始注册进去的callback函数`closeMethodCallback`内部再次调用`LocalDOMWindowV8Internal::closeMethod`；在closeMethod中得到blink中的LocalDomWindowImpl对象；通过这个对象调用在LocalDOMWindowImpl.cpp中的close方法；
    static void closeMethodCallback(const v8::FunctionCallbackInfo<v8::Value>& info) {
        TRACE_EVENT_SET_SAMPLING_STATE("blink", "DOMMethod");
        LocalDOMWindowV8Internal::closeMethod(info);
        TRACE_EVENT_SET_SAMPLING_STATE("v8", "V8Execution");
    }
    static void closeMethod(const v8::FunctionCallbackInfo<v8::Value>& info)
    {
        LocalDOMWindow* impl = V8Window::toImpl(info.Holder());
        ExecutionContext* executionContext = currentExecutionContext(info.GetIsolate());
        impl->close(executionContext);
    }

在查看V8Window.cpp 中，通过关键字`close` 与函数对象的包装函数形成一个结构体，任何一个继承了`ScriptWrappable`的变量都要通过宏`DEFINE_WRAPPERTYPEINFO`来对扩充这个对象的功能，其实就是给当前类添加了一个public的方法`const WrapperTypeInfo* wrapperTypeInfo() const override`和一个静态对象`static const WrapperTypeInfo& s_wrapperTypeInfo`,看下面的代码：

    //out/Debug/gen/blink/bindings/core/v8/V8Window.cpp
    const WrapperTypeInfo& DOMWindow::s_wrapperTypeInfo = V8Window::wrapperTypeInfo;

也就是说，我们注册的那些函数，方法，属性，都通过一层层的包装，包装成v8里面的一些对象和结构体，存储在了s_wrapperTypeInfo 这个静态变量里面了，webkit、blink中每一个`ScriptWrappable`的对象，即每一个可以在js中访问交互的对象，变量，属性，方法都需要通过这样的方法保存到WrapperTypeInfo中，也就是说fram，wiindow，document，navigator，css所有的动画变量什么的都需要通过这样的宏生成一个包装对象，等到合适的时机通过wrap来将这些注册到V8context中；

最后至于如何一步步在加载一个新页面在什么时机调用这些静态函数将这些注册到v8中；C/C++功力暂时有点吃力，加上对V8不甚了解，暂时还没完全弄懂，以后有机会更多的了解V8之后，可能有机会再来弄懂这些；



```c
//src/out/Release/gen/blink/bindings/core/v8/V8Window.cpp
static void closeMethod(const v8::FunctionCallbackInfo<v8::Value>& info)
{
    LocalDOMWindow* impl = V8Window::toImpl(info.Holder());
    ExecutionContext* executionContext = currentExecutionContext(info.GetIsolate());
    impl->close(executionContext);
}
static void closeMethodCallback(const v8::FunctionCallbackInfo<v8::Value>& info)
{
    TRACE_EVENT_SET_SAMPLING_STATE("blink", "DOMMethod");
    LocalDOMWindowV8Internal::closeMethod(info);
    TRACE_EVENT_SET_SAMPLING_STATE("v8", "V8Execution");
}
```
到这里我们身下的就是对整个函数调用的callstack的一个跟踪了；在V8Window.cpp中通过`LocalDOMWindow* impl = V8Window::toImpl(info.Holder());`拿到了我们最终的在webkit中的实现部分；在下面的close中我们可以看到做了一些类的条件检测，只有当我们的页面是通过window.open打开且backForwardList <=1 且在我们的webpreference中允许js关闭窗口时，我们才会关闭当前的页面；当上面所有的条件都满足的时候调用了chrome对象的`closeWindowSoon()`方法来关闭页面；其实chrome对象也是将这个动作转发给了我chromeclient对象去执行；在chromeclient中， 通过`m_webView->mainFrame()->stopLoading();`来停止加载整个页面，最后通过`m_webView->client()->closeWidgetSoon();`调用到chromium的conent层的closewidgetSoon来关闭这个页面；

```c
void LocalDOMWindow::close(ExecutionContext* context) {
    if (!m_frame || !m_frame->isMainFrame())
        return;

    Page* page = m_frame->page();
    if (!page)
        return;
    if (context) {
        ASSERT(isMainThread());
        Document* activeDocument = toDocument(context);
        if (!activeDocument)
            return;
        if (!activeDocument->canNavigate(*m_frame))
            return;
    }
    Settings* settings = m_frame->settings();
    bool allowScriptsToCloseWindows = settings && settings->allowScriptsToCloseWindows();

    if (!(page->openedByDOM() || page->backForward().backForwardListCount() <= 1 ||  allowScriptsToCloseWindows)) {
        frameConsole()->addMessage(ConsoleMessage::create(JSMessageSource, WarningMessageLevel, "Scripts may close only the windows that were opened by it."));
        return;
    }

    if (!m_frame->loader().shouldClose())
        return;
    page->chrome().closeWindowSoon();
}
```

```c
void Chrome::closeWindowSoon() {
    m_client->closeWindowSoon();
}

void ChromeClientImpl::closeWindowSoon() {
    // Make sure this Page can no longer be found by JS.
    Page::ordinaryPages().remove(m_webView->page());
    // Make sure that all loading is stopped.  Ensures that JS stops executing!
    m_webView->mainFrame()->stopLoading();
    if (m_webView->client())
        m_webView->client()->closeWidgetSoon();
}
```

```c
void RenderWidget::closeWidgetSoon() {
  DCHECK(content::RenderThread::Get());
  if (is_swapped_out_) {
    // This widget is currently swapped out, and the active widget is in a
    // different process.  Have the browser route the close request to the
    // active widget instead, so that the correct unload handlers are run.
    Send(new ViewHostMsg_RouteCloseEvent(routing_id_));
    return;
  }

  // If a page calls window.close() twice, we'll end up here twice, but that's
  // OK.  It is safe to send multiple Close messages.

  // Ask the RenderWidgetHost to initiate close.  We could be called from deep
  // in Javascript.  If we ask the RendwerWidgetHost to close now, the window
  // could be closed before the JS finishes executing.  So instead, post a
  // message back to the message loop, which won't run until the JS is
  // complete, and then the Close message can be sent.
  base::ThreadTaskRunnerHandle::Get()->PostTask(
      FROM_HERE, base::Bind(&RenderWidget::DoDeferredClose, this));
}
```



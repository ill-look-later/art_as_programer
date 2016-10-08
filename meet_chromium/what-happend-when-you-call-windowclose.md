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
- 在V8Window.cpp[h]中一方面是实现了对LocalDOMWindow各个方法属性的代理和jsapi到各个包装API的映射表，通过installV8WindowTemplate将所有的属性与方法注册进去;



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



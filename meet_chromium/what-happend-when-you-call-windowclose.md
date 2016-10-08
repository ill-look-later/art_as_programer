# What Happend When js Call window.close

在`third_party/WebKit/Source/core/frame/Window.idl`中， 我们为通过webIDL（web Interface description language）为窗口绑定了open close方法；
![idl-bindings](/meet_chromium/img/webkit_v8_webidl_bindings.png)

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



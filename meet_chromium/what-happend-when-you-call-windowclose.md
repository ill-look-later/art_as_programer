# What Happend When js Call window.close

```c
void LocalDOMWindow::close(ExecutionContext* context)
{
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

    if (!(page->openedByDOM() || 
        page->backForward().backForwardListCount() <= 1 || allowScriptsToCloseWindows)) {

        frameConsole()->addMessage(ConsoleMessage::create(JSMessageSource, WarningMessageLevel, "Scripts may close only the windows that were opened by it."));
        return;
    }

    if (!m_frame->loader().shouldClose())
        return;
    page->chrome().closeWindowSoon();
}
```
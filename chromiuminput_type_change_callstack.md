# Chromium_input_type_change_call_stack
###主要三种情况
- html element focus
- pepper plugin set focus
- mouse select event for editor
---


```
void EditorClientImpl::respondToChangedSelection(LocalFrame* frame, SelectionType selectionType)
{
    WebLocalFrameImpl* webFrame = WebLocalFrameImpl::fromFrame(frame);
    if (webFrame->client())
        webFrame->client()->didChangeSelection(selectionType != RangeSelection);
}

virtual void didChangeSelection(bool isSelectionEmpty) { }
//override by render_frame
```


## Focus
```
void Element::focus(const FocusParams& params)
    call: document().page()->chromeClient().showImeIfNeeded();

void ChromeClientImpl::showImeIfNeeded()
    call: if (m_webView->client())
            m_webView->client()->showImeIfNeeded();
        

void RenderWidget::showImeIfNeeded():
// override src/third_party/WebKit/public/web/WebWidgetClient.h	
    call: OnShowImeIfNeeded();

void RenderWidget::OnShowImeIfNeeded()
    Call: UpdateTextInputState
    
void RenderWidget::UpdateTextInputState(ShowIme show_ime,
                        ChangeSource change_source)
    set: text_input_type_ = new_type;
```







# Chromium_input_type_change_call_stack

```
void EditorClientImpl::respondToChangedSelection(LocalFrame* frame, SelectionType selectionType)
{
    WebLocalFrameImpl* webFrame = WebLocalFrameImpl::fromFrame(frame);
    if (webFrame->client())
        webFrame->client()->didChangeSelection(selectionType != RangeSelection);
}

virtual void didChangeSelection(bool isSelectionEmpty) { }
//override by render_frame





void Element::focus(const FocusParams& params)
    document().page()->chromeClient().showImeIfNeeded();

void ChromeClientImpl::showImeIfNeeded()
    if (m_webView->client())
        m_webView->client()->showImeIfNeeded();
        
void RenderWidget::showImeIfNeeded():
// override src/third_party/WebKit/public/web/WebWidgetClient.h	
    OnShowImeIfNeeded();
void RenderWidget::OnShowImeIfNeeded()
    Call: UpdateTextInputState
    
void RenderWidget::UpdateTextInputState(ShowIme show_ime,
                        ChangeSource change_source)

```
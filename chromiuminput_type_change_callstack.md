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


```
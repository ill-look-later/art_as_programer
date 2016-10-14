# Aura_Toolkit_Event

这是一份v39上的chromium在x11平台上的事件的backtrace，这时候的事件分发其实还是比较乱的，从V32开始引入新的AURA以来， 改动比较大，新一点的v47 v52上面整理的改进之后，这一部分变得简单多了；

0x0000000004de5c25 in ui::(anonymous namespace)::XSourceDispatch (source=0x16b02cd9f860, unused_func=0x0, data=0x16b02ce37a40) at ../../ui/events/platform/x11/x11_event_source_glib.cc:39
 0x0000000004de534b in ui::X11EventSource::DispatchXEvents (this=0x16b02ce37a40) at ../../ui/events/platform/x11/x11_event_source.cc:118
 
 0x0000000004de543f in ui::X11EventSource::DispatchEvent (this=0x16b02ce37a40, xevent=0x7fffffffc7b8)
    at ../../ui/events/platform/x11/x11_event_source.cc:142
    
0x0000000004dde023 in ui::PlatformEventSource::DispatchEvent (this=0x16b02ce37a40, platform_event=0x7fffffffc7b8)
    at ../../ui/events/platform/platform_event_source.cc:79
    
0x0000000004a6bfdf in non-virtual thunk to views::DesktopWindowTreeHostX11::DispatchEvent(_XEvent* const&) ()
    at ../../ui/views/widget/desktop_aura/desktop_window_tree_host_x11.cc:1916

views::DesktopWindowTreeHostX11::DispatchEvent

ui::EventSource::SendEventToProcessor

ui::EventSource::DeliverEventToProcessor

ui::EventProcessor::OnEventFromSource

ui::EventDispatcherDelegate::DispatchEvent

ui::EventDispatcherDelegate::DispatchEventToTarget

ui::EventDispatcher::ProcessEvent

ui::EventDispatcher::DispatchEventToEventHandlers

ui::EventDispatcher::DispatchEvent

ui::EventHandler::OnEvent

wm::CompoundEventFilter::OnKeyEvent

wm::CompoundEventFilter::FilterKeyEvent

wm::InputMethodEventFilter::OnKeyEvent

ui::InputMethodAuraLinux::DispatchKeyEvent

ui::InputMethodBase::DispatchKeyEventPostIME

wm::InputMethodEventFilter::DispatchKeyEventPostIME

wm::InputMethodEventFilter::DispatchKeyEventPostIME

ui::EventProcessor::OnEventFromSource

ui::EventDispatcherDelegate::DispatchEvent

ui::EventDispatcherDelegate::DispatchEventToTarget

ui::EventDispatcher::ProcessEvent

ui::EventDispatcher::DispatchEvent

ui::EventTarget::OnEvent

ui::EventHandler::OnEvent

views::DesktopNativeWidgetAura::OnKeyEvent(ui::KeyEvent*)

views::DesktopNativeWidgetAura::OnKeyEvent

 views::Widget::OnKeyEvent

ui/views/widget/desktop_aura/desktop_native_widget_aura.cc
  void DesktopNativeWidgetAura::RepostNativeEvent(gfx::NativeEvent native_event)
```
void DesktopNativeWidgetAura::RepostNativeEvent(gfx::NativeEvent native_event) {
  OnEvent(native_event);
}
```

ui/events/event_handler.cc
  void EventHandler::OnEvent(Event* event)


src/ui/views/widget/desktop_aura/desktop_native_widget_aura.cc
```
void DesktopNativeWidgetAura::OnKeyEvent(ui::KeyEvent* event) {
  if (event->is_char()) {
    // If a ui::InputMethod object is attached to the root window, character
    // events are handled inside the object and are not passed to this function.
    // If such object is not attached, character events might be sent (e.g. on
    // Windows). In this case, we just skip these.
    return;
  }
  // Renderer may send a key event back to us if the key event wasn't handled,
  // and the window may be invisible by that time.
  if (!content_window_->IsVisible())
    return;

  native_widget_delegate_->OnKeyEvent(event);
}
```

widget.cc
```

```
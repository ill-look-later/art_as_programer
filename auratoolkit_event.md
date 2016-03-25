# Aura_Toolkit_Event
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
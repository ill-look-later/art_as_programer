void RenderWidget::OnRepaint(gfx::Size size_to_paint)
void RenderWidgetCompositor::SetNeedsRedrawRect(gfx::Rect damage_rect);
void LayerTreeHost::SetNeedsRedrawRect(const gfx::Rect& damage_rect)




RenderWidget::SetNeedsRedrawRect

RenderWidgetCompositor::SetNeedsRedrawRect(const gfx::Recctt&damagerect)




render_widget 维护一个 RenderWidgetCompositor 【compositor_】对象

RenderWidgetCompositor里面有cc::LayerTreeHost<layer_tree_host_> 对象；
,
RenderWidget::SetNeedsRedrawRect

RenderWidgetCompositor::SetNeedsRedrawRect(const gfx::Recctt&damagerect)




render_widget 维护一个 RenderWidgetCompositor 【compositor_】对象

RenderWidgetCompositor里面有cc::LayerTreeHost<layer_tree_host_> 对象；
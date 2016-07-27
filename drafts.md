void RenderWidget::OnRepaint(gfx::Size size_to_paint)
void RenderWidgetCompositor::SetNeedsRedrawRect(gfx::Rect damage_rect);
void LayerTreeHost::SetNeedsRedrawRect(const gfx::Rect& damage_rect)
void ProxyMain::SetNeedsRedraw(const gfx::Rect& damage_rect);
void ThreadProxy::SetNeedsRedraw(const gfx::Rect& damage_rect)
   > PostTask(FROM_HERE, base::Bind(&ThreadProxy::SetNeedsRedrawRectOnImplThread,...);

void ThreadProxy::SetNeedsRedrawRectOnImplThread(const gfx::Rect& damage_rect)
   > impl().layer_tree_host_impl->SetViewportDamage(damage_rect);

void ThreadProxy::SetNeedsRedrawOnImplThread();
   > impl().scheduler->SetNeedsRedraw();

void Scheduler::SetNeedsAnimate();
   > state_machine_.SetNeedsAnimate();
   > ProcessScheduledActions();

// LayerTreeHost<-->ProxyMain<-->ChannelMain
//                                    |
//                                    | 
//                                ChannelImpl<-->ProxyImpl<-->LayerTreeHostImpl

RenderWidget::SetNeedsRedrawRect

RenderWidgetCompositor::SetNeedsRedrawRect(const gfx::Recctt&damagerect)



render_widget 维护一个 RenderWidgetCompositor 【compositor_】对象
RenderWidgetCompositor里面有cc::LayerTreeHost<layer_tree_host_> 对象；
RenderWidget::SetNeedsRedrawRect

RenderWidgetCompositor::SetNeedsRedrawRect(const gfx::Recctt&damagerect)




render_widget 维护一个 RenderWidgetCompositor 【compositor_】对象

RenderWidgetCompositor里面有cc::LayerTreeHost<layer_tree_host_> 对象；
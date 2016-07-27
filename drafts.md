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
   > state_machine_.SetNeedsAnimate();ProcessScheduledActions();
void Scheduler::ProcessScheduledActions();
   > call SchedulerClient* serial function according to action type
void Scheduler::DrawAndSwapIfPossible()//判断是否可以绘制， 如果可以绘制的话，调用client的绘制接口
  - client_->ScheduledActionDrawAndSwapIfPossible();
  - state_machine_.DidDrawIfPossibleCompleted(result); //绘制完成后更新状态机的状态

DrawResult ThreadProxy::ScheduledActionDrawAndSwapIfPossible()
  - DCHECK(impl().layer_tree_host_impl->CanDraw());
  - DrawSwapInternal(forced_draw);

DrawResult ThreadProxy::DrawSwapInternal(bool forced_draw);


ProxyImpl:  LayerTreeHostImplClient, SchedulerClient 








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
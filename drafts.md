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
  - layer_tree_host_impl->DrawLayers( const FrameData& frame )
  -  LayerTreeHostImpl::DidDrawAllLayers(const FrameData& frame) 

LayerTreeHostImpl::DidDrawAllLayers(const FrameData& frame);
其中，DrawLayers 中会判断这个 framedata中是否有需要更新的区域， 如果没有就直接返回了；
否则就拿当前系统是使用哪种绘制方式 **const DrawMode draw_mode = GetDrawMode();**
```CPP
enum DrawMode {
 DRAW_MODE_NONE,
 DRAW_MODE_HARDWARE,
 DRAW_MODE_SOFTWARE,
 DRAW_MODE_RESOURCELESS_SOFTWARE
};
```

LayerTreeHostImpl::DidDrawAllLayers(const FrameData& frame);


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
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

如果是  DRAW_MODE_RESOURCELESS_SOFTWARE 模式，这里会新创建一个临时的SoftRender来绘制；否则调用layer_tree_host_impl 内部的 SoftRender对象来完成绘制，在DrawFrame中，每完成一部分的绘制， 会根据需要copy到bitmap当中；

```Cpp
void DirectRenderer::DrawFrame(RenderPassList* render_passes_in_draw_order,
 float device_scale_factor,
 const gfx::Rect& device_viewport_rect,
 const gfx::Rect& device_clip_rect,
 bool disable_picture_quad_image_filtering) {
 TRACE_EVENT0("cc", "DirectRenderer::DrawFrame");
 UMA_HISTOGRAM_COUNTS("Renderer4.renderPassCount",
 render_passes_in_draw_order->size());
 const RenderPass* root_render_pass = render_passes_in_draw_order->back();
 DCHECK(root_render_pass);
 DrawingFrame frame;
 frame.render_passes_in_draw_order = render_passes_in_draw_order;
 frame.root_render_pass = root_render_pass;
 frame.root_damage_rect = Capabilities().using_partial_swap
 ? root_render_pass->damage_rect
 : root_render_pass->output_rect;
 frame.root_damage_rect.Intersect(gfx::Rect(device_viewport_rect.size()));
 frame.device_viewport_rect = device_viewport_rect;
 frame.device_clip_rect = device_clip_rect;
 frame.disable_picture_quad_image_filtering =
 disable_picture_quad_image_filtering;
 overlay_processor_->ProcessForOverlays(render_passes_in_draw_order,
 &frame.overlay_list);
 EnsureBackbuffer();
 // Only reshape when we know we are going to draw. Otherwise, the reshape
 // can leave the window at the wrong size if we never draw and the proper
 // viewport size is never set.
 output_surface_->Reshape(device_viewport_rect.size(), device_scale_factor);
 BeginDrawingFrame(&frame);
 for (size_t i = 0; i < render_passes_in_draw_order->size(); ++i) {
 RenderPass* pass = render_passes_in_draw_order->at(i);
 DrawRenderPass(&frame, pass);
 for (ScopedPtrVector<CopyOutputRequest>::iterator it =
 pass->copy_requests.begin();
 it != pass->copy_requests.end();
 ++it) {
 if (i > 0) {
 // Doing a readback is destructive of our state on Mac, so make sure
 // we restore the state between readbacks. http://crbug.com/99393.
 UseRenderPass(&frame, pass);
 }
 CopyCurrentRenderPassToBitmap(&frame, pass->copy_requests.take(it));
 }
 }
 FinishDrawingFrame(&frame);
 render_passes_in_draw_order->clear();
}
```
当绘制完成后调用**FinishDrawingFrame(&frame);**来完成绘制；绘制完成后调用FinishDrawingFrame函数来通知SoftwareOutputDevice::EndPaint() 来将绘制完成的数据copy到显示后端去完成显示；比如说在我们嵌入式linux平台是， 我们对接到dfb的surface上；之后通过dfbwindow的相关接口跟新显示；

很遗憾的是这部分在新的chromium中变动还是很大的， 我这部分的分析是基于v39上；在新的版本v50版本上这部分很多函数都有所更新， 不过整体的流程还是大致一样额，具体可以到最新的代码上看一下；



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




```C++
std::unique_ptr<RenderWidgetHostIterator> widgets(
      RenderWidgetHostImpl::GetAllRenderWidgetHosts());
  while (RenderWidgetHost* widget = widgets->GetNextHost()) {
    RenderViewHost* rvh = RenderViewHost::From(widget);
    if (!rvh)
      continue;

    // Skip widgets in other processes.
    if (rvh->GetProcess()->GetID() != GetID())
      continue;

    rvh->OnWebkitPreferencesChanged();
```





helloMessage 
PluginProcessHost::OnChannelConnected(int32 peer_pid)

这个调用是从browserChildProcessHostDelegate处继承来的， 当BrowserChiledProssHost接收到hellomsg之后就转发到了PluginProcessHost， V47中新增的mediaHost同样的流程；

在每个进程Connect来的时候，同步所有的config到对应的子进程中....

向render进程发送设置可以通过render_process_host_impl.cc 发送 ChildProcessMsg_xxx 个对应的render进程；

有静态函数RenderProcessHost::AllHostIterator();可以拿到所有的render_process_host 来发消息




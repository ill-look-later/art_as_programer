# Qt wenegine的实现结构

页面绘制完成，比如说一个css动画触发的绘制，当绘制完成Chromium render进程会发送IPC消息到Browser这边；在RenderWidgetHostViewQt对象中通过复写`virtual void OnSwapCompositorFrame(uint32_t output_surface_id, scoped_ptr<cc::CompositorFrame> frame)`函数， 在函数中触发qtquickitem【QML】或者qtwebenginewidget【QTWIDGET】对象的update；update函数就是安排重绘；

### void QQuickItem::update\(\)

Schedules a call to [updatePaintNode](http://doc.qt.io/qt-5/qquickitem.html#updatePaintNode)\(\) for this item.

The call to [QQuickItem::updatePaintNode](http://doc.qt.io/qt-5/qquickitem.html#updatePaintNode)\(\) will always happen if the item is showing in a [QQuickWindow](http://doc.qt.io/qt-5/qquickwindow.html).

Only items which specify [QQuickItem::ItemHasContents](http://doc.qt.io/qt-5/qquickitem.html#Flag-enum) are allowed to call QQuickItem::update\(\).

在重绘函数`updatePaintNode`中将冲render中交换回来的数据对象ChromiumCompositorData通过`DelegatedFrameNode::commit`完成数据的交换，经过commit函数，所有chromium绘制的数据都交换到了QSGNode对象中；QSGNode在`updatePaintNode`中返回给qt的绘制系统，与qtwidget/qtquickitem对象继续合成并显示；
这里的



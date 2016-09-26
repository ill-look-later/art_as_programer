# Qt wenegine的实现结构

### void QQuickItem::update()

Schedules a call to [updatePaintNode](http://doc.qt.io/qt-5/qquickitem.html#updatePaintNode)() for this item.

The call to [QQuickItem::updatePaintNode](http://doc.qt.io/qt-5/qquickitem.html#updatePaintNode)() will always happen if the item is showing in a [QQuickWindow](http://doc.qt.io/qt-5/qquickwindow.html).

Only items which specify [QQuickItem::ItemHasContents](http://doc.qt.io/qt-5/qquickitem.html#Flag-enum) are allowed to call QQuickItem::update().
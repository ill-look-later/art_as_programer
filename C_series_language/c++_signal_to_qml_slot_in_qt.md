# C++ SIGNAL to QML SLOT in Qt

在qt的MetaObject的支持下, 我们可以方便的完成QML和C++的交互, 而QML在C++中没一个版本的变化都挺大的. 有很多中方法来支持C++和QML的交互; 具体可以参考QT的官方文档,{吐槽一下: 现在qt的文档越来越跟不上版本和代码的变更了}

[QT Documents](http://doc.qt.io/qt-5/qtqml-cppintegration-interactqmlfromcpp.html)


这里讲到的是我们如何将我们C++类中得信号,导出到QML中, 按照Qt官方的说法,在MetaObject的支持向, 理论上不管是C++　还是QML中得信号, 都是可以connect的, 可这是理论上呀{哭晕...};
看下面的代码:
```javascript
Item {
  objectName: ""
  Connections {
      target: MetaObject // namely QOBJECT 
      onAxxxsignal: {
        objectName = sinal_arguments
      }
  }
}
```
一个直接的问题, 如果这个metaobject是一个c++对象, 在qml中如何拿到这个对象呢? 在最新的Qt5.1+的版本中, 用子对象QQmlApplicationEngine代替了QQmlEngine, 这个QQmlApplicationEngine变成了整个qml执行环境的完整上下文, 它继承了QObject, 
QQmlEngine,QJSEngine; 通过它可以取到qml对象的QQmlContext,RootObject等等对象;
具体参考:[](http://doc.qt.io/qt-5/qqmlapplicationengine.html)

要想在QML中访问C++对象:
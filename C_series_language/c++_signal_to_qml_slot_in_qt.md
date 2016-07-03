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
一个直接的问题, 如果这个metaobject是一个c++对象, 在qml中如何拿到这个对象呢? 在最新的Qt5.1+的版本中, 用子类QQmlApplicationEngine代替了QQmlEngine, 这个QQmlApplicationEngine变成了整个qml执行环境的完整上下文, 它继承了QObject, QQmlEngine, QJSEngine; 通过它可以取到qml对象的QQmlContext,RootObject等等对象;
具体参考:[](http://doc.qt.io/qt-5/qqmlapplicationengine.html)

要想在QML中访问C++对象, 按照下面的步骤即可将QObject对象暴露到qml中:

- 1. 实现自己的类, 必须继承自Qobject类;这是Qt本身机制所必须得
- 2. 通过QQmlApplicationEngine 获取到 QQmlContext对象, 它提供了直接的借口来get到这个上下文环境
- 3. 通过Context()->setContextProperty("YourObject", myOb);来将你的对象暴露给QML
- 4. 在qml中直接使用YourObject对象即可;

  > 信号和槽本身是整个qt中qml 和 qt/C++ 共同的机制, 所以一旦将你设计的类暴露出去之后, 你的类所拥有的信号和槽本身就已经暴露出去了; QML中直接使用 YourObject.yoursignal(); YourObject.youSlot(); 都是可以的;当然也可以使用信号来连接到其他对象[不管是c++的还是qml的]槽上, 或者将其他信号连接到yourobject的slot上

所以上面说的这些就可以用下面的代码来实现:
```c++
sinal c++Signal(QString singalString);
...
QString objproperty = "XReaderContext";
QQmlContext* qml_context = m_engine->rootContext();
qml_context->setContextProperty(objproperty,this);```

```javascript
//qml中
Connections {
    target: XReaderContext 
    onC++Signal: {
        qmlString = signalString
    }
}
```

同样的, 如果你需要将qml对象的信号, 连接到C++ Object的slot上, 可以在这个模块OnLoadCompleted:中利用 signale.conect(C++object, slotfunction)来完成绑定

参考: 
http://stackoverflow.com/questions/8834147/c-signal-to-qml-slot-in-qt
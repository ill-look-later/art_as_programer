# Event Binding to UI action

绑定event到类成员函数；

- step 01： 在storyboard ui控件对应的UIcontroller类中新建 处理函数，并将函数通过@IBAction 标示；
  
  > ![](QQ20160507-0.png)

- step 2:
  -  右击UI控件在Send envent选项中将相应地event事件拖拽到对应的ViewController上， 在弹出的函数选择框上选择需要绑定的函数； 
  - 另外一个办法就是在相应的viewcontroller上右侧工具栏中选择connections inspects， 按住响应函数拖到UI控件上，之后在弹出来的事件选择框中，选择事件
  ![](xxxxxxx.png)
  
  ---
  
  
  
  # 将新的viewcontroller 绑定到swift 类
  
  1. 在storyboard中拖入新的uiviewcontroller， 拖动并设置为相应的present 方式；
  ![](QQ20160507-1.png)
  
  2. 在项目中新建一个cocoatouch 类，基类为新的ui控件的基类， 上图中我们选择的时uiviewcontroller 所以我们在新建swift类的时候选择基类为UIViewController；

  3. 在storyboard中选择新建的viewcontroller， 在右侧属性栏中得class上填上我们的新的类类名即可完成binding
  
  4. 运行， 点击第一页面的按钮， 就会跳转到我们新建的uiviewcontroller中
    ![](current_status.gif)
# Draw API的使用和页面跳转上一点点的优# Draw API的使用和页面跳转上一点点的优na化


　　参看别人的做的， 但是那个博客中是通过静态cell，和viewcontroller进行连接的， 也就是一个cell 会connect 到一个storyboard中拖入的viewcontroller， 对于我这种内存恐惧症还是有蛮大的伤害的， 所以我自己改了改，新手、勿喷！

- ####storyboard 中是这样的

|![](QQ20160509-0.png)|
| :--: | 


- ####运行成功后是这样子嘀

|![](jupm.gif)|
| :--: |


- ####很容易看明白吧， 就是在页面跳转的时候，通过函数将tableview中得数据传递到下一个要显示的page上。看代码：不得不赞一个apple公司在这函数和api的说明上写的非常cool

```swift
      // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        // Get the new view controller using segue.destinationViewController.
        // Pass the selected object to the new view controller.
        let new_view = segue.destinationViewController as! PaletteViewController
        
        if let indexPath = self.tableView.indexPathForSelectedRow {
            switch indexPath.row {
            case 0:
                new_view.type = DrawType.Line
            case 1:
                new_view.type = DrawType.Rectangle
            case 2:
                new_view.type = DrawType.Circle
            default:
                new_view.type = nil
            }
            //let object : NSDictionary = listVideos[indexPath.row] as NSDictionary
            //(segue.destinationViewController as JieDetailViewController).detailItem = object
        }
    }
```


#### 在接收的那边， 收到类型之后根据不同的type绘制不同的形状就ok了,新建一个类binding到第二个页面中得view上；在drawapi来绘制不同的形状
```swift
import UIKit

class CanvasView: UIView {

    var drawtype:DrawType? = nil
    // *
    // Only override drawRect: if you perform custom drawing.
    // An empty implementation adversely affects performance during animation.
    override func drawRect(rect: CGRect) {
        print("I will Draw a \(drawtype!) here in size:width:\(rect.width),height:\(rect.height)")
        // Drawing code
    }
    //*/

    func drawLines() {
        print("i will draw lines here, but i gona sleep now")
    }
    func drawRectangle() {
        print("i will draw Rectangle here, but i gona sleep now")
    }
    func drawCircle() {
        print("i will draw Circle here, but i gona sleep now")
    }
    
}
```

---

### 再来看绘图 在ios中叫 core graphics


有时间先看看这篇文章：http://www.cocoachina.com/industry/20140115/7703.html，  看完之后应该不用说不会写代码了吧..(*^__^*) 嘻嘻……，

下面是其核心代码部分, 其中drawtype是通过prepareForSegue 函数传递过来的. 其中绘图的上下文通过
*"UIGraphicsGetCurrentContext"* 拿到, 这个上下文类似于javascript在浏览器中拿到的canvas.

其中core graphics 提供了一系列的函数来在特定的函数来完成基本的绘制
- CGPathMoveToPoint //将路径移动到一个点
- CGContextAddPath // 为上下文(我更愿意称为canvas
- CGContextClosePath
- CGPathMoveToPoint
- CGContextMoveToPoint
- CGPathAddLineToPoint
- CGContextStrokePath
- CGContextSetRGBStrokeColor
- CGContextAddArc
- CGContextSetLineWidth
- CGContextFillPath
- CGContextAddEllipseInRect
- CGContextDrawImage
- CGContextAddEllipseInRect
- CGContextSetRGBFillColor
- CGContextSaveGState
- CGContextRestoreGState
- ....

这一系列的基本函数来帮助我们完成图形的绘制,

```swift
    override func drawRect(rect: CGRect) {
        print("I will Draw a \(drawtype!) here in size:width:\(rect.width),height:\(rect.height)")
        if (nil == drawtype) {
            return
        }
        switch drawtype! {
        case DrawType.Line:
            self.drawLines()
        case DrawType.Rectangle:
            self.drawRectangle()
        case DrawType.Circle:
            self.drawCircle()
        case DrawType.Image:
            drawImage()
        case DrawType.Figure:
            PaiterBoard();
        }
        // Drawing code
    }
    
    
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        //let p = (touches as NSSet).anyObject()?.locationInview(self)
        let p = touches.first?.locationInView(self)
        CGPathMoveToPoint(path, nil, (p?.x)!, (p?.y)!)
        
        //(path, nil, touches.locationInView(self), )
    }
    
    override func touchesMoved(touches: Set<UITouch>, withEvent event: UIEvent?) {
        let p = touches.first?.locationInView(self)
        CGPathAddLineToPoint(path, nil, (p?.x)!, (p?.y)!)
        setNeedsDisplay();// 安排一次重绘
    }
    
    //*/
    func PaiterBoard() {
        print("i will draw when i move")
        let context = UIGraphicsGetCurrentContext()
        CGContextAddPath(context, path)
        CGContextStrokePath(context)
    }

    func drawLines() {
        print("i will draw lines here, but i gona sleep now")
        let context = UIGraphicsGetCurrentContext()
        CGContextMoveToPoint(context, 100, 100)
        CGContextAddLineToPoint(context, 100, 200)
        CGContextAddLineToPoint(context, 200, 200)
        
        CGContextSetRGBStrokeColor(context, 255, 0, 0, 1)
        CGContextStrokePath(context)
        
        
        //如果前面已经调用了 绘制的函数，比如CGContextStrokePath 下面就会失败，
        //相当于一个path已经被绘制了，就不在了
        CGContextClosePath(context) //当前path若不是闭合的，这个函数会添加一条直线让当前path闭合
        CGContextFillPath(context)
    }
    func drawRectangle() {
        let context = UIGraphicsGetCurrentContext()
        CGContextAddRect(context, CGRect(x: 110, y: 110, width: 40, height: 40))
        CGContextStrokePath(context)
        
        // 没有用，这里是fill，CGContextSetRGBStrokeColor(context, 255, 0, 0, 1)
        CGContextSetRGBFillColor(context, 255, 0, 0, 0.8)
        CGContextFillRect(context, CGRect(x: 200, y: 200, width: 40, height: 40))
        
        print("i will draw Rectangle here, but i gona sleep now")
    }
    func drawCircle() {
        let context = UIGraphicsGetCurrentContext()
        CGContextAddArc(context, 200, 200, 100, 0, 2*3.14, 0) //0: 顺时针, 1: 逆时针
        CGContextSetLineWidth(context, 6)
        CGContextStrokePath(context)
        CGContextAddArc(context, 200, 200, 90, 0, 2*3.14, 0) //0: 顺时针, 1: 逆时针
        CGContextFillPath(context) //填充
        
        //椭圆的方式来绘制圆
        CGContextAddEllipseInRect(context, CGRect(x: 100, y: 400, width: 100, height: 100))
        CGContextFillPath(context)
        
        //椭圆
        CGContextAddEllipseInRect(context, CGRect(x: 100, y: 600, width: 100, height: 50))
        CGContextFillPath(context)
        
        print("i will draw Circle here, but i gona sleep now")
    }
    
    func drawImage() {
        // UIImage(contentsOfFile: "") 这个借口不能用assets里面的资源,
        let img:UIImage? = UIImage(named: "xxx.png")
        let cgimg:CGImage? = img?.CGImage
        let context = UIGraphicsGetCurrentContext() //保存绘制前的状态
        CGContextSaveGState(context)//保存状态
        CGContextTranslateCTM(context, 10, 400)
        CGContextDrawImage(context, CGRect(x:100,y:100,width:300,height:300), cgimg)
        
        CGContextRestoreGState(context) //恢复之前的状态
    }

```


---

| ——**我就是code小兵，code小兵就是我**  |
| --: |




















































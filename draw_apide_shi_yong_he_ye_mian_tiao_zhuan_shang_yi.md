# Draw API的使用和页面跳转上一点点的优化

####参看别人的做的， 但是那个博客中是通过静态cell，和viewcontroller进行连接的， 也就是一个cell 会connect 到一个storyboard中拖入的viewcontroller， 对于我这种内存恐惧症还是有蛮大的伤害的， 所以我自己改了改，新手、勿喷！

- ####storyboard 中是这样的

| |![](QQ20160509-0.png)| |
| -- | -- | -- |


- ####运行成功后是这样子嘀

|  |![](jupm.gif)| |
| -- | -- | -- |


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
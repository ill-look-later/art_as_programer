
 And I love the developers, who spend their valuable private time creating amazing things, which then they share with other people and don’t want anything in return. **Open source authors and contributors, you are awesome people.** Thank you for all your work.

 ---
 So **from my daily work on** [**my own apps**](https://www.eclerstudios.com/), here I’ve selected favorites from my favorites iOS open source libraries. Order of these projects is totally random, all of them are simply awesome.
 The overwhelming majority of the libraries support [CocoaPods](https://cocoapods.org/), so adding them to your Xcode project is a breeze.
 **On the bottom of the article you will find a TL;DR version **— a simple list with only titles and links to the projects. If you’ll find this article useful, **share it with your iOS dev buddies. Good things need to spread.**
 ## 1. DZNEmptyDataSet
 This should be a standard, built-in into iOS way of dealing with empty table and collection views. By default if your table view is empty, the screen is empty. It’s not the best user experience you can have.
 With this library you just need to conform to a few protocols and iOS will beautifully take care of your collection view and display proper, good looking to user messages. No brainer for every iOS project.
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*-GPQJ5GrZGlo560Vjn6ueQ.png)


 #### CocoaPods:

         pod 'DZNEmptyDataSet'

 [**DZNEmptyDataSet**
 _DZNEmptyDataSet - A drop-in UITableView/UICollectionView superclass category for showing empty datasets whenever the…_github.](https://github.com/dzenbot/DZNEmptyDataSet "https://github.com/dzenbot/DZNEmptyDataSet")[](https://github.com/dzenbot/DZNEmptyDataSet)
 ## 2. PDTSimpleCalendar
 Need a simple, nice looking and working calendar component for your app? Now you have — PDTSimpleCalendar is probably the best calendar component for iOS. You can customize it in many ways, both working logic and looking.

 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*py6WgEruAWaYGGG0xxgRwg.png)

 #### CocoaPods:

         pod 'PDTSimpleCalendar'

 [**jivesoftware/PDTSimpleCalendar**
 _PDTSimpleCalendar - A simple Calendar / Date Picker for iOS using UICollectionView_github.com](https://github.com/jivesoftware/PDTSimpleCalendar "https://github.com/jivesoftware/PDTSimpleCalendar")[](https://github.com/jivesoftware/PDTSimpleCalendar)
 ## 3. MagicalRecord
 _Core Data is simple,_ they said. _It’s nice and simple,_ they said. __Huh, really, Apple? A ton of boilerplate code added to each project isn’t very elegant and simple. Not to mention adding, removing and updating a lot of entities, saving context, creating different Core Data stacks for different environments etc, etc. I like Core Data very much of course, but Apple _really_ could simplify it in a little better way —** the MagicalRecord way.**
 MagicalRecord works like a wrapper for Core Data and hides from developer all non-relevant stuff. If you’ve ever worked with active record pattern (e.g. Ruby on Rails), you’re in home. Really, really recommended library if you are using Core Data in your app.
 #### CocoaPods:

         pod 'MagicalRecord'

 [**magicalpanda/MagicalRecord**
 _MagicalRecord - Super Awesome Easy Fetching for Core Data 1!!!11!!!!1!_github.com](https://github.com/magicalpanda/MagicalRecord "https://github.com/magicalpanda/MagicalRecord")[](https://github.com/magicalpanda/MagicalRecord)
 ## 4. Chameleon
 If you are reading this, odds that you’re a better programmer than a designer are very high. This is something for you.
 
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*bjdhJDlCZJtya6bAI5938g.png)

 Chameleon is a color framework for iOS. It extends UIColor with beautiful, modern flat colors. It also gives us ability to create color palletes from color defined by us. It can do many other things, explore readme. If you want beautiful application, definitely add this library to your project.

 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*sMgYah4wuGhN3IQsWtG_7A.png)
 

 #### CocoaPods:

     pod 'ChameleonFramework'

 [**ViccAlexander/Chameleon**
 _Chameleon - Flat Color Framework for iOS (Obj-C & Swift)_github.com](https://github.com/ViccAlexander/Chameleon "https://github.com/ViccAlexander/Chameleon")[](https://github.com/ViccAlexander/Chameleon)
 ## 5. Alamofire
 Alamofire is an elegant networking library written in Swift. Have you ever been using AFNetworking? Alamofire is it’s younger brother. Younger and more stylish, of course (AFNetworking is written in Objective-C).
 
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*qkOCEhp5g9ixArBdLDwPyw.png)

 Need to do networking stuff like downloading, uploading, getting JSONs etc.? Alamofire is for you. 8000 people on GitHub cannot be wrong.
 #### CocoaPods:

     pod 'Alamofire'

 [**Alamofire/Alamofire**
 _Alamofire - Elegant HTTP Networking in Swift_github.com](https://github.com/Alamofire/Alamofire "https://github.com/Alamofire/Alamofire")[](https://github.com/Alamofire/Alamofire)
 ## 6. TextFieldEffects
 Don’t you think that standard UITextField is a little boring? Me too — so **say hello to TextFieldEffects!** I won’t write too much, I’ll just show you a few examples what this library can do:
![](https://d262ilb51hltx0.cloudfront.net/max/800/1*U1VSW9jDKKWF2yYlbhQ-Ug.gif)
 
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*Nws8VPi9br54Ae-tCwLXGA.gif)
 
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*lZ7TWmt36U9eBNdoxmyjhA.gif)

 Yep, **these are simple drop-in controllers.** You can even make use from IBDesignables in storyboard!
 #### CocoaPods:

         pod 'TextFieldEffects'

 #### Carthage:
 
         github "raulriera/TextFieldEffects"

 [**raulriera/TextFieldEffects**
 _TextFieldEffects - Custom UITextFields effects inspired by Codrops, built using Swift_github.com](https://github.com/raulriera/TextFieldEffects "https://github.com/raulriera/TextFieldEffects")[](https://github.com/raulriera/TextFieldEffects)
 ## 7. GPUImage
 Have you ever created a camera app? **If not, you surely will after meeting this library.**
 <figure name="b74c" id="b74c" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*s7tpFPCnKV8YZ7qdCVZyqg.png)
 <figcaption class="imageCaption">GPUImage possibilities.</figcaption>
 </figure>
 GPUImage provides us a GPU-accelerated camera effects (both images and video) with blazing speed. There are hundreds of apps in the App Store that use this library — and one mine’s too:
 <figure name="448c" id="448c" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*o5JTFxgggZUYCvGZm6kh1g.png)
 <figcaption class="imageCaption">GPUImage in use in one of my apps.</figcaption>
 </figure>
 8869 stars on GitHub and still counting.
 #### CocoaPods:
 <pre name="574e" id="574e" class="graf--pre graf-after--h4">
 pod 'GPUImage'
 </pre>
 [**BradLarson/GPUImage**
 _GPUImage - An open source iOS framework for GPU-based image and video processing_github.com](http://github.com/BradLarson/GPUImage "http://github.com/BradLarson/GPUImage")[](http://github.com/BradLarson/GPUImage)
 ## 8. iRate
 What’s the best way to get more reviews in the App Store? I don’t have hard data to answer that question, but if I had to guess, I would say that **simple asking the user.** Maybe it’s a little oldschool way to do this— most developers now create custom in-app alerts — but if you don’t have time or you don’t want to implement everything from scratch, it’s better to use iRate than not to. And this is iRate exactly —** a small library that you include in your project and forget about asking users for review — iRate will do it for you, at proper time.**
 #### CocoaPods:
 <pre name="4a72" id="4a72" class="graf--pre graf-after--h4">
 pod 'iRate'
 </pre>
 [**nicklockwood/iRate**
 _iRate - A handy class that prompts users of your iPhone or Mac App Store app to rate your application after using it…_github.com](https://github.com/nicklockwood/iRate "https://github.com/nicklockwood/iRate")[](https://github.com/nicklockwood/iRate)
 ## 9. GameCenterManager
 Love or hate singletons, but in this case managing Game Center is **just easier** with a little help of our best known anti-pattern (you _have only one Game Center_ in your game, right?).
 <figure name="ee84" id="ee84" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*t1Sy5Jt2fAaeU1VaAeEjbQ.png)
 </figure>
 To be honest, _vanilla-managing_ Game Center in iOS isn’t that hard, **but with this library is just simple and fast.** And better is the enemy of the good.
 <figure name="74db" id="74db" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*P3bj6yW7GnnT0yTt0RdboQ.png)
 </figure>
 I’m using this in one of my games and it’s a pleasure experience.
 #### CocoaPods:
 <pre name="4393" id="4393" class="graf--pre graf-after--h4">
 pod 'GameCenterManager'
 </pre>
 [**nihalahmed/GameCenterManager**
 _GameCenterManager - iOS Game Center helper singleton_github.com](https://github.com/nihalahmed/GameCenterManager "https://github.com/nihalahmed/GameCenterManager")[](https://github.com/nihalahmed/GameCenterManager)
 ## 10. **PKRevealController 2**
 This is a real gem here, **one of my most favorited iOS control.** PKRevealController is a slideable side menu (left, right or both), which slides with a help of your finger (or just by pressing the button, but it’s not as much cool as sliding).
 <figure name="6a7f" id="6a7f" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*uM5nPOq0tbieJBYMC0f1jg.png)
 </figure>
 I’ve tried a few other libraries providing this kind of control and PKRevealController is just the best. Very easy to setup, highly customizable and recognizes gestures very, very well. It could be included in iOS SDK as a standard control, really.
 #### CocoaPods:
 <pre name="e5b8" id="e5b8" class="graf--pre graf-after--h4">
 pod 'PKRevealController'
 </pre>
 [**pkluz/PKRevealController**
 _Introducing PKRevealController 2 - The second version of one of the most popular view controller containers for iOS…_github.com](https://github.com/pkluz/PKRevealController "https://github.com/pkluz/PKRevealController")[](https://github.com/pkluz/PKRevealController)
 ## 11. SlackTextViewController
 Have you ever used Slack iOS app? If you are working in a bigger software company, probably yes. For these people who haven’t — Slack rocks. And Slack’s iOS app too, especially for the great, custom text input control… which here you have — a code ready for use in your app!
 **Self growing text area? Check.Gestures recognizing, autocompletion, multimedia pasting? Check. Easy drop-in solution? Check.** What else can you possibly need?
 #### CocoaPods:
 <pre name="a80f" id="a80f" class="graf--pre graf-after--h4">
 pod 'SlackTextViewController'
 </pre>
 [**slackhq/SlackTextViewController**
 _SlackTextViewController - A drop-in UIViewController subclass with a growing text input view and other useful messaging…_github.com](https://github.com/slackhq/SlackTextViewController "https://github.com/slackhq/SlackTextViewController")[](https://github.com/slackhq/SlackTextViewController)
 ## 12. RETableViewManager
 RETableViewManager will help you with dynamically creating and managing your table views, everything in code. It deliver us predefined cells (for bools, texts, dates etc. — check screenshots below), but you can also create your custom views and use them along with the default ones.
 <figure name="7d86" id="7d86" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*Tq-kGojYO7NjWWa2R7pKcQ.png)
 <figcaption class="imageCaption">Left screenshot is so oldschool!</figcaption>
 </figure>
 All of this stuff you can do in storyboard without help of this library, but sometimes code is simply better than visual editor.
 #### CocoaPods:
 <pre name="4f09" id="4f09" class="graf--pre graf-after--h4">
 pod 'RETableViewManager'
 </pre>
 [**romaonthego/RETableViewManager**
 _RETableViewManager - Powerful data driven content manager for UITableView._github.com](https://github.com/romaonthego/RETableViewManager "https://github.com/romaonthego/RETableViewManager")[](https://github.com/romaonthego/RETableViewManager)
 ## 13. PermissionScope
 Useful library to deliver better user experience by informing user about needed system permissions **before** asking user for them. Higher acceptance rate -> more users actively using the app -> better retention -> better stats -> more downloads. Highly recommended pod.
 <figure name="6818" id="6818" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*lCNOzpkOzDj-WUFbUITtQA.png)
 </figure>
 #### CocoaPods:
 <pre name="9ceb" id="9ceb" class="graf--pre graf-after--h4">
 pod 'PermissionScope'
 </pre>
 [**nickoneill/PermissionScope**
 _PermissionScope - A Periscope-inspired way to ask for iOS permissions_github.com](https://github.com/nickoneill/PermissionScope "https://github.com/nickoneill/PermissionScope")[](https://github.com/nickoneill/PermissionScope)
 ## 14. SVProgressHUD
 This image **is** loaded properly, don’t wait longer and don’t refresh the page. **This is exactly how SVProgressHUD looks like in your app.** If you need custom waiting indicator, here you have (the best probably) one.
 <figure name="8d03" id="8d03" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*nUWClDbqfsNzRqFx6jhA3g.gif)
 </figure>
 #### CocoaPods:
 <pre name="b5fb" id="b5fb" class="graf--pre graf-after--h4">
 pod 'SVProgressHUD'
 </pre>
 [**TransitApp/SVProgressHUD**
 _SVProgressHUD - A clean and lightweight progress HUD for your iOS app._github.com](https://github.com/TransitApp/SVProgressHUD "https://github.com/TransitApp/SVProgressHUD")[](https://github.com/TransitApp/SVProgressHUD)
 ## 15. FontAwesomeKit
 **Font Awesome is awesome** and with this library you can easily add the font to your project and use it in many ways.
 <figure name="a2e3" id="a2e3" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*QYbVsOA96NsUxSCrQ_CIUw.jpeg)
 </figure>
 #### CocoaPods:
 <pre name="81d1" id="81d1" class="graf--pre graf-after--h4">
 pod 'FontAwesomeKit'
 </pre>
 [**PrideChung/FontAwesomeKit**
 _FontAwesomeKit - Icon font library for iOS. Currently supports Font-Awesome, Foundation icons, Zocial, and ionicons._github.com](https://github.com/PrideChung/FontAwesomeKit "https://github.com/PrideChung/FontAwesomeKit")[](https://github.com/PrideChung/FontAwesomeKit)
 ## 16. SnapKit
 Love auto layout? You should!
 _At least when creating it in storyboards._
 Creating constraints in code is painful without some help, but luckily SnapKit is here and with it on board you can code your constraints in easy, declarative way. Check it out.
 <figure name="c9c5" id="c9c5" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*PVelVchmwY6kHZ9FMGuJiQ.png)
 </figure>
 #### CocoaPods:
 <pre name="ca72" id="ca72" class="graf--pre graf-after--h4">
 pod 'SnapKit'
 </pre>
 [**SnapKit/SnapKit**
 _SnapKit - A Swift Autolayout DSL for iOS & OS X_github.com](https://github.com/SnapKit/SnapKit "https://github.com/SnapKit/SnapKit")[](https://github.com/SnapKit/SnapKit)
 ## 17. MGSwipeTableCell
 Another UI component, that is so often seen in many apps that Apple should probably think about including something similar in standard iOS SDK. **Swipeable table cell**, this is the best description of this pod. The best one.
 <figure name="1f65" id="1f65" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*g9U2AX7pYXm5QY0_rBKOUQ.gif)
 </figure>
 <figure name="f565" id="f565" class="graf--figure graf-after--figure">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*5ZLSg0LU_vA-qiV8CIakFA.gif)
 </figure>
 <figure name="59bf" id="59bf" class="graf--figure graf-after--figure">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*j4GvOd6JFS93U2kx2vkCLQ.gif)
 </figure>
 These are just 3 animation types, there are more of them. Explore readme.
 #### CocoaPods:
 <pre name="17e5" id="17e5" class="graf--pre graf-after--h4">
 pod 'MGSwipeTableCell'
 </pre>
 [**MortimerGoro/MGSwipeTableCell**
 _MGSwipeTableCell - An easy to use UITableViewCell subclass that allows to display swippable buttons with a variety of…_github.com](https://github.com/MortimerGoro/MGSwipeTableCell "https://github.com/MortimerGoro/MGSwipeTableCell")[](https://github.com/MortimerGoro/MGSwipeTableCell)
 ## 18. Quick
 Unit testing in Swift, for Swift (ok, for Objective-C too), integrated with Xcode. If you are Objective-C fan, I would recommend [Specta](https://github.com/specta/specta) instead of this, but for Swift Quick will be probably the best shot.
 <figure name="07e1" id="07e1" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*slKaci9ZuZRTUjDzgi3-SA.png)
 </figure>
 <figure name="2406" id="2406" class="graf--figure graf-after--figure">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*ZMm8Jgzv9VTPi3v7cxc4KA.png)
 </figure>
 #### CocoaPods:
 <pre name="52c1" id="52c1" class="graf--pre graf-after--h4">
 pod 'Quick'
 </pre>
 [**Quick/Quick**
 _Quick - The Swift (and Objective-C) testing framework._github.com](https://github.com/Quick/Quick "https://github.com/Quick/Quick")[](https://github.com/Quick/Quick)
 ## 19. IAPHelper
 In-app purchases brings us a lot of boilerplate code, which this library get rid of and give us a simple wrapper for most common tasks related to money transfer from iOS user to your (or your company) wallet.
 #### CocoaPods:
 <pre name="2115" id="2115" class="graf--pre graf-after--h4">
 pod 'IAPHelper'
 </pre>
 [**saturngod/IAPHelper**
 _IAPHelper - in app purchases helper for iOS_github.com](https://github.com/saturngod/IAPHelper "https://github.com/saturngod/IAPHelper")[](https://github.com/saturngod/IAPHelper)
 ## 20. ReactiveCocoa
 OK, here we have a little monster.
 <figure name="c5f9" id="c5f9" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*ZGn-IDtnbbN7ryiXwpNQTg.png)
 </figure>
 ReactiveCocoa isn’t a rather small, drop-in project like the others on this list. **ReactiveCocoa brings us a totally different programming style/architecture based on signals and streams of values.** It’s totally mind blowing and first you need unlearn what you’ve learned to understand how this work. It’s not an easy task, but rewarding.
 This isn’t a proper place to teach you ReactiveCocoa, but I’ll give you good resources if you are interested:
 [**Getting Started with ReactiveCocoa**
 _Note: This is going to be a slightly more technical post geared toward our friends in the iOS developer community. In…_www.teehanlax.com](http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/ "http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/")[](http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/)
 [**ReactiveCocoa**
 _Languages are living works. They are nudged and challenged and bastardized and mashed-up in a perpetual cycle of…_nshipster.com](http://nshipster.com/reactivecocoa/ "http://nshipster.com/reactivecocoa/")[](http://nshipster.com/reactivecocoa/)
 [**ReactiveCocoa Tutorial - The Definitive Introduction: Part 1/2**
 _As an iOS developer, nearly every line of code you write is in reaction to some event; a button tap, a received network…_www.raywenderlich.com](http://www.raywenderlich.com/62699/reactivecocoa-tutorial-pt1 "http://www.raywenderlich.com/62699/reactivecocoa-tutorial-pt1")[](http://www.raywenderlich.com/62699/reactivecocoa-tutorial-pt1)
 #### CocoaPods:
 <pre name="2559" id="2559" class="graf--pre graf-after--h4">
 pod 'ReactiveCocoa'
 </pre>
 [**ReactiveCocoa/ReactiveCocoa**
 _ReactiveCocoa - A framework for composing and transforming streams of values_github.com](https://github.com/ReactiveCocoa/ReactiveCocoa "https://github.com/ReactiveCocoa/ReactiveCocoa")[](https://github.com/ReactiveCocoa/ReactiveCocoa)
 ## 21. SwiftyJSON
 JSON parsing in Swift made easy.
 #### CocoaPods:
 <pre name="4c86" id="4c86" class="graf--pre graf-after--h4">
 pod 'SwiftyJSON'
 </pre>
 [**SwiftyJSON/SwiftyJSON**
 _SwiftyJSON - The better way to deal with JSON data in Swift_github.com](https://github.com/SwiftyJSON/SwiftyJSON "https://github.com/SwiftyJSON/SwiftyJSON")[](https://github.com/SwiftyJSON/SwiftyJSON)
 ## 22. Spring
 Animations made easy, chainable and declarative.
 <figure name="953e" id="953e" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*J0QzPXBCDAs6d5MS9DPMpg.jpeg)
 </figure>
 #### CocoaPods:
 <pre name="0bb2" id="0bb2" class="graf--pre graf-after--h4">
 pod 'Spring'
 </pre>
 [**MengTo/Spring**
 _Spring - A library to simplify iOS animations in Swift._github.com](https://github.com/MengTo/Spring "https://github.com/MengTo/Spring")[](https://github.com/MengTo/Spring)
 ## 23. FontBlaster
 Load custom fonts to your app easily.
 #### CocoaPods:
 <pre name="053a" id="053a" class="graf--pre graf-after--h4">
 pod 'FontBlaster'
 </pre>
 [**ArtSabintsev/FontBlaster**
 _FontBlaster - Programmatically load custom fonts into your iOS app._github.com](https://github.com/ArtSabintsev/FontBlaster "https://github.com/ArtSabintsev/FontBlaster")[](https://github.com/ArtSabintsev/FontBlaster)
 ## 24. TAPromotee
 Cross promoting your apps is one of the best marketing strategies you can implement in them for free. And with this library it’s so easy that you can’t longer justify not doing it — add TAPromotee to your podfile, configure and enjoy more downloads for free.
 <figure name="4c54" id="4c54" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*FF4dr7V617RSqlrUhuvr7Q.png)
 </figure>
 #### CocoaPods:
 <pre name="3fd8" id="3fd8" class="graf--pre graf-after--h4">
 pod 'TAPromotee'
 </pre>
 [**JanC/TAPromotee**
 _TAPromotee - Objective-C library to cross promote iOS apps_github.com](https://github.com/JanC/TAPromotee "https://github.com/JanC/TAPromotee")[](https://github.com/JanC/TAPromotee)
 ## 25. Concorde
 Do you load a lot of JPEGs in your app? With Concorde you can do it in a bit better looking way. A progressive way.
 <figure name="9488" id="9488" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*2UdNaQS15wesjGjjiHAvaQ.gif)
 </figure>
 #### CocoaPods:
 <pre name="ccdf" id="ccdf" class="graf--pre graf-after--h4">
 pod 'Concorde'
 </pre>
 [**contentful-labs/Concorde**
 _Concorde - Download and decode progressive JPEGs on iOS._github.com](https://github.com/contentful-labs/Concorde "https://github.com/contentful-labs/Concorde")[](https://github.com/contentful-labs/Concorde)
 ## 26. KeychainAccess
 Little helper library to manage Keychain access.
 <figure name="18e7" id="18e7" class="graf--figure graf--layoutOutsetRow is-partialWidth graf-after--p" style="width: 33.333%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*uLid5pu3_m0YQIptfim5jg.png)
 </figure>
 <figure name="ffcf" id="ffcf" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 33.333%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*lfVh0XU6GaSr-irD8xeb8A.png)
 </figure>
 <figure name="8bcb" id="8bcb" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 33.333%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*tEIFKi-dRQW7FkohYgELAw.png)
 </figure>
 #### CocoaPods:
 <pre name="bcff" id="bcff" class="graf--pre graf-after--h4">
 pod 'KeychainAccess'
 </pre>
 [**kishikawakatsumi/KeychainAccess**
 _KeychainAccess - Simple Swift wrapper for Keychain that works on iOS and OS X_github.com](https://github.com/kishikawakatsumi/KeychainAccess "https://github.com/kishikawakatsumi/KeychainAccess")[](https://github.com/kishikawakatsumi/KeychainAccess)
 ## 27. iOS-charts
 And last but not least — the iOS charts library! It’s so useful and beautiful, that I won’t write here too much —** just scroll below and see what you can do in your app with this project.**
 <figure name="bf39" id="bf39" class="graf--figure graf-after--p">
 ![](https://d262ilb51hltx0.cloudfront.net/max/800/1*VbtAD_1hxOldYxjJx6vASA.png)
 </figure>
 <figure name="f0c0" id="f0c0" class="graf--figure graf--layoutOutsetRow is-partialWidth graf-after--figure" style="width: 29.772%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*WaQkU2dRwNzqMEZunyqNAQ.png)
 </figure>
 <figure name="f56a" id="f56a" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 28.55%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*OeGTvNnJUqBIehn1wjIbpg.png)
 </figure>
 <figure name="f835" id="f835" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 41.678%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/600/1*sDI562oJKyDVGq49BjMeYA.png)
 </figure>
 <figure name="684a" id="684a" class="graf--figure graf--layoutOutsetRow is-partialWidth graf-after--figure" style="width: 32.472%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*3Nn3WU5YSg26_8vPQZ2rDQ.png)
 </figure>
 <figure name="5015" id="5015" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 32.553%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*pOqvE_3UQPfkepplZA2zqA.png)
 </figure>
 <figure name="43b7" id="43b7" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 34.975%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*RSiBbr-v5YXod6MQNDXz2g.png)
 </figure>
 <figure name="38aa" id="38aa" class="graf--figure graf--layoutOutsetRow is-partialWidth graf-after--figure" style="width: 36.478%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*BCtAfcEAjrU1KW-IfG7jmA.png)
 </figure>
 <figure name="e7ac" id="e7ac" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 52.044%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/600/1*IPF0dEGyvDLsw4pzASPxaA.png)
 </figure>
 <figure name="e9ec" id="e9ec" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 11.478%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/200/1*vHUAaOByjBHfLfo2x1Z6yw.png)
 </figure>
 <figure name="4b24" id="4b24" class="graf--figure graf--layoutOutsetRow is-partialWidth graf-after--figure" style="width: 50.166%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/600/1*JMvTfSBPIFj-ganbyCPTzg.png)
 </figure>
 <figure name="61d4" id="61d4" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 49.834%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/600/1*bC4jLpWVEqhxGRXbdv_2xw.png)
 </figure>
 <figure name="cf56" id="cf56" class="graf--figure graf--layoutOutsetRow is-partialWidth graf-after--figure" style="width: 80.074%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/1000/1*AAcJvh9nb4BETcE4f6g9lw.png)
 </figure>
 <figure name="b5dd" id="b5dd" class="graf--figure graf--layoutOutsetRowContinue is-partialWidth graf-after--figure" style="width: 19.926%;">
 ![](https://d262ilb51hltx0.cloudfront.net/max/400/1*YfdosQop9CLvd1cMV7JsXw.png)
 </figure>
 Yes, everything is available as a drop-in (ok, maybe “code-in”) component.
 Unfortunately there is no CocoaPods support yet, so you need to manually drag the project to your Xcode workspace.
</section>
<section name="29cd" class=" section--body">
 ---
 ### **TL;DR list of all these libraries for quick access:**
 1. [DZNEmptyDataSet](https://github.com/dzenbot/DZNEmptyDataSet) [UI, empty table view solver]
 2. [PDTSimpleCalendar](https://github.com/jivesoftware/PDTSimpleCalendar) [UI, drop-in calendar component]
 3. [MagicalRecord](https://github.com/magicalpanda/MagicalRecord) [Core Data helper implementing active record pattern]
 4. [Chameleon](https://github.com/ViccAlexander/Chameleon) [UI, color framework]
 5. [Alamofire](https://github.com/Alamofire/Alamofire) [Swift networking]
 6. [TextFieldEffects](https://github.com/raulriera/TextFieldEffects) [UI, custom looking text fields]
 7. [GPUImage](https://github.com/BradLarson/GPUImage) [fast image processing]
 8. [iRate](https://github.com/nicklockwood/iRate) [getting user ratings]
 9. [GameCenterManager](https://github.com/nihalahmed/GameCenterManager) [easily manage Game Center]
 10. [PKRevealController](https://github.com/pkluz/PKRevealController) [UI, slide side menu]
 11. [SlackTextViewController](https://github.com/slackhq/SlackTextViewController) [UI, highly customizable custom text field]
 12. [RETableViewManager](https://github.com/romaonthego/RETableViewManager) [create table views dynamically from code]
 13. [PermissionScope](https://github.com/nickoneill/PermissionScope) [UI, nicely pre-asking user for system permissions]
 14. [SVProgressHUD](https://github.com/TransitApp/SVProgressHUD) [UI, custom waiting spinner]
 15. [FontAwesomeKit](https://github.com/PrideChung/FontAwesomeKit) [easily add Font Awesome to your project]
 16. [SnapKit](https://github.com/SnapKit/SnapKit) [easy auto layout in code]
 17. [MGSwipeTableCell](https://github.com/MortimerGoro/MGSwipeTableCell) [UI, swipeable table view cells]
 18. [Quick](https://github.com/Quick/Quick) [Swift unit testing framework]
 19. [IAPHelper](https://github.com/saturngod/IAPHelper) [In-App Purchases helper wrapper]
 20. [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) [[FRP](http://en.wikipedia.org/wiki/Functional_reactive_programming) framework]
 21. [SwiftyJSON](https://github.com/SwiftyJSON/SwiftyJSON) [Swift JSON library]
 22. [Spring](https://github.com/MengTo/Spring) [Animation framework]
 23. [FontBlaster](https://github.com/ArtSabintsev/FontBlaster) [easily load custom fonts into your app]
 24. [TAPromotee](https://github.com/JanC/TAPromotee) [cross promote your apps with drop-in view]
 25. [Concorde](https://github.com/contentful-labs/Concorde) [download and decode progressive JPEGs]
 26. [KeychainAccess](https://github.com/kishikawakatsumi/KeychainAccess) [manage keychain easily]
 27. [iOS-charts](https://github.com/danielgindi/ios-charts) [beautiful charts library]
</section>
<section name="deb4" class=" section--body section--last">
 ---
 Thanks for reading, it was a long list! If you think it was worth creating, **please share it by clicking the _Share_ button below the article **— more people will benefit from it**.** Also if you’re a Medium user, **please click the _Recommend_ button** — it will inspire me to create more iOS development articles!
</section>


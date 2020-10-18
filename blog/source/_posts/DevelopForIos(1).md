---

date: 2020/4/8
tags:
- ios
categories:
- development
- ios
---
## Stack view (get the Auto layout contraints)

library ->stackview (horizontal/vertegal doesen't matter

* drag the elements into the stackview and change the ***Aligment*** and ***Distribution*** in the property bar.  

we can use the stack view to manage many elements and their layout.

*change the **backgroud** of button to give it texture*

* add ***horizantally in container*** and ***vertically in container***

  ![](https://img.cetacis.dev/uploads/big/bef4430bb6422d782a0f03e1e5cb6ad9.png)

  

*everything have to have constrains*

* when we use the imageview to set the background, we'd uncheck the ***constraint margins*** and set the 0 to the ***view***(not the safe area). 

  Set the ***Content Mode*** to Aspect fill.

* ***Content Compression Resistance Priority***: decide the priority of whether the picture will be compressed (default for 750)

![resistance for priority](https://img.cetacis.dev/uploads/big/dc59c99442768426ad4f8bffc968975a.png)

​	***Content Hugging Priority***: the same when the zone is too big.

<!-- more -->

## Vary layout

* when change the iphone to the horizontal, maybe them will be a strange issure

  ```
  an internal error occured
  ```

  So, **Save the project **before the change and after the issure, project->clean the build folder, then alt+Q to quit xcode and relaunch it.

* Making the exiting stack view and an laber(or other elements) to be a new stackview is possible. And xcode can make them be oriented. 

**The button is near the Add constrains button**

* add the certain constrains to the certain device. 

![](https://img.cetacis.dev/uploads/big/6f6cdb5be94afdba4d1c01d511771bc5.png)

Size class : wC hC ; wC hR; wR hR (C: compact R:regular)

### **vary  for traits** 

the in the certain size class's stack view's Spacing, click the + and add the variation.

* when want to link to element, hold the ***control*** and the element and make a blue line link to the other element

![](https://img.cetacis.dev/uploads/big/888c8a380b4d5b34df6dc089ad3b9a03.png)

* Lable should link to other elements in three or four directions and the other element should have higher hugging prority the lable so that only the lable will be stretch to fill up any remaining space.

## Swift

### basic

let: constant  var: variable

```swift
var i = 32 
var i:Int = 32
var u:Int? // optionals
var g = abs(-1) // absolute value
var h = ceil(1.8)  // get the top number
var i = floor(1.4)  //get the bottom number
var j = sqrt(36)  // Square root
var k = pow(2,4) //square
```

```swift
func myFunction(name:String, age:Int) { 
	print("Here's \(name), age \(age)")
}

func addFourTo(number:Int) -> Int {
	return number + 4
}
```

```swift
import UIKit

class Spaceship {
    
    var fuelLevel = 100
    var name = ""
    
    func cruise() {
        // Code to initiate cruising
        print("Cruising is initiated for \(name)")
    }
    
    func thrust() {
        // Code to initiate rocket thrusters
        print("Rocket thrusters initiated for \(name)")
    }
    
}

var myShip:Spaceship = Spaceship()
myShip.name = "Tom"
print(myShip.name)
print(myShip.fuelLevel)

```

```swift
import UIKit

let x = 4
let y = 6

if (x >= 11 || y == 6) && x + y == 10{
    print("Hello")
}
else if x > 5 {
    print("Good afternoon")
}
else if x <= 2 {
    
}
else if x != 2 {
    
}
else {
    
}

import UIKit

func vowelDetector (str:String) -> Bool {
    
    if str.contains("a") ||
        str.contains("e") ||
        str.contains("i") ||
        str.contains("o") ||
        str.contains("u") {
        
        return true
    }
    else {
        return false
    }
    
    
}

print (vowelDetector(str: "hello"))
print (vowelDetector(str: "sync"))
```

#### [Array](https://developer.apple.com/documentation/swift/array)

#### **optionals**  可nil可object

nil is like none 

optional 存在是因為swift要求開發者必須規定一個變量是否為可nil變量，如果聲明了一個optioanl變量，我們必須在調用他的時候驗證這個是否是nil。驗證方法有兩種：

```swift
var myInt:Int? // 聲明optionals 
string
if myInt !=  nil{
  print(str!)// ！表示輸出str的內容，否則會輸出Oprional(myInt)
}
if myInt == nil{
  //show the wrong message
}
// 2. optional binding 
if let realint = str{
  print(realint)
}//當str == nil，什麼都不輸出。當str != nil 輸出string
```

也有不需要驗證的方法, ! 告訴xcode聲明的變量可能是nil也可能是object，但是這個行為是不安全的。這種變量不是optional，但是可以存儲nil。

```swift
var str:String!
str = nil
print(str)
```

#### Dictionary

* Key word 是optional 輸出對應的data時應該考慮驗證optional

![](https://img.cetacis.dev/uploads/big/9310c9a8fdefaad1f911c5a082f43367.png)

![](https://img.cetacis.dev/uploads/big/4b93703ed1a4428a89c34bbbc6fce4f9.png)

**LOOP**

![](https://img.cetacis.dev/uploads/big/d134398bd729fe1960acdbddbfba0a66.png)

**隨機生成數列** 

* for _ 
* Arc4random_uniform

![隨機生成數列](https://img.cetacis.dev/uploads/big/4fcdf201e3264267e3355c353bc91efc.png)

**while loop1**

![while生成無重複數列](https://img.cetacis.dev/uploads/big/ef122ebd9bbcbd6a09d0e7c66f1bbccd.png)

**while loop 2 **

```swift
while ..{}
```





### inherit

![整體的繼承層次](https://img.cetacis.dev/uploads/big/21a22ee715d2cc1cde94d1d47e19b387.png)

## ViewControler.swift

### 將elements hook到.swift中，綁定action/outlet



control+drag = blue line  IB outlet

And we can enter the viewDigLoad to change the properties of Outlet

![](https://img.cetacis.dev/uploads/big/cdf7c03eefed1f31d82dd0a3a5d6ceeb.png)

![change the information](https://img.cetacis.dev/uploads/big/999fed4da9ebbc9421fe065bbe546f7f.png)

And the button can be link to action. The type of the connection is Action  （IB Action）

![給elemnet創建action：touch up inside可以改成其他的觸發事件](https://img.cetacis.dev/uploads/big/93d405ab9e4002a032ede3b3f4b21d69.png)

![](https://img.cetacis.dev/uploads/big/e8ece4ca49579b1cc0c725492ceb1f43.png)

## Ueser Interaction

![IB Action](https://img.cetacis.dev/uploads/big/e96bbd072a11b4dc4a3649bb74b8b9e7.png)

###  ***random***

如果想讓左右的牌同時變並且隨機出現不同的點數，可以採用隨機數。

![隨機數](https://img.cetacis.dev/uploads/big/3b33e11f8ae4ca31a55e65812d2a0fcb.png)

## buildup actual combat

### MVC

view : User use 

Controller: between view and model

Model: data and logic

database ——model——model——view

優點：可複用性；代碼簡潔條理清晰

Toryboard : view

ViewController.swift : controller

### 1. 在storyboard中建立collectionview 拖入素材照片並設置collection view cell 的identity 為CardCell custom class 為 CardCollectionViewCell

此遊戲橫向，不可縱向 因此取消勾選portrait

![規定橫向](https://img.cetacis.dev/uploads/big/4728bbb5766920205583b5e352b81b48.png)

這個將屏幕分成一個一個cell，規定位置時與safe area關聯，這樣可以防止重要的元素被遮擋。

根據asset的大小調整cell的大小

![](https://img.cetacis.dev/uploads/big/d21da7b6d76fc0a2373ecb7ee3287a6b.png)

可以給這裡的conllection View Cell 起個名字，把它當做模板。

![](https://img.cetacis.dev/uploads/big/e240e8c9028c2a2ce742831a907f9b13.png)

### 2. 创建CardModel.swift (創建新的卡牌array), Card.swift（存放card 的數據結構）

创建CardModel.swift (創建新的卡牌array), Card.swift（存放card 的數據結構）

```swift
CardModel.swift
import Foundation

class CardModel{
    func getCards()->[Card]{
        //Declare an array to store generated cards
        var generatedCardsArray = [Card]()
        var unique = [UInt32]()
       
        
        //Randomly generate pairs of cards
        repeat{
            let random = arc4random_uniform(13)+1
            if unique.contains(random){
                continue
            }
            unique.append(random)
            let card1 = Card()
            card1.imageName = "card\(random)"
            generatedCardsArray.append(card1)
            let card2 = Card()
            card2.imageName = "card\(random)"
            generatedCardsArray.append(card2)
            //print(random)
        }while generatedCardsArray.count<16
        
        //Randomize the array
        
        //Return the array
        return generatedCardsArray
        
    }
}
```

```swift
Card.swift

import Foundation

class Card{
    var imageName = ""
    var isFlipped = false
    var isMatched = false 
}
```



然后初始化cardarray

![在viewcontrol中調用](https://img.cetacis.dev/uploads/big/19198b70a3b531275c431dcd97c4febe.png)

### 3. 協議操作

#### protocol and delegate



在viewcontrol.swift中，我們知道可以通過viewcontrol.swift 改變 view，但這個交流是單向的，即viewcontrol可以向view傳遞信息，但是如果view想要請求得來自viewcontrol的信息（比如通過代碼來直接佈局view，或者在用戶拖動屏幕時，view會請求更新，但是他並不知道viewcontrol類的存在，我們需要解決這個問題），則只能通過 protocol（協議）和delegate（代理）

*代理个人理解是一种设计模式,OC中代理的模式是通过Protocol来实现的,指的是让其他类去实现遵守的协议中的方法.在本类中再调用这个方法,从而达到代理的目的. 比如A这个类想有一个方法,但是不想去实现这个方法,那么就找到B,B去实现A的这个方法,然后A再调用这个方法,这样A就成功的委托B去实现方法,达到代理的目的.*

我們知道 label, text,button 都是ui kidds classes，他們遵守一些協議，即擁有一些協議方法和協議屬性（可以理解成在專門的類中實現的方法和屬性），我們可以在viewcontrol中調用這些方法，以此來修改 label，button等。

![](https://img.cetacis.dev/uploads/big/ea8a7c1bc297f3086bd189a04f296e15.png)

相反的，button，text都不知道viewcontrol的存在。

#### An analogy

警察 需要調用不同車輛處理對應事件 他向警局申請，警局派出特定車輛

警察不知道警局是誰在回應他，但是他只要告訴電話裡的那一端的人事件對應的代號，警局就可以發出特定的車輛。這個代號就存在協議中，而警局就相當於代理（delegate），相當於警察想要完成的事情（調用車輛），讓警局完成。

換一種更接近代碼語言的類比，officer想要調用method A() , 而method A()的實現在代理類中實現，officer之所以可以直接請求method A() （或者說officer之所以知道代理中有這個類，是因為在協議中標明了應該有類和屬性）

如下

![](https://img.cetacis.dev/uploads/big/1e6e3a49a31208a1814a66d810c27e14.png)

![](https://img.cetacis.dev/uploads/big/351641441be0afba4059c2580aa1cf97.png)

#### 举一个工程中的例子

將一個collection hook 到viewcontrol中。得到：

```swift
@IBOutlet weak var mycollectionView: UICollectionView!
```

我們知道，collection存在在UICollectionView這個類中。這個類遵守某個定義好的協議。當我們點進這個類的簡介，我們可以看到他所處的協議和代理的方式。datasource用來獲得數據，delegate用於事件相應等方法。

![](https://img.cetacis.dev/uploads/big/159367c6cad4afc1a2c21b0e67c3dc33.png)

再點入某個協議，可以得知這個協議中規定的方法。

在工程中，我們常常將viewController當做我們的代理類以及數據來源類，來實現協議中規定的方法和數據。使用者類本身的定義是在庫函數中（即collectionview），但是實例化則是在viewcontroller中（@IBOutlet weak var mycollectionView: UICollectionView!），實例化成mycollectionView後，給使用者類中定義的協議delegate和dataSource賦值（即使用者類所在的協議會在使用者類中被實例化為delegate和dataSource，通過賦值給代理類可以完成連接。類似於之前警察警局例子中的o.radio = b）。這個時候，代理類中就可以定義會自動執行的函數啦（我還沒想明白為什麼會自動執行這些函數，為什麼沒有沒有mycollection.XXX這樣的，但是這個函數定義的本身就很奇怪，先用吧）

*”使用者類“ （我自己啟的名字，我覺得將他比作協議的使用者比較合適）*

```swift
import UIKit

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {
    
    @IBOutlet weak var myTableView: UITableView!
    let dataArray = ["bird", "dog", "cat", "turtle", "bear"]
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        myTableView.delegate = self
        myTableView.dataSource = self
        
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        
        return dataArray.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        // Get the data for this row
        let rowData = dataArray[indexPath.row]
        
        // Get a cell to display
        let cell = tableView.dequeueReusableCell(withIdentifier: "BasicCell", for: indexPath)
        cell.textLabel?.text = rowData
        
        // Return the cell
        return cell
    }

}
```

#### 本cardgame中的協議操作

```swift

import UIKit

class ViewController: UIViewController,UICollectionViewDataSource,UICollectionViewDelegate {
    var model = CardModel()
    var cardArray = [Card]()
    @IBOutlet weak var collectionView: UICollectionView!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        collectionView.delegate = self;
        collectionView.dataSource = self;
        
        cardArray = model.getCards()
    }
    
    //COLLECTIONVIEW ask it's delegate to calculate the sums of items in the collection
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return cardArray.count;
    }
    
    //ask the datasource for the new data for actually for each individual cell that need to be dispalyed
    // cell for item in that indexPath
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CardCell", for: indexPath)as! CardCollectionViewCell;
        // use the exiting cell to repeat called by identity
        return cell;
    }
    
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        
        //when tap on the certain cell
    }

}


```





如果需要定義一個底層的class 用swift file；反之，用cocoa



特定的標識在注釋裡可以在整體函數預覽時可見 



```swift
// TODO: 
// MARK: 
```



editor ->structure->In-dent 可以自動糾正格式

### tip debug

po double check the certain value and properties in the project 

![debug](https://img.cetacis.dev/uploads/big/c4d211865b0b189909dda4c6b0a7576c.png)

![可以給斷點設置觸發條件](https://img.cetacis.dev/uploads/big/ad95a71e728913a09e3bdc094174ef31.png)

擁有可視化層次

![擁有可視化層次](https://img.cetacis.dev/uploads/big/d84587ccd9cf579db4d0df09548e215e.png)

在檢查code之前先檢查ui



接着之前的工程。现在总结一下之前要做的

* 在storyborad里建立collectionview 然后拖进两个image。将其cardcell确定custom class= CardCollectionViewCell和identify = CardCell。（cardcell是CardCollectionViewCell的一个实例）
* 建立card.swift(card的数据结构)和cardModel.swift(返回card array)
* 在viewcontroller中，进行协议的操作（具体如上）
* 建立CardCollectionViewCell.swift对于设定的custom class进行操作

### 4. 建立CardCollectionViewCell.swift对于设定的custom class进行操作

#### custom class



創建cocoa touch class 

![img](https://img.cetacis.dev/uploads/big/09a957c8fb1a982b3bd6ae64f042e938.png)

將cardcell的custom class修改成cardcollectionviewcell。這樣就可以在ccvc里用code控制cell中的圖片了

![img](https://img.cetacis.dev/uploads/big/3f883a8305b30cd8de923f4c9fbe896b.png)

將image hook到ccvc中，就可以控制了

我们设置这个class 叫做 CardCollectionViewCell ，他是UICollectionViewCell的子类 ，将frontImage和backImagine hook进来

*小tip： func name(_ type parameter){} 中的_ 代表形式參數是可缺省的*

```swift
// CardCollecionViewCell.swift 
import UIKit

class CardCollectionViewCell: UICollectionViewCell {
    
    @IBOutlet weak var frontImageView: UIImageView!
    
    @IBOutlet weak var backImageView: UIImageView!
    
    var card:Card?
    
    func setCard(_ card:Card){
        self.card = card
        frontImageView.image = UIImage(named: card.imageName)
        // we have to call the setCard method for each of cells
        
    }
    func flip(){
        
    }
    func flipback(){
        
    }
    
 }

```

所以，viewcontroller(){}中協議方法的返回應該是CardCollectionViewCell類型。

```swift
//viewcontroller.swift
import UIKit

class ViewController: UIViewController,UICollectionViewDataSource,UICollectionViewDelegate {
    var model = CardModel()
    var cardArray = [Card]()
    @IBOutlet weak var collectionView: UICollectionView!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        collectionView.delegate = self;
        collectionView.dataSource = self;
        
        cardArray = model.getCards()
    }
    
    //COLLECTIONVIEW ask it's delegate to calculate the sums of items in the collection
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return cardArray.count;
    }
    
    //ask the datasource for the new data for actually for each individual cell that need to be dispalyed
    // cell for item in that indexPath
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        
        // get a cardcollectionviewcell object
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CardCell", for: indexPath)as! CardCollectionViewCell;
        
       //get the card that the collectionview is trying to display
        let card = cardArray[indexPath.row]
        
        //set the card for the cell
        cell.setCard(card)
        
        return cell;
    }
    
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        
        //when tap on the certain cell
    }

}


```

現在已經實現了每一個cell都有了自己的圖畫（根據生成的cardarray）

現在要定義flip的方法 

在CardCollectionViewCell.swift中加上

```swift
func flip(){
        UIView.transition(from: backImageView, to: frontImageView, duration: 0.3, options: [.transitionFlipFromLeft,.showHideTransitionViews], completion: nil)
    }
```

並且修改viewController.swift 的didSelectItemAt函數

```swift

    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        // get the cell which is clicked
        let cell = collectionView.cellForItem(at: indexPath) as! CardCollectionViewCell
        
        
        // get the card which is clicker
        let card = cardArray[indexPath.row]
            
      	//flip the card
        if card.isFlipped == false{
            cell.flip()
        }else{
            cell.flipback()
        }
       
        //when tap on the certain cell
    }

}
```

現在運行後便可以正常翻轉了，但是這時候當我們翻轉後，將屏幕下拉，等到拉回去的時候，就會發現翻開的不是原來那兩個了。這是因為每次出現這樣的情況，我們的setCard的func會重置，抹去之前的印記。

修改CardCollectionViewCell.swift中的set

```swift
  func setCard(_ card:Card){
        self.card = card
        frontImageView.image = UIImage(named: card.imageName)
        // we have to call the setCard method for each of cells
        
        //determine if the card is in a flipped up state or flipped down state.
        if card.isFlipped == true{
            //Make sure the frontview is on top
            UIView.transition(from: backImageView, to: frontImageView, duration: 0.3, options: [.transitionFlipFromLeft,.showHideTransitionViews], completion: nil)
        }else{
            // make sure the backview is on top
            UIView.transition(from: frontImageView, to: backImageView, duration: 0.3, options: [.transitionFlipFromLeft,.showHideTransitionViews], completion: nil)
        }
        
    }

```

### 5. Game Logic and optinization

可以使牌正常地翻轉，初始化、動畫和事件已經綁定完成，接下來就要控制遊戲邏輯了。

在每次翻到牌時，調用checkForMatch()，因此需要修改didSelectItemAt()

```swift
   func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        // get the cell which is clicked
        let cell = collectionView.cellForItem(at: indexPath) as! CardCollectionViewCell
        
        
        // get the card which is clicker
        let card = cardArray[indexPath.row]
        
        if card.isFlipped == false{
            cell.flip()
            card.isFlipped = true
            
            if firstFlippedCardIndex == nil{
                firstFlippedCardIndex = indexPath
            }else{
                // TODO: the flipped card is the second card, we will use the game logic
              checkForMatch(indexPath)
            }
        }
        //when tap on the certain cell
    }
```



检查翻到牌时是否匹配

```swift
vc.swift
    var firstFlippedCardIndex : IndexPath?
// MARK: Game logic methods.
    func checkForMatch(_ secondFlippedCardIndex:IndexPath){
        //Get two cells
        let cellOne = collectionView.cellForItem(at: firstFlippedCardIndex!) as? CardCollectionViewCell
        let cellTwo = collectionView.cellForItem(at: secondFlippedCardIndex) as? CardCollectionViewCell
        
        //Get two cards
        let cardOne = cardArray[firstFlippedCardIndex!.row];
        let cardTwo = cardArray[secondFlippedCardIndex.row];
        
        //Compeare the two card
        if cardOne.imageName == cardTwo.imageName{
            //matched
            //set the status of the cards
            cardOne.isMatched = true
            cardTwo.isMatched = true
            //remove the cards
            cellOne?.remove()
            cellTwo?.remove()
            
        }else{
            cardOne.isFlipped = false
            cardTwo.isFlipped = false
            cellOne?.flipback()
            cellTwo?.flipback()
        }
        firstFlippedCardIndex = nil
    }
```

这里的remove() 实在ccvc.swift中定義得

```swift
func remove() {
        frontImageView.alpha = 0
        backImageView.alpha = 0
        
    }
```

这个时候会发现，因为flipback没有停留时间，如果一样，会立即消失，如果不同，则会立即返回，现在我们需要暂时的停顿。修改ccvc.swift的flipback函数

```swift
func flipback(){
        DispatchQueue.main.asyncAfter(deadline: DispatchTime.now()+0.3){
            UIView.transition(from: self.frontImageView, to: self.backImageView, duration: 0.3, options: [.transitionFlipFromLeft,.showHideTransitionViews], completion: nil)}
    }
    
```

同时消失快我们可以通过改变remove()来解决（添加动画）

```swift
func remove() {
        
        backImageView.alpha = 0
        
        UIView.animate(withDuration: 0.3, delay: 0.5, options: .curveEaseOut, animations:
            {self.frontImageView.alpha = 0}, completion: nil)
        
    }
```

这时候还有一个问题，当纸牌翻消失，他并不是真的消失，他只是将alpha通道调为0，cell还在，所以再次点击还是会执行flip操作。因此我们应该给didSelectItemAt执行加一个条件，就是

card.isFlipped == false && card.isMatched == false

```swift
 func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        // get the cell which is clicked
        let cell = collectionView.cellForItem(at: indexPath) as! CardCollectionViewCell
        
        
        // get the card which is clicker
        let card = cardArray[indexPath.row]
        
        if card.isFlipped == false && card.isMatched == false{
            cell.flip()
            card.isFlipped = true
            
            if firstFlippedCardIndex == nil{
                firstFlippedCardIndex = indexPath
            }else{
               checkForMatch(indexPath)
            }
        }
        //when tap on the certain cell
    }
```

并且，我们的setcard执行也需要这样的条件，当这个被翻开或者已经匹配过 就不会进行setcard操作了.同時，當card.isMatched == false 時，我們要注意不能讓這些alpha=0的被重用，導致未匹配的卡消失。

```swift
func setCard(_ card:Card){
        self.card = card
        frontImageView.image = UIImage(named: card.imageName)
        // we have to call the setCard method for each of cells
        if card.isMatched == true{
            backImageView.alpha = 0
            frontImageView.alpha = 0
          	return
        }else{
          backImageView.alpha = 1
          frontImageView.alpha = 1
        }
        //determine if the card is in a flipped up state or flipped down state.
        if card.isFlipped == true{
            //Make sure the frontview is on top
            UIView.transition(from: backImageView, to: frontImageView, duration: 0.3, options: [.transitionFlipFromLeft,.showHideTransitionViews], completion: nil)
        }else{
            // make sure the backview is on top
            UIView.transition(from: frontImageView, to: backImageView, duration: 0.3, options: [.transitionFlipFromLeft,.showHideTransitionViews], completion: nil)
        }
        
    }
```

當cardOne滑出屏幕，他的index會丟失，因此我們應該當cardOne == nill 重新調用 index


# Factory Method 工廠模式

## Intent 目標

Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

工廠模式是一種創建型設計模式，其在父 Class 中提供一個創建物件的方法，允許子 Class 決定實例化物件的類型。

## Problem 問體

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the `Truck` class.

想像你正在開發一款物流管理應用程式。最初版本只能處理 Truck 運輸，因此大部分程式碼都在位於名為 `Truck` Class 中。

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

一段時間後，你的這款應用程式變得極熱門，每天你都能收到十幾次來自海運公司的需求，希望應用程式能夠支援海上運輸功能。

![factory-method](https://refactoring.guru/images/patterns/diagrams/factory-method/problem1-en.png)

Adding a new class to the program isn’t that simple if the rest of the code is already coupled to existing classes.

如果程式碼其餘部分與現有 Class 已經存在耦合關係，那麼像程式中增加新的 Class 其實沒有那麼容易。

Great news, right? But how about the code? At present, most of your code is coupled to the `Truck` class. Adding `Ships` into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

這可是個好消息，但是程式碼問題該如何處理呢？目前大部分程式碼都與 `Truck` Class 相關。在程式中新增 `Ships` Class 需要修改全部程式碼。更糟糕的是，如果你以後需要再程式碼中支援另一種運輸方式，很可能需要再次對這些程式碼進行大幅修改。

As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app’s behavior depending on the class of transportation objects.

最後，你將不得不編寫繁雜的程式碼，根據不同的運輸物件 Class，在應用程式中運行不同的處理。

## Solution 解決方案

The Factory Method pattern suggests that you replace direct object construction calls (using the new operator) with calls to a special _factory_ method. Don’t worry: the objects are still created via the new operator, but it’s being called from within the factory method. Objects returned by a factory method are often referred to as _products_.

工廠模式建議使用特殊的 _factory_ method 代替直接呼叫物件建構函數(使用 `new` 運算符)。不用擔心，這些物件仍然能透過 `new` 運算符建立，只是該運算符改在工廠模式中調用而已。工廠模式返回的物件被稱作 _product_"

At first glance, this change may look pointless: we just moved the constructor call from one part of the program to another. However, consider this: now you can override the factory method in a subclass and change the class of products being created by the method.

乍看之下，這個改變可能看起來毫無意義：我們只是改變了程式中呼叫建構函數的位置而已。然而，思考這個：你現在可以在子 class 中覆蓋 factory method，從而改變其建立 products 的 class

There’s a slight limitation though: subclasses may return different types of products only if these products have a common base class or interface. Also, the factory method in the base class should have its return type declared as this interface.

不過有一點需要注意：只有在這些 products 有一個共同的 base class 或是 interface 的時候，subclasses 才能回傳不同類型的 products。此外，base class 中的 factory method 應該將其回傳類型宣告作為一個 interface。

For example, both Truck and Ship classes should implement the Transport interface, which declares a method called deliver. Each class implements this method differently: trucks deliver cargo by land, ships deliver cargo by sea. The factory method in the RoadLogistics class returns truck objects, whereas the factory method in the SeaLogistics class returns ships.

舉例來說，`Truck` 和 `Ship` classes 都必須實現 `Transport` interface，其中宣告了一個名為 `deliver` 的 method。每個 class 都用不同的 method 來實現：卡車在陸地上交付貨物、船隻在海上交付貨物。`RoadLogistics` class 中的 factory method 回傳 truck `物件，SeaLogistics` class 則是回傳 ship 物件

The code that uses the factory method (often called the client code) doesn’t see a difference between the actual products returned by various subclasses. The client treats all the products as abstract Transport. The client knows that all transport objects are supposed to have the deliver method, but exactly how it works isn’t important to the client.

使用 factory method 的程式碼(通常稱作 client code) 不用暸解不同 subclass 回傳的實際 products 之間的差異。每個 client 將所有的 products 是為抽象的 `Transport`。Client 知道所有 transport 物件都提供了 deliver method，但不需要暸解具體的實現方法。

## Applicability 適用場景

- Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.

  當在編寫程式碼的過程中，如果無法預知物件的確切類別和其相依關係時，可使用工廠模式。

  The Factory Method separates product construction code from the code that actually uses the product. Therefore it’s easier to extend the product construction code independently from the rest of the code.

  工廠模式將建立商品的程式碼與實際使用產品的程式碼分離，從而達到能在不影響其他程式碼的情況下擴展商品建立部分程式碼。

  For example, to add a new product type to the app, you’ll only need to create a new creator subclass and override the factory method in it.

  例如，如果需要在應用程式中添加一個新產品，你只需要開發新的建立者 subclass，然後覆蓋其 factory method 即可。

## How to Implement

## Pros and Cons

### Pros

- You avoid tight coupling between the creator and the concrete products.
  避免 creator 和具體 products 之間的緊密耦合
- Single Responsibility Principle. You can move the product creation code into one place in the program, making the code easier to support.
  單一職權原則，你可以將 product 建立程式碼放在程式的單一位置，使得程式碼更容易維護
- Open/Closed Principle. You can introduce new types of products into the program without breaking existing client code.
  開關原則，不用再更改現有 client code 的情況下，就可以在程式中導入新的 product 類型

### Cons

- The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.
  當需要引入許多新的 subclass 的時候，其程式碼可能會因此變得複雜許多。最好的情況是該模式引入 creator class 的現有階層架構中

## Relations with Other Patterns

- Many designs start by using Factory Method (less complicated and more customizable via subclasses) and evolve toward Abstract Factory, Prototype, or Builder (more flexible, but more complicated).
  在許多的設計初期都會使用 Factory Method(較為簡單且更方便的使用 subclass 做訂製)，隨後演化使用 Abstract Factory、Prototype 或 Builder(更靈活但更複雜)

- Abstract Factory classes are often based on a set of Factory Methods, but you can also use Prototype to compose the methods on these classes.
  Abstract Factory classes 通常基於一組 Factory Methods，但也可以使用 Prototype 來組成這些 classes 的 methods

- You can use Factory Method along with Iterator to let collection subclasses return different types of iterators that are compatible with the collections.
  可以同時使用 Factory Method 和 Iterator 來讓 subclass 集合回傳不同類型的 iterators 並使其與集合相匹配

- Prototype isn’t based on inheritance, so it doesn’t have its drawbacks. On the other hand, Prototype requires a complicated initialization of the cloned object. Factory Method is based on inheritance but doesn’t require an initialization step.
  Prototype 並不基於繼承，因此沒有其缺點。另一方面，Prototype 需要對被複製物件進行複雜的初始化。Factory Method 則是基於繼承，但是不需要初始化步驟

- Factory Method is a specialization of Template Method. At the same time, a Factory Method may serve as a step in a large Template Method.
  Factory Method 是 Template Method 的一種特殊形式。同時間，Factory Method 可以作為一個大型 Template Method 中的一的步驟

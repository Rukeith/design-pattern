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

## Applicability 適用場景

* Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.

  當在編寫程式碼的過程中，如果無法預知物件的確切類別和其相依關係時，可使用工廠模式。

  The Factory Method separates product construction code from the code that actually uses the product. Therefore it’s easier to extend the product construction code independently from the rest of the code.

  工廠模式將建立商品的程式碼與實際使用產品的程式碼分離，從而達到能在不影響其他程式碼的情況下擴展商品建立部分程式碼。

  For example, to add a new product type to the app, you’ll only need to create a new creator subclass and override the factory method in it.

  例如，如果需要在應用程式中添加一個新產品，你只需要開發新的建立者 subclass，然後覆蓋其 factory method 即可。

* Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.

  當你希望使用者能擴展你的 Library 或 framework 的內部元件時，可使用工廠模式。

  Inheritance is probably the easiest way to extend the default behavior of a library or framework. But how would the framework recognize that your subclass should be used instead of a standard component?

  繼承可能是擴展 Library 或 Framework 預設行為上最簡單的方式。但是當你使用 subclass 替換標準元件時，Framework 如何辨識出該 subclass?

The solution is to reduce the code that constructs components across the framework into a single factory method and let anyone override this method in addition to extending the component itself.

Let’s see how that would work. Imagine that you write an app using an open source UI framework. Your app should have round buttons, but the framework only provides square ones. You extend the standard Button class with a glorious RoundButton subclass. But now you need to tell the main UIFramework class to use the new button subclass instead of a default one.

To achieve this, you create a subclass UIWithRoundButtons from a base framework class and override its createButton method. While this method returns Button objects in the base class, you make your subclass return RoundButton objects. Now use the UIWithRoundButtons class instead of UIFramework. And that’s about it!

 Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.

 You often experience this need when dealing with large, resource-intensive objects such as database connections, file systems, and network resources.

Let’s think about what has to be done to reuse an existing object:

First, you need to create some storage to keep track of all of the created objects.
When someone requests an object, the program should look for a free object inside that pool.
… and then return it to the client code.
If there are no free objects, the program should create a new one (and add it to the pool).
That’s a lot of code! And it must all be put into a single place so that you don’t pollute the program with duplicate code.

Probably the most obvious and convenient place where this code could be placed is the constructor of the class whose objects we’re trying to reuse. However, a constructor must always return new objects by definition. It can’t return existing instances.

Therefore, you need to have a regular method capable of creating new objects as well as reusing existing ones. That sounds very much like a factory method.

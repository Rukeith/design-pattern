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

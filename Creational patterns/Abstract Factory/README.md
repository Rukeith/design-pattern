# Abstract Factory 抽象工廠模式

## Intent 目標

Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

抽象工廠模式是一種創建型設計模式，能建立一系列相關的物件，而無需指定其具體的類別。

## Problem 問題

Imagine that you’re creating a furniture shop simulator. Your code consists of classes that represent:

假設你正在開發一個家具商模擬器，你的程式碼中包含了一些 Class，用於表示：

1. A family of related products, say: `Chair` + `Sofa` + `CoffeeTable`.

   一系列相關產品，例如：椅子 `Chair`、沙發 `Sofa` 和咖啡桌 `Coffee­Table`

2. Several variants of this family. For example, products `Chair` + `Sofa` + `CoffeeTable` are available in these variants: `Modern`, `Victorian`, `ArtDeco`.

   系列產品的不同類型。例如，你可以使用 `Modern`、`Victorian`、​`Art­Deco` 等風格生成 `Chair` + `Sofa` + `CoffeeTable`。

You need a way to create individual furniture objects so that they match other objects of the same family. Customers get quite mad when they receive non-matching furniture.

你需要設法單獨生成每件家具物件，這樣才能確保其風格一致，如果顧客收到的家格不同，他們可能會不開心。

Also, you don’t want to change existing code when adding new products or families of products to the program. Furniture vendors update their catalogs very often, and you wouldn’t want to change the core code each time it happens.

此外，你也不希望再添加新產品或新風格時修改既有的程式碼。家具供應商對於產品目錄的更新非常頻繁，你不會想再每次更新時都去修改核心程式碼的。

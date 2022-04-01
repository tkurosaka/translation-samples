---
content_description: 関連したオブジェクトの集団を、具象クラスを指定することなく生成することを可能とします。
---
# Abstract Factory

## 意図

**Abstract Factory** （抽象ファクトリー=工場) は、生成用デザインパターンのひとつで、関連したオブジェクトの集団を、具象クラスを指定することなく生成することを可能とします。

![Abstract Factory パターン](/images/patterns/content/abstract-factory/abstract-factory-en.png)


## 問題

家具工場シュミレーターを作ることを想像してみてください。次のようなモノを表現するクラスが必要となります:

1. 関連製品、例えば: `Chair` (椅子) + `Sofa` (ソファー) + `CoffeeTable` (コーヒーテーブル）.

2. この関連製品の様式。`Chair` + `Sofa` + `CoffeeTable` 製品は、例えば以下の様式のものがあります: `Modern` (現代風), `Victorian` (ビクトリア調), `ArtDeco` (アールデコ).

![製品群と様式](/images/patterns/diagrams/abstract-factory/problem-en.png)
> 製品群と様式
様式がマッチするために、同じ様式の家具オブジェクトを生成する何らかの方法が必要となります。様式の異なる家具が配達されたら、客は怒りますよね。

![](/images/patterns/content/abstract-factory/abstract-factory-comic-1-en.png)
> 現代風ソファーは、ビクトリア様式の椅子とは合いません！

プログラムに新しい製品や製品群を追加する時、コードの変更はしたくありません。家具メーカーはカタログを常に更新しますが、そのたびに中核のコードを変更するのは避けたいところです。

## Solution

The first thing the Abstract Factory pattern suggests is to explicitly declare interfaces for each distinct product of the product family (e.g., chair, sofa or coffee table). Then you can make all variants of products follow those interfaces. For example, all chair variants can implement the `Chair` interface; all coffee table variants can implement the `CoffeeTable` interface, and so on.

![The Chairs class hierarchy](/images/patterns/diagrams/abstract-factory/solution1.png)
> All variants of the same object must be moved to a single class hierarchy.

The next move is to declare the _Abstract Factory_—an interface with a list of creation methods for all products that are part of the product family (for example, `createChair`, `createSofa` and `createCoffeeTable`). These methods must return **abstract** product types represented by the interfaces we extracted previously: `Chair`, `Sofa`, `CoffeeTable` and so on.

![The _Factories_ class hierarchy](/images/patterns/diagrams/abstract-factory/solution2.png)
> Each concrete factory corresponds to a specific product variant.

Now, how about the product variants? For each variant of a product family, we create a separate factory class based on the `AbstractFactory` interface. A factory is a class that returns products of a particular kind. For example, the `ModernFurnitureFactory` can only create `ModernChair`, `ModernSofa` and `ModernCoffeeTable` objects.

The client code has to work with both factories and products via their respective abstract interfaces. This lets you change the type of a factory that you pass to the client code, as well as the product variant that the client code receives, without breaking the actual client code.

![](/images/patterns/content/abstract-factory/abstract-factory-comic-2-en.png)
> The client shouldn't care about the concrete class of the factory it works with.

Say the client wants a factory to produce a chair. The client doesn't have to be aware of the factory's class, nor does it matter what kind of chair it gets. Whether it's a Modern model or a Victorian-style chair, the client must treat all chairs in the same manner, using the abstract `Chair` interface. With this approach, the only thing that the client knows about the chair is that it implements the `sitOn` method in some way. Also, whichever variant of the chair is returned, it'll always match the type of sofa or coffee table produced by the same factory object.

There's one more thing left to clarify: if the client is only exposed to the abstract interfaces, what creates the actual factory objects? Usually, the application creates a concrete factory object at the initialization stage. Just before that, the app must select the factory type depending on the configuration or the environment settings.

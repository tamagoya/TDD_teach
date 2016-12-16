## PHPUnit入門

## ユニットテストとは

ユニットテスト == 単体テスト?

### [xUnit](https://ja.wikipedia.org/wiki/XUnit)

```
xUnitとは、コンピュータプログラムの単体テスト（ユニットテスト）を行うためのテスティングフレームワークの総称である。これらのフレームワークでは、関数やクラスなど、ソフトウェアの様々な要素（ユニット）をテストすることができる。xUnitフレームワークの主な利点は、テストを自動化できること、同じテストを何度も書かずに済むこと、個々のテストの結果がどうあるべきかを覚えておかなくても良いことである。
```

## PHPUnitとは

https://phpunit.de/manual/5.6/ja/index.html

## TDDハンズオン

### [TDD](https://ja.wikipedia.org/wiki/%E3%83%86%E3%82%B9%E3%83%88%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA)

```
テスト駆動開発 (てすとくどうかいはつ、test-driven development; TDD) とは、プログラム開発手法の一種で、プログラムに必要な各機能について、最初にテストを書き（これをテストファーストと言う）、そのテストが動作する必要最低限な実装をとりあえず行った後、コードを洗練させる、という短い工程を繰り返すスタイルである。
```

※ 本日の目的はTDDの習得ではありません。PHPUnitに慣れ親しんでもらうために、TDDハンズオンを行います。

###  <del>準備</del> (終わってるはず)

https://github.com/kiy0taka/ec-cube をfork
cloneしたらtdd-startブランチに移動

    $ git clone git@github.com:<自分のアカウント>/ec-cube.git
    $ cd ec-cube
    $ git co tdd-start

## お題「カートを実装する」
ショッピングカートを表すCartクラス (`src/Eccube/Entity/Cart.php`) の以下のメソッドを空実装にしているので、テストコードを書きながら実装していきます。

- カートに商品を追加する `addCartItem()`
- カートに入っている商品の個数を計算する `getTotalQuantity()`
- カートに入っている商品の合計金額を計算する `getTotalPrice()`

## 仕様説明

### \Eccube\Entity\Cart
- ショッピングカートを表すクラス
- カートに追加された商品(`CartItem`)を保持し、それらに対する操作を行う

#### フィールド

|名称|型|説明|
|---|---|---|
|CartItems|CartItem[]|カート商品の配列|

#### メソッド

|名称|戻り値の型|説明|
|---|---|---|
|addCartItem(CartItem)|void|カートに商品を追加する|
|getTotalQuantity()|int|カートに入っている商品の個数を計算する|
|getTotalPrice()|int|カートに入っている商品の合計金額を計算する|


### \Eccube\Entity\CartItem
- ショッピングカートに入れる商品を表す

#### フィールド
|名称|型|説明|
|---|---|---|
|class_name|string|商品名|
|class_id|string|商品ID|
|price|int|商品1つあたりの金額|
|quantity|int|数量|



#### テストクラスの作成
`\PHPUnit_Framework_TestCase`を継承したクラスを作る

- WebTestCase
- EccubeTestCase
- \PHPUnit_Extensions_Database_TestCase
- EccubeDatabaseTestCase

`tests/Eccube/Tests/Entity/CartTest.php`を以下のように作成。(IntelliJ/PhpStormの人はCartクラスで`Cmd`+`Shift`+`T`)

    <?php
    namespace Eccube\Tests\Entity;

    use Eccube\Entity\Cart;
    use Eccube\Entity\CartItem;

    class CartTest extends \PHPUnit_Framework_TestCase
    {

    }

※ テストクラスの名前空間

#### 最初のテストを書く
このテストで正しく検証できることを確認するためにとりあえず実行してテストが失敗することを確認

    public function testAddCartItem()
    {
		// Arrange
        $CartItem = new CartItem();
        $CartItem->setClassName("商品名");
        $CartItem->setClassId("100");
        $CartItem->setQuantity(1);
        $CartItem->setPrice(1000);

		// Act
        $Cart = new Cart();
        $Cart->addCartItem($CartItem);

		// Assert
        $actual = count($Cart->getCartItems());
        self::assertEquals(1, $actual);
    }

- 実行してみる


    $ vendor/bin/phpunit tests/Eccube/Tests/Entity/CartTest.php


#### テストを通すための最小限の実装
いきなりすべてを実装しない。小さなステップを踏んでいく。今回のように実装が見えていると面倒な作業だが、実際はまだどう実装していいかわからないものに対してテストを書いていく。ここでは時間をかけずにとりあえずの実装をしてテストが通ることを確認する。「Done is better than perfect.」

#### AAA

#### 新しいテストの追加
「Red -> Green -> Refactor」のサイクルで実装していく

 - 複数追加した時に期待通りの動きをするか？
 - nullのを渡したときは？
 - CartItem以外の型を渡したときは？
 - etc.

#### テストクラスのリファクタリング

    $CartItem = new CartItem();
    $CartItem->setClassName("商品名");
    $CartItem->setClassId("100");
    $CartItem->setQuantity(1);
    $CartItem->setPrice(1000);

のようなコードをテストメソッドごとに何回も書いているとテストコードの可読性が落ちる。テストコードもいい感じにリファクタリングしましょう。

    public function testAddCartItem()
    {
		// Arrange
        $Cart = new Cart();

		// Act
        $Cart->addCartItem(newCartItem("商品名", "100", 1, 1000));

		// Assert
        $actual = count($Cart->getCartItems());
        self::assertEquals(1, $actual);
    }

## CIとの連携
https://travis-ci.org/EC-CUBE/ec-cube
https://travis-ci.org/EC-CUBE/eccube-codeception


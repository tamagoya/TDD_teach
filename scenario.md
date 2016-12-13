## PHPUnit入門

## ユニットテストとは？

## PHPUnitとは

https://phpunit.de/manual/5.6/ja/index.html

## TDDハンズオン

TDDが絶対ではない。TDDハンズオンをすることで、PHPUnitに慣れ親しんでもらう。

### 準備
https://github.com/kiy0taka/ec-cube をfork
cloneしたらtdd-startブランチに移動

    $ git clone git@github.com:<自分のアカウント>/ec-cube.git
    $ cd ec-cube
    $ git co tdd-start

## お題「カートを実装する」
ショッピングカートを表すCartクラスの幾つかのメソッドをから実装にしているので、テストコードを書きながら実装していきます。

- カートに商品を追加する
- カートに入っている商品の個数を計算する
- カートに入っている商品の合計金額を計算する

## 仕様説明

### \Eccube\Entity\Cart
- ショッピングカートを表すクラス
- カートに追加された商品(カート商品)を保持し、それらに対する操作を行う

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
- ショッピングカートに入れる商品を表すPOPO

#### フィールド
|名称|型|説明|
|-|-|-|
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

`tests/Eccube/Tests/Entity/CartTest.php`を以下のように作成。(IntelliJ/PhpStormの人はCartクラスでCmd+Shift+T)

    <?php
    namespace Eccube\Tests\Entity;

    use Eccube\Entity\Cart;
    use Eccube\Entity\CartItem;

    class CartTest extends \PHPUnit_Framework_TestCase
    {

    }


#### 最初のテストを書く
このテストで正しく検証できることを確認するためにとりあえず実行してテストが失敗することを確認

    public function testAddCartItem()
    {
        $CartItem = new CartItem();
        $CartItem->setClassName("商品名");
        $CartItem->setClassId("100");
        $CartItem->setQuantity(1);
        $CartItem->setPrice(1000);

        $Cart = new Cart();
        $Cart->addCartItem($CartItem);

        $actual = count($Cart->getCartItems());
        self::assertEquals(1, $actual);
    }

- 実行してみる


    $ vendor/bin/phpunit tests/Eccube/Tests/Entity/CartTest.php


#### テストを通すための最小限の実装
いきなりすべてを実装しない。小さなステップを踏んでいく。今回のように実装が見えていると面倒な作業だが、実際はまだどう実装していいかわからないものに対してテストを書いていく。ここでは時間をかけずにとりあえずの実装をしてテストが通ることを確認する。「Done is better than perfect.」

#### 新しいテストの追加
「Red -> Green -> Refactor」のサイクルで実装していく

 - nullのを渡したときは
 - CartItem以外の型を渡したときは?

#### テストクラスのリファクタリング

    $CartItem = new CartItem();
    $CartItem->setClassName("商品名");
    $CartItem->setClassId("100");
    $CartItem->setQuantity(1);
    $CartItem->setPrice(1000);

のようなコードをテストメソッドごとに何回も書いているとテストコードの可読性が落ちる。テストコードもいい感じにリファクタリングしましょう。


## CIとの連携
https://travis-ci.org/EC-CUBE/ec-cube
https://travis-ci.org/EC-CUBE/eccube-codeception

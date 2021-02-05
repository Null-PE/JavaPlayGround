# 値オブジェクトからドメイン駆動設計に入門する

## 課題

1. UserAccountServiceTraditionalImplクラスを修正し、UserAccountServiceTraditionalImplTestクラスのテストを合格させましょう
1. UserNameクラスを修正し、UserAccountServiceDddImplTestクラスのテストを合格させましょう
1. 上記の2つのJUnit Testについて、updateUserメソッドに対しても同じようなルールのテストケースを追加しましょう
1. 追加したテストケースを合格させましょう
1. 最後に、TraditionalとDddのどちらのコードのほうが好き/嫌いだと思ったか、またそう思った理由を書きましょう
   - 「好き嫌いとその理由」を自分の言葉にすることで、記憶が精緻化され、より忘れにくくなります

## ドメイン駆動設計における値オブジェクトとは

- システム固有の値を表現するもの
- **「オブジェクト自身が知っていること」** を値オブジェクトの実装とすることで、下記のようなメリットがある
    - (メソッドの引数や返り値などでの)表現力が増す
    - プログラム中に不正な値を存在させない
    - 誤った代入を防ぐ
    - ロジックの散在を防ぐ

## 参考書籍

[ドメイン駆動設計入門 ボトムアップでわかる！ドメイン駆動設計の基本](https://www.shoeisha.co.jp/book/detail/9784798150727)
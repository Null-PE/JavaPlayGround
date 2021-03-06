# パラメータ化テスト(parameterized Test)
`junit.parameterized` パッケージ配下にあるコードを使います。  
[実装](https://github.com/Null-PE/JavaPlayground/tree/master/src/main/java/junit/parameterized)  
[テスト](https://github.com/Null-PE/JavaPlayground/tree/master/src/test/java/junit/parameterized)

## タイムスケジュール
10:31 ~ 10:35 解説  
10:35 ~ 10:45 ライブコーディング(みんなで一緒に)  
10:45 ~ 10:55 課題  
10:55 ~ 11:00 質疑応答  

## 解説
パラメータ化テストとは、一つのテストケースメソッドに対して様々に条件を変えて実行することのテストです。  
このテストはテストコードとデータを分割して記載できるため、以下の様なメリットがあります。
- 引数と期待値が異なる同じようなテストメソッドを書かなくても良い
- テストケースを追加するときに、パラメータを追加するだけで良い
- テストコードとデータが分割されるので可読性が高い

「このメソッドのテスト、データの生成手順も同じだし、アサーションも同じなのになんでこんなに大量のコード書かなきゃいけないんだろう。」  
「テストの構造化はしているけど、ケースの差分は少しだし、なんか冗長だな」  
というときに使えます。

## 練習
師範と一緒に
`MainDayOfMonthTest` に`junit.parameterized.MainDayOfMonth.getDayOfMonth` メソッドのテストを書きましょう。

一人でやる場合は、`SampleEndOfMonthTest` クラスの `DayOfMonthTest` を参考に、うるう年を考慮せずにテストを書いてみてください。

### [参考]登場人物
`@RunWith` : どのようにテストを実行するかを指定するアノテーション  
`Parameterized` : パラメータ化テストを実行するためのテストランナー  
`@Parameters` : テストランナーに `Parameterized` が指定されたときに、テストケースとなるパラメータを格納する変数に指定するアノテーション  
`Theories` : パラメータ化テストを実行するためのテストランナー  
`@DataPoints` : テストランナーに `Theories` が指定されたときにテストケースとなるパラメタを格納する変数に指定するアノテーション

## 課題
### 課題1
テストケースにうるう年のケースを足してみましょう。  
テストが失敗するようになるので、テスト対象メソッドがうるう年をサポートするようにしてください。

### 課題2
パラメータ化テストには `Parameterized` の他に `Theories` を使った実装方法があります。  
 `SampleEndOfMonthTest.DayOfMonthTheoriesTest` を参考に、`MainDayOfMonthTest.DayOfMonthTheoriesTest` を実装してください。

### 課題3
以下の問いについて自分なりに回答してみてください。  
- `Parameterized` テストランナーと `Theories` テストランナーはどのような点が異なるのでしょうか？  
- 自分はどちらが好みでしょうか？その理由はなんですか？  

## 解答例
課題1,2は `SampleEndOfMonthTest` に実装してあります。  
課題3は自分の周りの人と議論してみてください。

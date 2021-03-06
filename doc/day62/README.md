# F12や別のjsでDubble Gameを破壊しよう。そして即時関数でそれを阻止しよう

今日破壊するコードはこちら
https://github.com/Null-PE/JavaPlayground/tree/master/doc/DobbleGame

# getImageResourcesをF12で破壊せよ

app.jsの今のコード
```
function getImageResources() {
  return [
    { src: "img/black-circle.png" },
    { src: "img/black-square.png" },
    { src: "img/black-triangle.png" },
    { src: "img/red-circle.png" },
    { src: "img/red-square.png" },
    { src: "img/red-triangle.png" },
    { src: "img/yellow-circle.png" },
    { src: "img/yellow-square.png" },
    { src: "img/yellow-triangle.png" },
    { src: "img/green-circle.png" },
    { src: "img/green-square.png" },
    { src: "img/green-triangle.png" },
    { src: "img/blue-circle.png" },
    { src: "img/blue-square.png" },
    { src: "img/blue-triangle.png" },
  ];
}
```

F12で以下を実行

```
function getImageResources() {

    alert("Hacked!!");

}

```

答えを選ぼう、2問目へすすめないぞ

リロードして以下を実施

F12
```
function alert(){
   console.log('alertは動きませんぞ'); 
}

alert('hello');
```
ダイアログがでないぞ

なぜそうなるかというと、上記の書き方はMapのような構造体であるjsonオブジェクト{}にfunctionが追加、上書きされていく仕組みだから
https://developer.mozilla.org/ja/docs/Learn/JavaScript/Objects/JSON

正確ではないが、ゲームの画面を読み込んだときの雑なイメージ
```
var window ＝ {};

ブラウザに実装されている関数をロード（こうなっているかは不明だが、わかりやすさのために、、）
window.alert = function(val){xxx}); 
window.console = function(val){xxx});

or 

window = { 
  alert: function(){
    XXXXX
  },
  console: function(){
    XXX;
  }
}

https://developer.mozilla.org/ja/docs/Web/API/Window

app.jsを読み込んだとき
window.apendImage =function(){略};
window.pickUpAndRemoveRandomImages = function(){略};
window.getImageResources=function(){略A};

or 

window = { 
  alert: function(){
    XXXXX
  },
  console: function(){
    XXX;
  },
  apendImage: function(){
    XXX;
  },
  pickUpAndRemoveRandomImages: function(){
    XXX;
  },
  getImageResources: function(){
    XXX;
  }
}


F12でやったこと
window.getImageResources=function(){CCC};
```

なにがまずい？

```
    <script src="lib/jquery-3.5.1.js"></script>
    <script src="app.js"></script>
```

もし同じメソッド名やプロパティがあった場合に後勝ちで上書きされてしまう。
実際に上書きしてみよう

hack.jsを作ろう
```
function alert(){
    console.log('残念、alertはもう動きませんよ');
}

function getImageResources(){
    console.log('もう画像ないから、2問目はいけませんよ');    
}
```

index.html

```
    <script src="lib/jquery-3.5.1.js"></script>
    <script src="app.js"></script>
    <script src="hack.js"></script>
```

あなたのプロダクトは1画面にどれだけのjsをいれているだろうか？

constやletはどうなる？

再宣言なのでエラーになる。

varは？

エラーがでずに値が上書きされる。

hack.jsに追加してみよう

```
    let $card1 = 1
    let $card2 = 2
    const numberOfImagesInCard = 10;

```


# じゃあどうすればいいの？

ちょっと関数内の関数をためしてみよう

F12
```
function a(){

    function b_(){
        console.log('abc');
    }

    B();
}
```
https://w.atwiki.jp/aias-jsstyleguide2/pages/15.html#naming

```
b();
window.b();

a();
```

関数内の関数は定義した関数内以外からは呼べない仕様です。

この仕組みを利用しましょう

app.js
```
function Game() {

    ここにソースコードを入れる
}

Game();
```

実行しよう

うごいた！！！

F12で破壊しよう
```
function getImageResources(){
    console.log('もう画像ないよ');    
}

```

破壊できない。。。　つまり　他のフレームワークと競合しなくなった


### 即時関数にしよう

でもGame.getImageResourcesで上書きできるじゃん。。。
サイドバーとかでGameを切り替えられる様なUIだったらそもそもGame自体が被りそうじゃん

その通りです。Gameという新しい関数名をつかわずに、

やりたいことは、functionでfunction郡を囲んですぐ実行したいだけ、
それが即時関数でできます。F12でやってみよう

引数なしパターン
```
(function(){
    function a_(){
       alert('log');
    }
    
    a_();
    
})();

```

引数ありパターン
```
(function(val){

    function a_(c){
       alert(c);
    }
    
    a_(val);
})('kobayashi');

```

じゃあゲームに適用しよう

app.js
```
(function(){
   
    ここにいれる

})();

```

これでめでたく、DubbleGameは誰にも影響をあたえないし、
誰の影響もうけない（正確にはつかっているjqueryのようなグローバル変数の変更の影響はうける）良い子になりました。

# 昔のjQueryの実装をみてみよう

ちなみに、昔の1.X時代のjQueryの実装をみてみよう
https://github.com/jquery/jquery/blob/1.6.4/src/core.js

```
var jQuery = (function(){
    var jQuery = function(){}
    jQuery.prototype = {}
    return jQuery;
})();
```

https://github.com/jquery/jquery/blob/1.6.4/src/deferred.js

```
(function(jQuery){
  
})(jQuery);
```


今日のDubbleの最終形態

```
(function ($) {
  function apendImage_($targetCard, imageResourceArray) {
    imageResourceArray.forEach((element) => {
      $appendImage = $("<img>", element);
      $appendImage.addClass("character-image");
      $targetCard.append($appendImage);
    });
  }

  const numberOfImagesInCard_ = 8;

  function pickUpAndRemoveRandomImages_(targetImageRersourceArray) {
    let returnArray = [];
    for (let i = 0; i < numberOfImagesInCard - 1; i++) {
      let randomIndex = Math.floor(
        Math.random() * targetImageRersourceArray.length
      );
      returnArray.push(targetImageRersourceArray[randomIndex]);
      targetImageRersourceArray.splice(randomIndex, 1);
    }
    return returnArray;
  }

  function getImageResources_() {
    return [
      { src: "img/black-circle.png" },
      { src: "img/black-square.png" },
      { src: "img/black-triangle.png" },
      { src: "img/red-circle.png" },
      { src: "img/red-square.png" },
      { src: "img/red-triangle.png" },
      { src: "img/yellow-circle.png" },
      { src: "img/yellow-square.png" },
      { src: "img/yellow-triangle.png" },
      { src: "img/green-circle.png" },
      { src: "img/green-square.png" },
      { src: "img/green-triangle.png" },
      { src: "img/blue-circle.png" },
      { src: "img/blue-square.png" },
      { src: "img/blue-triangle.png" },
    ];
  }

  let $card1 = $("#card1");
  let $card2 = $("#card2");

  function startGame_() {
    let imageResources = getImageResources();

    let card1Images = pickUpAndRemoveRandomImages(imageResources);
    let card2Images = pickUpAndRemoveRandomImages(imageResources);

    let answerIndex = Math.floor(Math.random() * imageResources.length);
    let answerImage = imageResources[answerIndex];
    answerImage["class"] = "answer";

    let card1AnswerIndex = Math.floor(Math.random() * card1Images.length + 1);
    card1Images.splice(card1AnswerIndex, 0, answerImage);

    let card2AnswerIndex = Math.floor(Math.random() * card2Images.length + 1);
    card2Images.splice(card2AnswerIndex, 0, answerImage);

    apendImage($card1, card1Images);
    apendImage($card2, card2Images);
  }

  $("#game-field").on("click", ".answer", function () {
    alert("正解！");
    $card1.empty();
    $card2.empty();
    startGame();
  });

  startGame_();
})(jQuery);

```























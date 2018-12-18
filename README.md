# Minesweeper VueJS

今週はマインスイーパーを実装してみます！

チェックポイントは５つあります。６日あるのでレビューの時間も含めて、毎日１つのチェックポイント進めるのが良いペースとなります。フィーチャーが未完成でも毎日プルリクエストを投稿してメンターと確認しながら進めてください。

こちらが[マインスイーパ](https://ja.wikipedia.org/wiki/%E3%83%9E%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%A4%E3%83%BC%E3%83%91)の[参考動画](https://www.youtube.com/watch?v=_aGPGQ0DDZk)です。

## コンテンツ：

* [Month 2 Git Workflow](https://github.com/bootcamp-tpa/tpa-resources/wiki/%5B%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC%5D-2%E3%83%B6%E6%9C%88%E7%9B%AE%E3%81%AEGit-Workflow)
* Checkpoints
  * [Checkpoint 1 README](#checkpoint-1)
  * [Checkpoint 2 README](#checkpoint-2)
  * [Checkpoint 3 README](#checkpoint-3)
  * [Checkpoint 4 README](#checkpoint-4)
  * [Checkpoint 5 README](#checkpoint-5)
* [Starting the App](#starting-the-app)

***

# Checkpoint-1

モグラ叩きではクラスネームを変更するとCSSが変わって、モグラが出て来てくれましたよね。マインスイーパーのタイルの数字、爆弾、周りのタイルに爆弾がない空のタイルも全てクラスネームによって表示されます。ご覧ください：

[クラスネームを変えると再描画](https://youtu.be/PoKH6smG1GY)

**タスク：**

* データ型を設計して、コンポーネントのdata関数からデータを戻そう。
  * 横19タイル、縦10タイルのボードサイズにしましょう。
* startGameメソッドを追加して、start ボタンのclickイベントに紐付けよう。
* startGameの実装：データ型をリセットしましょう。
* `<tr><td></td></tr>`タグを使用して19x10のボードを表示しよう。
  * _ヒント_ 描画のコードはデータ設計にもよりますが、タイルが二次元配列の場合、double v-for文で`<Tile>`を回す感じになります。
  * _ヒント:_ v-for文で、要素だけではなく、indexも取得したかったらこういうシンタックスです `v-for="(row, rowIndex) in tiles"`
* 一つの`<td>`をTileとしてコンポーネント化しよう。

[Checkpoint1完成画面（スタートボタンを押した後）](https://i.imgur.com/dZhUUoV.png)

# Checkpoint-2

チェックポイント２を挑戦する前に、一つのタイルのデータ型を確認してください：

```javascript
{
  row: rowIndex,
  column: columnIndex,
  mined: Math.random() * 6 > 5,
  class: 'unopened',
}
```

上記みたいな形になりましたか？

row と column を取得しやすいように格納しておく
minedはタイルが爆弾か爆弾ではないかを表す（`* 6 > 5` の部分で爆弾の確率を調整する）
classは表示されているSVGを変えるため

次はゲームロジックを紹介します。基本ユーザーは左クリックか右クリックするだけのゲームなので、二つのイベントに対する処理が全てとなります。

```diff
// ...

methods: {
  openTile: function() {
+   // if the tile is mined
+     // show a mine
+     // reveal all other tiles
+   // if its not mined
+     // collect information on its neighbors
+     // count how many mines surround the tile
+     // if there are mines
+       // reveal the number of mines that surround the tile
+     // if there are no mines
+       // show the tile as 'opened'
+       // (for each neighbor)
+         // (if the neighbor has not been opened yet)
+           // (open the neighbor)
  },
  setFlag: function(tile) {
+   // change tile to 'flagged'
  },
},

// ...
```

カッコに囲まれている処理は再帰処理です。とりあえず、再帰処理を置いといて一つのタイルをopenします。再帰処理はDay5に挑戦しますので！

今日の課題では**スタブ**を利用します。

スタブは関数名を定義して、入力・出力は考えておくけど、関数の中身の実装は後回しにするテクニックです。

[スタブ参考](https://github.com/bootcamp-tpa/tpa-resources/wiki/%E3%82%B9%E3%82%BF%E3%83%96%E5%8F%82%E8%80%83)

今日の課題のポイント：ヘルパー関数のスタブを作ることによってあまり実装の細かい所に引っかからなくてロジックを一通り書ける。シンタックスなどを気にしながらロジックを上手く想像するのは難しい。様々の関数の中身を気にしながらコードを書いていくと、どうしても処理に対して「何をしてたんだっけ？」って考えがまとめにくくなってしまう。スタブを上手に使いながら実装して行きましょう！

**タスク：**

* タイルの左クリックイベントと右クリックイベントにそれぞれのハンドラーを紐付けよう。
* 右クリックの場合、タイルにフラッグを表示しよう。
* 左クリックハンドラーはまだ実装しなくてOK。上記のロジックに従うためにヘルパーメソッドをスタブしとこう（メソッドを定義するけど中身は実装しなくてOK。）
* ヘルパーメソッドをJSDOCスタイルで上に関数説明・入力・出力コメントを残しておこう。

JSDocの参考：

```javascript
// App.vue
// ...

methods: {
  /**
   * flags a tile
   * @function
   * @param {Object} tile
   * @return {undefined}
   */
  flagTile: function(tile) {

  },
},

// ...
```


**リンク：**

* [$emit](https://jp.vuejs.org/v2/guide/components-custom-events.html)
* [`v-on`](https://jp.vuejs.org/v2/api/#v-on)
* [click イベント](https://developer.mozilla.org/en-US/docs/Web/Events/click)
* [contextMenu イベント](https://developer.mozilla.org/en-US/docs/Web/Events/contextmenu)
* [JSDOC 関数](http://usejsdoc.org/tags-function.html)

# Checkpoint-3

**タスク：**

* openTile と showAll 以外、Checkpoint 2 からのヘルパーメソッドを全て実装しよう。

まだopenTileと紐づけていないので、コードがどういう風に動くかが確認しにくいかもしれません。openTile でメソッドを実行してみたり、コンソールログを利用して確認しながら進めましょう。

# Checkpoint-4

**タスク：**

* openTileメソッドも実装しよう。（まだ再帰処理は置いといてOK）
* 一つのタイルをopenしたら[こういう風](https://youtu.be/sPY8w5zIyQE)に動くはずです。確認してください。

# Checkpoint-5

再帰呼出の処理に挑戦してみます。

再帰呼出とは：

（単純な例）

```
function foo () {
  foo();
}

foo(); // ← MAXIMUM CALL STACK EXCEEDED というエラーが出ます。
```

foo 関数は自分(foo)を呼び出すことによって「関数そんなに読んだらコールスタック用のメモリーが足りない。絶対コードおかしいよ」と伝えてくれるエラーが出て来ます。

再帰処理を行うときは必ず再帰呼び出しがいずれ止まる仕組みを含めて書かないといけません。例えば：

```
function foo(arr) {
  arr.forEach(() => {
    foo(arr.slice(1));
  });
}

foo([1,2,3,4,5]);
```

上記の場合、最終的には arr が空の配列となって forEach が回されない→再帰呼出が止まるというパターンです。

マインスイーパーの場合：

- まずはタイルの周りに爆弾があるかを判断する。一つでもあったら再帰呼出はしないで、見つかった爆弾数を表示する。
- もし周りに爆弾が一つもなかったら、for each neighbor を回す時に、neighborがすでにopenの状態の場合は再帰呼出をしない。（もしまだopenされていなかったら再帰的に呼び出す）

ということによって周りのタイルが全て開いているか、爆弾があったかという状況で再帰処理はいずれ止まります。

**タスク：**

* openTileメソッドの再帰処理を実装してみましょう。[参考動画](https://youtu.be/AccQ5m8lQfA)はず。
* もし爆弾タイルをクリックしてしまった場合、全てのタイルを表示するメソッド (showAll) も実装しよう。[参考動画](https://youtu.be/RUim6xxH8FI)

### **以上です！マインスイーパーもお疲れ様でした！**

***

# Starting the App

```bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm start
```

Building the app for production:

```bash
# build for production with minification
npm run build
```

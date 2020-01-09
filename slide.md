### Omotesando.rb #54
- - -

### ActiveRecord のコードを読んでみる

---

#### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
  * [ピュア Ruby で Ruby 2.7 の Numbered parameter を実装してみよう！ - Secret Garden(Instrumental)](http://secret-garden.hatenablog.com/entry/2019/12/01/154607)   <!-- .element: class="fragment" -->
* Rails 歴 2年弱                                       <!-- .element: class="fragment" -->
* 趣味で Ruby にパッチを投げたりしてます                              <!-- .element: class="fragment" -->
  * Ruby 2.7 に [Time#floor](https://bugs.ruby-lang.org/issues/15653) / [Time#ceil](https://bugs.ruby-lang.org/issues/15772) を追加したり
* エディタは Vim                             <!-- .element: class="fragment" -->
  * <del>わしの vimrc は5000行あるぞ</del>


---

### 今日話すこと
- - -

* 普段やっているように ActiveRecord のコードを読みてみます                         <!-- .element: class="fragment" -->
  * 読むコードが ActiveRecord なだけで ActiveRecord に特化しているわけではない                        <!-- .element: class="fragment" -->
  * ActiveRecord のコード以外でも同じような感じで読んでいる        <!-- .element: class="fragment" -->
* コードを読みつつ使っている道具の説明                         <!-- .element: class="fragment" -->

---

#### 使用する道具
- - -

* Method#source_location                 <!-- .element: class="fragment" -->
  * 任意のメソッド定義位置を調べる
* grep コマンド + α                 <!-- .element: class="fragment" -->
  * どこでメソッドが利用されているのかとか検索
* p でデバッグ出力                 <!-- .element: class="fragment" -->
  * メソッド名や引数の値を出力
* binding.irb                        <!-- .element: class="fragment" -->
  * 実行中のコードを操作
* エディタの機能を使う                 <!-- .element: class="fragment" -->
  * ハイライトしたり検索したり

---

## 実際にやってみる！

---

#### Method#source_location
- - -

```ruby
class X
  def hoge; end
end
x = X.new

# メソッドの定義位置を返す
pp x.method(:hoge).source_location
# => ["/home/hoge/test.rb",

# Ruby 2.7 だと #inspect で出力される
pp x.method(:hoge)
# => #<Method: X#hoge() /home/hoge/test.rb:2>
```

---

#### grep コマンド + α
- - -

* 任意の文字が書かれている場所を検索
* Vim 上で検索するようにしている
  * [unite.vim](https://github.com/Shougo/unite.vim) + [vimfiler.vim](https://github.com/Shougo/vimfiler.vim) を利用している
  * 今ならもっとモダンなプラグインがあるかも

---

#### pp でデバッグ出力
- - -

```ruby
def plus(a, b)
  # メソッド名や引数を出力
  pp __method__    # => :plus
  pp [a, b]        # => [1, 2]
  pp a.class.name  # => "Integer"
  pp caller        # コールスタックを出力

  a + b
end

plus 1, 2
```

↓ ↓ ↓ ↓

>>>

### binding-debug を使う
- - -

* $ gem install binding-debug

```ruby
require "binding/debug"
using BindingDebug

def plus(a, b)
  # {} でメソッドや変数を書くと名前付きで出力する
  pp { __method__ }    # => __method__ : :plus
  pp { [a, b] }        # => [a, b] : [1, 2]
  pp { a.class.name }  # => a.class.name : "Integer"
  a + b
end
plus 1, 2
```


---

#### エディタの機能を使う
- - -

* 便利プラグインを使う
* [vim-quickrun](https://github.com/thinca/vim-quickrun)
  * Vim 上でコードを実行する
* [vim-brightest](https://github.com/osyo-manga/vim-brightest)
* [anzu.vim](https://github.com/osyo-manga/vim-anzu)
  * 検索数を表示する
* [vim-quickhl](https://github.com/t9md/vim-quickhl)
  * 任意の単語をハイライトする


---

#### まとめ
- - -

* Ruby にはコードを読むための道具がいろいろとある               <!-- .element: class="fragment" -->
* Ruby だけではなくてエディタにもこだわるとより捗る               <!-- .element: class="fragment" -->
* コードの読み方は人それぞれだと思うので自分にあったものをいろいろと試してみよう               <!-- .element: class="fragment" -->
* 追記したデバッグコードはちゃんと消そう！         <!-- .element: class="fragment" -->


---

## ご清聴
## ありがとうございました

# rails Tutorial

[第4章 Rails風味のRuby](https://railstutorial.jp/chapters/rails_flavored_ruby?version=5.1#cha-rails_flavored_ruby)

## カスタムヘルパー

ページタイトルを生成するメソッドを作成

app/helpers/application_helper.rb

```ruby
module ApplicationHelper

  # ページごとの完全なタイトルを返します。
  def full_title(page_title = '')
    base_title = "Ruby on Rails Tutorial Sample App"
    if page_title.empty?
      base_title
    else
      page_title + " | " + base_title
    end
  end
end
```

app/views/layouts/application.html.erb

```ruby
<title><%= full_title(yield(:title)) %></title>
```

## Railsコンソールで実行結果を確認

```terminal
$ rails console
```

### 数字と文字列

```terminal
>> 17 + 42
=> 59

>> "foo" + "bar"
=> "foobar"

>> first_name = "Michael"
=> "Michael"
>> "#{first_name} Hartl"
=> "Michael Hartl"
```

### putsとprint

```terminal
>> puts "foo"
foo # 改行される
=> nil

>> print "foo"
foo=> nil　# 改行されない

>> print "foo\n" # 改行文字で改行
foo
=> nil
```

### シングルクォート

```terminal
>> 'foo'
=> "foo"
>> 'foo' + 'bar'
=> "foobar"

# シングルクォート内の文字列では式展開ができない
>> '#{foo} bar'
=> "\#{foo} bar"

# 'バックスラッシュ n' をそのまま扱う
>> '\n'
=> "\\n"
```

### 論理値を返す/条件文/オブジェクト

Rubyではあらゆるものがオブジェクトである。文字列やnilもオブジェクトである

```terminal
>> "foobar".length
=> 6
```

論理値(boolean)を返すときに末尾に疑問符を示す

```terminal
>> s = "foobar"
>> if s.empty?
>>   "The string is empty"
>> else
>>   "The string is nonempty"
>> end
=> "The string is nonempty"
```

条件文を2つ以上含めたいとき

```terminal
>> if s.nil?
>>   "The variable is nil"
>> elsif s.empty?
>>   "The string is empty"
>> elsif s.include?("foo")
>>   "The string includes 'foo'"
>> end
=> "The string includes 'foo'"
```

論理値and/or/not

```terminal
>> x = "foo"
=> "foo"
>> y = ""
=> ""
>> puts "Both strings are empty" if x.empty? && y.empty?
=> nil
>> puts "One of the strings is empty" if x.empty? || y.empty?
"One of the strings is empty"
=> nil
>> puts "x is not empty" if !x.empty?
"x is not empty"
=> nil
```

nilもオブジェクト

```terminal
>> nil.to_s
=> ""

>> nil.empty?
NoMethodError: undefined method `empty?' for nil:NilClass
# メソッドチェーン
>> nil.to_s.empty?
=> true

>> "foo".nil?
=> false
>> "".nil?
=> false
>> nil.nil?
=> true
```

ifを後に書くこともできる

```terminal
x = "foo"
puts "x is not empty" if !x.empty?
=> x is not empty
```

!!(バンバン)で強制的に論理値にする

```terminal
>> !!nil
=> false

>> !!0
=> true
```

### railsコンソールでメソッドを定義することもできる

```terminal
>> def string_message(str = '')
>>   if str.empty?
>>     "It's an empty string!"
>>   else
>>     "The string is nonempty."
>>   end
>> end
=> :string_message
>> puts string_message("foobar")
The string is nonempty.
>> puts string_message("")
It's an empty string!
>> puts string_message
It's an empty string!
```

### 文字列を配列に変換

```terminal
>> "foo bar baz".split
=> ["foo", "bar", "baz"]

>> "fooxbarxbaz".split('x')
=> ["foo", "bar", "baz"]
```

### 配列の要素を取得

```terminal
>> a = [42, 8, 17]
=> [42, 8, 17]
>> a[0]
=> 42
>> a[1]
=> 8
>> a[2]
=> 17
>> a[-1]
=> 17

>> a.first
=> 42
>> a.second
=> 8
>> a.last
=> 17
>> a.last == a[-1]
=> true
```

### 配列にメソッド

```terminal
>> a = [42, 8, 17]
=> [42, 8, 17]
>> a.empty?
=> false
>> a.include?(42)
=> true
>> a.sort
=> [8, 17, 42]
>> a.reverse
=> [17, 8, 42]
>> a.shuffle
=> [17, 42, 8]
>> a
=> [42, 8, 17]
```

### 破壊的メソッド

メソッドの末尾に「!」を追加

```terminal
>> a = [42, 8, 17]
=> [42, 8, 17]
>> a.sort!
=> [8, 17, 42]
>> a
=> [8, 17, 42]
```

### push(<<)メソッドで要素追加

```terminal
>> a = [42, 8, 17]
=> [42, 8, 17]
>> a.push(6)
=> [42, 8, 17, 6]
>> a << 7
=> [42, 8, 17, 6, 7]
>> a << "foo" << "bar"
=> [42, 8, 17, 6, 7, "foo", "bar"]
```

### 配列から文字列に変換

```terminal
>> a = [42, 8, 17, 6, 7, "foo", "bar"]
=> [42, 8, 17, 6, 7, "foo", "bar"]
>> a.join
=> "4281767foobar"
>> a.join(', ')
=> "42, 8, 17, 6, 7, foo, bar"
```

### rangeとto_a

to_aは配列に変換するメソッド

```terminal
>> (0..9).to_a
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>> a = %w[foo bar baz quux]
=> ["foo", "bar", "baz", "quux"]
>> a[0..2]
=> ["foo", "bar", "baz"]

>> a = (0..9).to_a
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>> a[2..(a.length-1)]
=> [2, 3, 4, 5, 6, 7, 8, 9]
>> a[2..-1]
=> [2, 3, 4, 5, 6, 7, 8, 9]

>> ('a'..'e').to_a
=> ["a", "b", "c", "d", "e"]
```

### each

```terminal
>> (1..5).each { |i| puts 2 * i } # 1行のとき {}
2
4
6
8
10
=> 1..5

>> (1..5).each do |i| # 複数行のとき do end
?>   puts 2 * i
>> end
2
4
6
8
10
=> 1..5
```

### times

```terminal
>> 3.times { puts "Betelgeuse!" }   # 3.timesではブロックに変数を使っていない
"Betelgeuse!"
"Betelgeuse!"
"Betelgeuse!"
=> 3
```

### map

```terminal
>> (1..5).map { |i| i**2 }          # 「**」記法は冪乗 (べき乗)
=> [1, 4, 9, 16, 25]

>> %w[a b c]                        # %w で文字列の配列を作成
=> ["a", "b", "c"]

>> %w[a b c].map { |char| char.upcase }
=> ["A", "B", "C"]

>> %w[A B C].map { |char| char.downcase }
=> ["a", "b", "c"]

>> %w[A B C].map(&:downcase) # 省略記法
=> ["a", "b", "c"]
```

### ハッシュ(連想配列)

ハッシュの波カッコは、ブロックの波カッコとはまったく別物である

```terminal
>> user = {}                          # {}は空のハッシュ
=> {}
>> user["first_name"] = "Michael"     # キーが "first_name" で値が "Michael"
=> "Michael"
>> user["last_name"] = "Hartl"        # キーが "last_name" で値が "Hartl"
=> "Hartl"
>> user["first_name"]                 # 要素へのアクセスは配列の場合と似ている
=> "Michael"
>> user                               # ハッシュのリテラル表記
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}

>> user = { "first_name" => "Michael", "last_name" => "Hartl" }
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}
```

### シンボルを使ったハッシュ

```terminal
>> user = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> user[:name]              # :name に対応する値にアクセスする
=> "Michael Hartl"
>> user[:password]          # 未定義のキーに対応する値にアクセスする
=> nil
```

コロンとハッシュロケット

```terminal
>> h1 = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> h2 = { name: "Michael Hartl", email: "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> h1 == h2
=> true
```

### ハッシュとeach

```terminal
>> flash = { success: "It worked!", danger: "It failed." }
=> {:success=>"It worked!", :danger=>"It failed."}
>> flash.each do |key, value|
?>   puts "Key #{key.inspect} has value #{value.inspect}"
>> end

Key :success has value "It worked!"
Key :danger has value "It failed."
```

### inspect

オブジェクトを表現する文字列を返す

```terminal
>> puts (1..5).to_a            # 配列を文字列として出力
1
2
3
4
5

>> puts (1..5).to_a.inspect    # 配列のリテラルを出力
[1, 2, 3, 4, 5]

>> puts :name, :name.inspect
name
:name

>> puts "It worked!", "It worked!".inspect
It worked!
"It worked!"
```

inspectのショートカットpメソッド

```terminal
>> p :name             # 'puts :name.inspect' と同じ
:name
```

### blankとempty

Railsはblank?メソッドをRubyに追加

```terminal
>> "".blank?
=> true

>> "      ".empty?
=> false

>> "      ".blank?
=> true

>> nil.blank?
=> true
```

## CSSファイル読み込みについて

```ruby
stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload'
```

- メソッド呼び出しで丸カッコは不要(stylesheet_link_tag())
- メッシュ呼び出しの最後の引数はハッシュ不要({media: 'all', 'data-turbolinks-track': 'reload'})
- 途中で改行されても空白行とはみなさない
- 第一引数の文字列はスタイルシートへのパス

実際のHTMLソース

```ruby
<link data-turbolinks-track="true" href="/assets/application.css"
media="all" rel="stylesheet" />
```

## クラス

Rubyは多くのオブジェクト指向言語と同様、メソッドをまとめるのにクラスを使っています。
これらのクラスからインスタンスが生成されることでオブジェクトが作成されます。

### コンストラクタ

ダブルクォートは文字列のコンストラクタ

```terminal
>> s = "foobar"
=> "foobar"
>> s.class
=> String
```

名前付きコンストラクタはnewメソッド呼び出す

文字列

```terminal
>> s = String.new("foobar")
=> "foobar"
>> s.class
=> String
>> s == "foobar"
=> true
```

配列

``` terminal
>> a = Array.new([1, 3, 2])
=> [1, 3, 2]
```

ハッシュ

```terminal
>> h = Hash.new
=> {}
>> h[:foo]            # 存在しないキー (:foo) の値にアクセスしてみる
=> nil
>> h = Hash.new(0)    # 存在しないキーのデフォルト値をnilから0にする
=> {}
>> h[:foo]
=> 0
```

### クラス継承

Rubyにおけるすべてのクラスは最終的にスーパークラスを持たないBasicObjectクラスを継承

``` terminal
>> s = String.new("foobar")
=> "foobar"

>> s.class                        # 変数sのクラスを調べる
=> String

>> s.class.superclass             # Stringクラスの親クラスを調べる
=> Object

>> s.class.superclass.superclass  # Ruby 1.9からBasicObjectが導入
=> BasicObject

>> s.class.superclass.superclass.superclass
=> nil
```

Wordクラスを定義

```terminal
>> class Word < String
>>   def palindrome?
>>     self == self.reverse # selfは文字列自身を表します
>>   end
>> end
=> :palindrome?

>> s = Word.new("level")
=> "level"
>> s.palindrome?
=> true
>> s.length
=> 5

>> s.class
=> Word
>> s.class.superclass
=> String
>> s.class.superclass.superclass
=> Object
```

### controllerの継承関係

``` terminal
>> controller = StaticPagesController.new
=> #<StaticPagesController:0x22855d0>
>> controller.class
=> StaticPagesController
>> controller.class.superclass
=> ApplicationController
>> controller.class.superclass.superclass
=> ActionController::Base
>> controller.class.superclass.superclass.superclass
=> ActionController::Metal
>> controller.class.superclass.superclass.superclass.superclass
=> AbstractController::Base
>> controller.class.superclass.superclass.superclass.superclass.superclass
=> Object
```

### Userクラスを作ってみる

```ruby
class User
  attr_accessor :name, :email

  def initialize(attributes = {})
    @name  = attributes[:name]
    @email = attributes[:email]
  end

  def formatted_email
    "#{@name} <#{@email}>"
  end
end
```

``` terminal
>> require './example_user'
=> true

>> example = User.new
=> #<User:0x224ceec @email=nil, @name=nil>

>> example.name = "Example User"
=> "Example User"

>> example.email = "user@example.com"
=> "user@example.com"

>> example.formatted_email
=> "Example User <user@example.com>"
```

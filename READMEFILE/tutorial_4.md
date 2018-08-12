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

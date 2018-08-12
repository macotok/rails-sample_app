# README一覧

- [第3章 ほぼ静的なページの作成](/READMEFILE/tutorial_3.md)

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

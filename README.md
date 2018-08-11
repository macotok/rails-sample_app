## rails tutorial

[第3章 ほぼ静的なページの作成](https://railstutorial.jp/chapters/static_pages?version=5.1#cha-static_pages)

## 初期設定

### セットアップ

```terminal
$ rails new sample_app
```

### Gemfile

test group追加

```
source 'https://rubygems.org'

gem 'rails',        '5.2'
gem 'puma',         '3.9.1'
gem 'sass-rails',   '5.0.6'
gem 'uglifier',     '3.2.0'
gem 'coffee-rails', '4.2.2'
gem 'jquery-rails', '4.3.1'
gem 'turbolinks',   '5.0.1'
gem 'jbuilder',     '2.6.4'
gem 'bootsnap', require: false

group :development, :test do
  gem 'sqlite3',      '1.3.13'
  gem 'byebug', '9.0.6', platform: :mri
end

group :development do
  gem 'web-console',           '3.5.1'
  gem 'listen',                '3.1.5'
  gem 'spring',                '2.0.2'
  gem 'spring-watcher-listen', '2.0.1'
end

group :test do
  gem 'rails-controller-testing', '1.0.2'
  gem 'minitest-reporters',       '1.1.14'
  gem 'guard',                    '2.13.0'
  gem 'guard-minitest',           '2.4.4'
end

group :production do
  gem 'pg', '0.20.0'
end

# Windows環境ではtzinfo-dataというgemを含める必要があります
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

```terminal
$ bundle install --without production
```

## 静的ページを作る

 - StaticPagesコントローラーを生成
 - home、helpアクションを生成

```terminal
$ rails generate controller StaticPages home help
```

viewファイルを生成

 - app/views/static_pages/home.html.erb
 - app/views/static_pages/help.html.erb

元に戻したいときのコマンド

```terminal
$ rails destroy  controller StaticPages home help
$ rails destroy model User
$ rails db:rollback // 1つ前の状態に戻る
$ rails db:migrate VERSION=0 // 最初の状態に戻る
```

## テスト

```rails generate controller```でテストファイルが作られる

test/controllers/static_pages_controller_test.rb

### テスト内容1

「Homeページのテスト。GETリクエストをhomeアクションに対して送信。リクエストに対するレスポンスは成功」

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest
  test "should get home" do
    get static_pages_home_url
    assert_response :success
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
  end

end
```

### テスト内容2

<title>タグ内に「Home | Ruby on Rails Tutorial Sample App」という文字列があるかどうかをチェック

```ruby
assert_select "title", "Home | Ruby on Rails Tutorial Sample App"
```

### テスト実行

```terminal
$ rails test
```

テスト成功の場合

```terminal
2 runs, 2 assertions, 0 failures, 0 errors, 0 skips
```

### setupメソッド

各テストが実行される直前で実行されるメソッド
共通の文字列を変数にする

```ruby
class StaticPagesControllerTest < ActionDispatch::IntegrationTest
  def setup
    @base_title = "Ruby on Rails Tutorial Sample App"
  end

  test "should get home" do
    get static_pages_home_url
    assert_response :success
    assert_select "title", "Home | #{@base_title}"
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
    assert_select "title", "Help | #{@base_title}"
  end

  test "should get about" do
    get static_pages_about_url
    assert_response :success
    assert_select "title", "About | #{@base_title}"
  end

end
```

## 埋め込みRuby

### provideメソッド

```ruby
<% provide(:title, "Home") %>
<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
```

### layoutファイルで管理

app/views/layouts/application.html.erb

```<%= yield %>```は各viewファイルの記述が読み込まれる

```ruby
<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
```

app/views/static_pages/home.html.erb

```
<% provide(:title, "Home") %>
<h1>Sample App</h1>
<p>
  This is the home page for the
  <a href="https://railstutorial.jp/">Ruby on Rails Tutorial</a>
  sample application.
</p>
```


## 短縮形

|完全なコマンド|短縮形|
|---|---|
|$ rails server|$ rails s|
|$ rails console|$ rails c|
|$ rails generate|$ rails g|
|$ rails test|$ rails t|
|$ bundle install|$ bundle|

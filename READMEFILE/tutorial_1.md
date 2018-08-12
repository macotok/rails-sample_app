# rails Tutorial

[第1章 ゼロからデプロイまで](https://railstutorial.jp/chapters/beginning?version=5.1#cha-beginning)

## 開発環境構築

[http://railsgirls.jp/install](http://railsgirls.jp/install)

## tip

### 初期設定

```
$ rails new hello_qpp
$ bundle install
$ rails server
```

### デプロイ

herokuを使ってデプロイする

```terminal:herokuがインストールされてるか確認
$ heroku --version
```

```terminal:デプロイ
$ heroku login
$ heroku keys:add
$ heroku create // サブドメインが表示される
$ git push heroku master
```

### ルーティング

```ruby:ルーティング方法
root 'controller_name#action_name'
```

```ruby:application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def hello
    render html: "hello, world"
  end
end
```

```ruby:routes.rb
Rails.application.routes.draw do
  root 'application#hello'
end
```

## 参考サイト

- [bootsnapについて調べてみた](https://qiita.com/Daniel_Nakano/items/aadeaa7ae4e227b73878)

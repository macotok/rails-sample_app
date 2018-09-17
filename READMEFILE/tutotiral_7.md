# tutorial

[第7章 ユーザー登録](https://railstutorial.jp/chapters/sign_up?version=5.1#cha-sign_up)

## デバッグ機能を追加

``` ruby:app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
  .
  .
  .
  <body>
    <%= render 'layouts/header' %>
    <div class="container">
      <%= yield %>
      <%= render 'layouts/footer' %>
      <%= debug(params) if Rails.env.development? %>
    </div>
  </body>
</html>
```

### 開発環境/テスト環境/本番環境

開発環境のみ表示

``` ruby
if Rails.env.development?
```

rails consoleで環境を確認

``` terminal:開発環境
$ rails console
 Loading development environment
 >> Rails.env
 => "development"
 >> Rails.env.development?
 => true
 >> Rails.env.test?
 => false
```

``` terminal:テスト環境
$ rails console test
  Loading test environment
  >> Rails.env
  => "test"
  >> Rails.env.test?
  => true
```

``` terminal:本番環境
$ rails server --environment production

$ rails db:migrate RAILS_ENV=production

$ heroku run rails console
>> Rails.env
=> "production"
>> Rails.env.production?
=> true
```

## Usersリソース

- RESTアーキテクチャの習慣に従ってユーザー情報を表示
- データの作成、表示、更新、削除をリソース(Resources)として扱う
- HTTPのrequestメソッド(POST、GET、PATCH、DELETE)を使う

### resources :users

- /users/1 のURLを有効にするために、routesファイル (config/routes.rb)に次の1行を追加
- RESTfulなUsersリソースで必要となるすべてのアクションが利用できる


``` ruby:config/routes.rb
Rails.application.routes.draw do
  root 'static_pages#home'
  get  '/help',    to: 'static_pages#help'
  get  '/about',   to: 'static_pages#about'
  get  '/contact', to: 'static_pages#contact'
  get  '/signup',  to: 'users#new'
  resources :users
end
```

|HTTPリクエスト|URL|アクション|名前付きルート|用途|
----|----|----|----|----
|GET|/users|index|users_path|すべてのユーザーを一覧するページ|
|GET|/users/1|show|user_path(user)|特定のユーザーを表示するページ|
|GET|/users/new|new|new_user_path|ユーザーを新規作成するページ (ユーザー登録)|
|POST|/users|create|users_path|ユーザーを作成するアクション|
|GET|/users/1/edit|edit|edit_user_path(user)|id=1のユーザーを編集するページ|
|PATCH|/users/1|update|user_path(user)|ユーザーを更新するアクション|
|DELETE|/users/1|destroy|user_path(user)|ユーザーを削除するアクション|

### show.html.erb作成

埋め込みRubyを使う

``` ruby:app/views/users/show.html.erb
<%= @user.name %>, <%= @user.email %>
```

インスタンス変数@userを定義

``` ruby:app/controllers/users_controller.rb
class UsersController < ApplicationController

  def show
    @user = User.find(params[:id])
  end

  def new
  end
end
```

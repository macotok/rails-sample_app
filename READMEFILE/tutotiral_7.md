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

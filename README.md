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

StaticPagesコントローラーを生成
home、helpアクションを生成

```terminal
$ rails generate controller StaticPages home help
```

## 短縮形

|完全なコマンド|短縮形|
|---|---|
|$ rails server|$ rails s|
|$ rails console|$ rails c|
|$ rails generate|$ rails g|
|$ rails test|$ rails t|
|$ bundle install|$ bundle|

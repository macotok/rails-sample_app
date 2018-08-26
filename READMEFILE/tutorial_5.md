# rails Tutorial

[第5章 レイアウトを作成する](https://railstutorial.jp/chapters/filling_in_the_layout?version=5.1#cha-filling_in_the_layout)

## Railsヘルパー

### link_to

link_toの第1引数はリンクテキスト、第2引数はURL。第3引数はオプションハッシュで、この場合はサンプルアプリのリンクでCSS id logoを指定。

``` ruby
<%= link_to "sample app", "#", id: "logo" %>
```

### image_tag

画像ファイルをアセットパイプラインを通してapp/assets/images/ディレクトリの中から探す

``` ruby
image_tag("rails.png", alt: "Rails logo")
```

HTMLのソース

``` html
<img alt="Rails logo" src="/assets/rails-9308b8f92fea4c19a3a0d8385b494526.png" />
```

 - ファイル名が重ならないようにするために「9308b8f92fea4c19a3a0d8385b494526」という文字列をRailsが追加している
 - src属性には "images" というディレクトリ名が含まれていない。これはassetsディレクトリ内の他のディレクトリ (imagesやjavascripts、stylesheetsなど) も同様。これは高速化のための仕組みで、Railsはassetsディレクトリ直下の画像をapp/assets/imagesディレクトリにある画像と紐付けている。このようなフラットなディレクトリ構成を採っていると、ファイルをより高速にブラウザに渡すことができる。

### render(パーシャル)

``` ruby
<%= render 'layouts/shim' %>
```

上記のrenderだと```app/views/layouts/_shim.html.erb```を探す。
ファイル名の先頭のアンダースコアは、パーシャルで使う普遍的な命名規約。

## Sass

### bootstrapを適用

Gemfileに「bootstrap-sass」を追加

``` ruby:Gemfile
gem 'bootstrap-sass', '3.3.7'
```

``` terminal
$ bundle install
```

``` css:app/assets/stylesheets/custom.scss
@import "bootstrap-sprockets";
@import "bootstrap";
```

### application.css

```app/assets/stylesheets/```に置かれたスタイルシートはapplication.cssの一部としてWebサイトのレイアウトに読み込まれる。

## アセットパイプライン

### アセットディレクトリ

Railsのアセットパイプラインでは、静的ファイルを目的別に分類する、標準的な3つのディレクトリが使われている。

- app/assets: 現在のアプリケーション固有のアセット
- lib/assets: あなたの開発チームによって作成されたライブラリ用のアセット
- vendor/assets: サードパーティのアセット

### マニフェストファイル

静的ファイルをそれぞれ配置すれば、マニフェストファイルを使って、それらをどのように1つのファイルにまとめるのかをRailsに指示することができます。
なお、実際にアセットをまとめる処理を行うのはSprocketsというgemです。
また、マニフェストファイルはCSSとJavaScriptに適用される。

#### CSS用のマニフェストファイル

```css:app/assets/stylesheets/application.css
*= require_tree .
*= require_self
```

```*= require_tree .```はapp/assets/stylesheetsディレクトリ (サブディレクトリを含む) 中のすべてのCSSファイルが、アプリケーションCSSに含まれる。

```*= require_self```はCSSの読み込みシーケンスの中でapplication.css自身もその対象に含めている。

### プリプロセッサエンジン

必要なアセットをディレクトリに配置してまとめた後、Railsはさまざまなプリプロセッサエンジンを介してそれらを実行し、ブラウザに配信できるようにする。

Railsはどのプリプロセッサを使うのかを、ファイル名の拡張子を使って判断します。
最も一般的な拡張子は、Sass用の.scss、CoffeeScript用の.coffee、埋め込みRuby (ERb) 用の.erbです。

例)

- foobar.js.coffee
- foobar.js.erb.coffee (この例ではCoffeeScriptが最初に実行される)

## レイアウトのリンク

名前付きルートを使って指定

``` ruby
<%= link_to "About", about_path %>
```

### サイトリンクのルーティングとURLのマッピング

| ページ名 | URL | 名前付きルート |
----|----|----
|Home|/|root_path|
|About|/about|about_path|
|Help|/help|help_path|
|Contact|/contact|contact_path|
|Sign up|/signup|signup_path|
|Log in|/login|login_path|

### ルートURL

基本的には_path書式を使い、リダイレクトの場合のみ_url書式を使う。

```
root_path -> '/'
root_url  -> 'http://www.example.com/'
```

```ruby:config/routes.rb
Rails.application.routes.draw do
  root 'static_pages#home'
  get '/help', to: 'static_pages/help'
  get '/about', to: 'static_pages/about'
  get '/contact', to: 'static_pages/contact'
end
```

getルールを使うとルートURLのときと同様に、help_pathやhelp_urlといった名前付きルートも使えるようになります。

```
help_path -> '/help'
help_url  -> 'http://www.example.com/help'
```

html.erbに適用

```ruby
<li><%= link_to "Home", root_path %></li>
<li><%= link_to "Help", help_path %></li>
```

## 統合テスト

ファイル生成

```terminal
$ rails generate integration_test site_layout
      invoke  test_unit
      create    test/integration/site_layout_test.rb
```

### テスト内容

1. ルートURL (Homeページ) にGETリクエストを送る.
2. 正しいページテンプレートが描画されているかどうか確かめる.
3. Home、Help、About、Contactの各ページへのリンクが正しく動くか確かめる。

```ruby:test/integration/site_layout_test.rb
require 'test_helper'

class SiteLayoutTest < ActionDispatch::IntegrationTest

  test "layout links" do
    get root_path
    assert_template 'static_pages/home'
    assert_select "a[href=?]", root_path, count: 2
    assert_select "a[href=?]", help_path
    assert_select "a[href=?]", about_path
    assert_select "a[href=?]", contact_path
  end
end
```

- Railsは自動的にはてなマーク "?" をpathに置換している
- count2はルートURLへのリンクが2つ存在しているため

テスト実行

```terminal
$ rails test:integration
```

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

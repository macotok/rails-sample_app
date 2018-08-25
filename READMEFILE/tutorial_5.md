# rails Tutorial

[第5章 レイアウトを作成する](https://railstutorial.jp/chapters/filling_in_the_layout?version=5.1#cha-filling_in_the_layout)

## Railsヘルパー

### link_to

link_toの第1引数はリンクテキスト、第2引数はURL。第3引数はオプションハッシュで、この場合はサンプルアプリのリンクでCSS id logoを指定。

``` ruby
<%= link_to "sample app", '#', id: "logo" %>
```

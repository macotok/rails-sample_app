# rails Tutorial

[第6章 ユーザーのモデルを作成する](https://railstutorial.jp/chapters/modeling_users?version=5.1#cha-modeling_users)

## Userモデル

- Railsでは、データモデルとして扱うデフォルトのデータ構造のことをモデル (Model) と呼ぶ
- データベースとやりとりをするデフォルトのRailsライブラリはActive Recordと呼ぶ
- Active Recordは、データオブジェクトの作成/保存/検索のためのメソッドを持っている
- これらのメソッドを使うのに、リレーショナルデータベースで使うSQLを意識する必要がない
- Railsにはマイグレーションという機能があり、データの定義をRubyで記述することができのでSQLのDDLを新たに学ぶ必要がありません

### Userモデルを生成する

- コントローラ名には複数形を使い、モデル名には単数形を用いる(コントローラはUsersでモデルはUser)
- 属性の型情報も一緒に渡す。

``` terminal
$ rails generate model User name:string email:string
```

| users |
----|----
|id|integer|
|name|string|
|email|string|

### マイグレーションファイル

generateコマンドの結果のひとつとしてマイグレーションと呼ばれる新しいファイルが生成される

```ruby:db/migrate/[timestamp]_create_users.rb
class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
```

- マイグレーション自体は、データベースに与える変更を定義したchangeメソッドの集まり
- changeメソッドはcreate_tableというRailsのメソッドを呼び、ユーザーを保存するためのテーブルをデータベースに作成
- モデル名は単数形 (User) ですが、テーブル名は複数形 (users)
- .timestampsは特別なコマンドで、created_atとupdated_atという２つの「マジックカラム (Magic Columns)」を作成(あるユーザーが作成または更新されたときに、その時刻を自動的に記録するタイムスタンプ)

| users |
----|----
|id|integer|
|name|string|
|email|string|
|created_at|datetime|
|updated_at|datetime|

### マイグレーションの適用

```terminal
$ rails db:migrate
```

### マイグレーションを元に戻す

```terminal
$ rails db:rollback
```

- drop_tableが適用されるので```db/schema.rb```が空になる
- あるカラムを削除する場合は、changeメソッドの代わりに、upとdownのメソッドを別々に定義する

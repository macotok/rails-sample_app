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

マイグレーション適用で```development.sqlite3```が更新される。
また、```app/models/user.rb```が作成される


### マイグレーションを元に戻す

```terminal
$ rails db:rollback
```

- drop_tableが適用されるので```db/schema.rb```が空になる
- あるカラムを削除する場合は、changeメソッドの代わりに、upとdownのメソッドを別々に定義する

## ユーザーオブジェクトを作成する

Railsコンソールでモデルの操作が可能

```terminal
$ rails console --sandbox
```

sandboxをつけることで「データベースへの変更をコンソールの終了時にすべて “ロールバック” (取り消し) する」設定になる

### User.new

User.newで新しいオブジェクトを生成

```terminal
>> user = User.new(name: "Michael Hartl", email: "mhartl@example.com")
=> #<User id: nil, name: "Michael Hartl", email: "mhartl@example.com",
created_at: nil, updated_at: nil>
```

userオブジェクトが有効かどうかを確認

```terminal
>> user.valid?
true
```

### User.save

現時点ではまだデータベースにデータは格納されていません。
つまり、User.newはメモリ上でオブジェクトを作成しただけです。

データベースにUserオブジェクトを保存するためには、userオブジェクトからsaveメソッドを呼び出す必要があります。

```terminal
>> user.save
   (0.1ms)  SAVEPOINT active_record_1
  SQL (0.8ms)  INSERT INTO "users" ("name", "email", "created_at",
  "updated_at") VALUES (?, ?, ?, ?)  [["name", "Michael Hartl"],
  ["email", "mhartl@example.com"], ["created_at", 2016-05-23 19:05:58 UTC],
  ["updated_at", 2016-05-23 19:05:58 UTC]]
   (0.1ms)  RELEASE SAVEPOINT active_record_1
=> true
```

saveメソッドは、成功すればtrueを、失敗すればfalseを返す。

```terminal
>> user
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2016-05-23 19:05:58", updated_at: "2016-05-23 19:05:58">
```

saveメソッド後、```id```、```created_at```、```updated_at```に値が入る。

### User.create

Active RecordではUser.createでモデルの生成と保存を同時におこなう方法も提供されている

```terminal
>> User.create(name: "A Nother", email: "another@example.org")
#<User id: 2, name: "A Nother", email: "another@example.org", created_at:
"2016-05-23 19:18:46", updated_at: "2016-05-23 19:18:46">
```

User.createは、trueかfalseを返す代わりに、ユーザーオブジェクト自身を返す

### User.destroy

```terminal
>> foo = User.create(name: "Foo", email: "foo@bar.com")
#<User id: 3, name: "Foo", email: "foo@bar.com", created_at: "2016-05-23
19:19:06", updated_at: "2016-05-23 19:19:06">

>> foo.destroy
   (0.1ms)  SAVEPOINT active_record_1
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 3]]
   (0.1ms)  RELEASE SAVEPOINT active_record_1
=> #<User id: 3, name: "Foo", email: "foo@bar.com", created_at: "2016-05-23
19:19:06", updated_at: "2016-05-23 19:19:06">
```

- createと同じようにdestroyはそのオブジェクト自身を返しますが、その戻り値を使ってもう一度destroyを呼ぶことはできません。
- 削除されたオブジェクトはまだメモリ上には残っている(```>> foo```)

### 検索

.find

``` terminal
>> User.find(1)
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2016-05-23 19:05:58", updated_at: "2016-05-23 19:05:58">
```

.find_by

``` terminal
>> User.find_by(email: "mhartl@example.com")
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2016-05-23 19:05:58", updated_at: "2016-05-23 19:05:58">
```

.first

``` terminal
>> User.first
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2016-05-23 19:05:58", updated_at: "2016-05-23 19:05:58">
```

.all

``` terminal
>> User.all
=> #<ActiveRecord::Relation [#<User id: 1, name: "Michael Hartl",
email: "mhartl@example.com", created_at: "2016-05-23 19:05:58",
updated_at: "2016-05-23 19:05:58">, #<User id: 2, name: "A Nother",
email: "another@example.org", created_at: "2016-05-23 19:18:46",
updated_at: "2016-05-23 19:18:46">]>
```

その他、オブジェクトなので```.length```も使える

``` terminal
>> User.all.length
=> 2
```

### 更新

.save

``` terminal
>> user           # userオブジェクトが持つ情報のおさらい
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2016-05-23 19:05:58", updated_at: "2016-05-23 19:05:58">
>> user.email = "mhartl@example.net"
=> "mhartl@example.net"
>> user.save
=> true
```

.saveをしないと更新されない

``` terminal
>> user.email
=> "mhartl@example.net"
>> user.email = "foo@bar.com"
=> "foo@bar.com"
>> user.reload.email
=> "mhartl@example.net"
```

.update_attributes

``` terminal
>> user.update_attributes(name: "The Dude", email: "dude@abides.org")
=> true
>> user.name
=> "The Dude"
>> user.email
=> "dude@abides.org"
```

``` terminal
>> user.update_attribute(:name, "El Duderino")
=> true
>> user.name
=> "El Duderino"
```

## validationとテスト駆動開発

### Userオブジェクトが有効か

 - setupは各テストが走る直前で実行される
 - @userはインスタンス変数
 - valid?は有効かを判定

``` ruby:test/model/user_test.rb
require 'test_helper'

class UserTest < ActiveSupport::TestCase

  def setup
    @user = User.new(name: "Example User", email: "user@example.com")
  end

  test "should be valid" do
    assert @user.valid?
  end
end
```

test実行

``` terminal
$ rails test:models
```

### validate presence

ユーザーがデータベースに保存される前にnameとemailフィールドの両方が存在することを保証する場合

``` ruby:test/models/user_test.rb
require 'test_helper'

class UserTest < ActiveSupport::TestCase

  def setup
    @user = User.new(name: "Example User", email: "user@example.com")
  end

  test "should be valid" do
    assert @user.valid?
  end

  test "name should be present" do
    @user.name = ""
    assert_not @user.valid?
  end

  test "email should be present" do
    @user.email = ""
    assert_not @user.valid?
  end
end
```

``` ruby:app/models/user.rb
class User < ApplicationRecord
  validates :name,  presence: true
  validates :email, presence: true
end
```

error messageを表示

``` terminal
>> user.errors.full_messages
=> ["Name can't be blank"]

>> user.errors.messages
=> {:name=>["can't be blank"], :email=>["can't be blank"]}

>> user.errors.messages[:email]
=> ["can't be blank"]
```

### validate length

``` ruby:test/models/user_test.rb
require 'test_helper'

class UserTest < ActiveSupport::TestCase

  def setup
    @user = User.new(name: "Example User", email: "user@example.com")
  end

  .
  .
  ,

  test "name should not be too lond" do
    @user.name = "a" * 51
    assert_not @user.valid?
  end

  test "email should not be too long" do
    @user.email = "a" * 244 + "@example.com"
    assert_not @user.valid?
  end
end
```

``` ruby:app/models/user.rb
class User < ApplicationRecord
  validates :name, presence: true, length: { maximum: 50 }
  validates :email, presence: true, length: { maximum: 255 }
end
```

### validates format mail-address

有効なメールフォーマットと無効なメールフォーマットをテスト

``` ruby:test/models/user_test.rb
require 'test_helper'

class UserTest < ActiveSupport::TestCase

  def setup
    @user = User.new(name: "Example User", email: "user@example.com")
  end

  .
  .
  ,

  test "email validation should accept valid addresses" do
    valid_addresses = %w[uesr@example.com USER@foo.COM A_US-ER@foo.bar.org first.last@foo.jp alice+bob@baz.cn]
    valid_addresses.each do |valid_address|
      @user.email = valid_address
      assert @user.valid?, "#{valid_address.inspect} should be valid"
    end
  end

  test "email validation should reject invalid addresses" do
    invalid_addresses = %w[uesr@example,com user_at_foo.org user.name@example. foo@bar_baz.com foo@bar+baz.com foo@bar..com]
    invalid_addresses.each do |invalid_address|
      @user.email = invalid_address
      assert_not @user.valid?, "#{invalid_address.inspect} should be invalid"
    end
  end
end
```

``` ruby:app/models/user.rb
class User < ApplicationRecord
  validates :name, presence: true, length: { maximum: 50 }
  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
  validates :email, presence: true, length: { maximum: 255 }, format: { with: VALID_EMAIL_REGEX }
end
```

メールアドレスが有効かを調べ正規表現

| 正規表現 | 意味 |
----|----
|/\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i | (完全な正規表現)|
|/ | 正規表現の開始を示す |
|\A |	文字列の先頭 |
|[\w+\-.]+ | 英数字、アンダースコア (_)、プラス (+)、ハイフン (-)、ドット (.) のいずれかを少なくとも1文字以上繰り返す|
|@ | アットマーク|
| [a-z\d\-.]+ | 英小文字、数字、ハイフン、ドットのいずれかを少なくとも1文字以上繰り返す|
|\. | ドット|
|[a-z]+ | 英小文字を少なくとも1文字以上繰り返す|
|\z | 文字列の末尾|
|/ | 正規表現の終わりを示す|
|i | 大文字小文字を無視するオプション|

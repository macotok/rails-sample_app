# rails turorial

[第2章 Toyアプリケーション](https://railstutorial.jp/chapters/toy_app?version=5.1#cha-a_toy_app)

## scaffoldを使って簡易アプリケーション作成

### Usersモデル

| 名前 | 型 |
----|----
|id|integer|
|name|string|
|email|string|


``` terminal
$ rails generate scaffold User name:string email:string
```

``` terminal:DBをmigrate
$ rails db:migrate
```

User一覧ページが作られる
[http://localhost:3000/users](http://localhost:3000/users)

### controllerの中身

app/controllers/users_controller.rb

```ruby
class UsersController < ApplicationController
  before_action :set_user, only: [:show, :edit, :update, :destroy]

  # GET /users
  # GET /users.json
  def index
    @users = User.all
  end

  # GET /users/1
  # GET /users/1.json
  def show
  end

  # GET /users/new
  def new
    @user = User.new
  end

  # GET /users/1/edit
  def edit
  end

  # POST /users
  # POST /users.json
  def create
    @user = User.new(user_params)

    respond_to do |format|
      if @user.save
        format.html { redirect_to @user, notice: 'User was successfully created.' }
        format.json { render :show, status: :created, location: @user }
      else
        format.html { render :new }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end
  end

  # PATCH/PUT /users/1
  # PATCH/PUT /users/1.json
  def update
    respond_to do |format|
      if @user.update(user_params)
        format.html { redirect_to @user, notice: 'User was successfully updated.' }
        format.json { render :show, status: :ok, location: @user }
      else
        format.html { render :edit }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /users/1
  # DELETE /users/1.json
  def destroy
    @user.destroy
    respond_to do |format|
      format.html { redirect_to users_url, notice: 'User was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_user
      @user = User.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def user_params
      params.require(:user).permit(:name, :email)
    end
end
```

Userモデルからすべてのユーザーの一覧を取り出し@usersという変数に保存

```ruby
def index
  @users = User.all
end
```

viewに反映させる

app/views/users/index.html.erb

```ruby
<% @users.each do |user| %>
  <tr>
    <td><%= user.name %></td>
    <td><%= user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, method: :delete, data: { confirm: 'Are you sure?' } %></td>
  </tr>
```

## RESTアーキテクチャ

アクション一覧

|HTTPリクエスト|URL|アクション|用途|
----|----|----|----
|GET|/users|index|すべてのユーザーを一覧するページ|
|GET|/users/1|show|id=1のユーザーを表示するページ|
|GET|/users/new|new|新規ユーザーを作成するページ|
|POST|/users|create|ユーザーを作成するアクション|
|GET|/users/1/edit|edit|id=1のユーザーを編集するページ|
|PATCH|/users/1|update|id=1のユーザーを更新するアクション|
|DELETE|/users/1|destroy|id=1のユーザーを削除するアクション|

## validates

app/models/micropost.rb

```ruby
class Micropost < ApplicationRecord
  validates :content, length: { maximum: 140 }, presence: true
end
```

## データモデル同士の関連付け

app/models/user.rb

```ruby
class User < ApplicationRecord
  has_many :micrposts
end
```

app/models/micropost.rb

```ruby
class Micropost < ApplicationRecord
  belong_to :user
  validates :content, length: { maximum: 140 }
end
```

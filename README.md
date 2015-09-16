StudyOnRails4
=============

#ドットインストール見ながらRuby on Rails 4入門

[ドットインストール Ruby on Rails 4入門 (全28回)](http://dotinstall.com/lessons/basic_rails_v2)

##1~3
### 環境設定
* ruby 2.0.0-p247
* Rails 4.0.0

を使用したいのだが、現在PCに入っているversionと違う。  
  
ruby&railsのバージョンの組み合わせをプロジェクトごとに変えたいので下記ページを参考にコマンド実行。  
[Ruby を使うなら「rbenv」で複数バージョンを切り替えられるようにしておこう](http://h2ham.net/ruby-rbenv)  
[Rails 3.2と4.0、複数バージョンをインストールする](http://www.airpucci.com/2013/09/rails-3-2%E3%81%A84-0%E3%80%81%E8%A4%87%E6%95%B0%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/)  
[Ruby1.9+Rails3.2に加えて、Ruby2.0+Rails4.0のプロジェクトを作る。](http://www.airpucci.com/2013/09/ruby1-9rails3-2%E3%81%AB%E5%8A%A0%E3%81%88%E3%81%A6%E3%80%81ruby2-0rails4-0%E3%81%AE%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%82%92%E4%BD%9C%E3%82%8B%E3%80%82/)


```sh:
# 作成
rbenv install 2.0.0-p247    # rubyインストール
rbenv local 2.0.0-p247      # このディレクトリのみ2.0.0-p247で動作させる
ruby --version              # 確認
gem install rails --version "=4.0.0"  # railsインストール
rails _4.0.0_ new myapp     # rails4.0.0にてプロジェクト作成
# 起動
cd myapp
rails s
```

作成画面  
[http://0.0.0.0:3000](http://0.0.0.0:3000)


##4

```sh:
rails generate scaffold User name:string score:integer  # db設定作成
rake db:migrate  # dbに反映
```

作成画面  
[http://0.0.0.0:3000/users](http://0.0.0.0:3000/users)


##5

```sh:
rails _4.0.0_ new taskapp
rails g model Project title:string   # modelは単数頭大文字、stringはdefaultなので省略可
rake db:migrate  # dbに反映
```

##6

db

```sh:
rails db  # db内確認
sqlite>
.schema   # 5でmigrateしたschemaを確認
.exit     # 終了
```

console

```sh:
rails console # コンソール開始
irb(main):001:0>
p = Project.new(title:"p1") # 作成
p.save                      # 保存
p                           # 確認
Project.create(title:"p2")  # 作成 & 保存
Project.all                 # 全部確認
quit          # コンソール終了
```

dbにて作成確認

```sh:
rails db  # db内確認
select * from projects; 確認
```


##7

controllerの作成

```sh:
rails g controller Projects   # Projectsとsが付き複数形になっている
# config/routes.rb に下記記入。(projectsページのURI表示設定)
   resources :projects
rake routes   # ルーティングの確認(超大事！！！！！！！！！！！！！)
>      Prefix Verb   URI Pattern                  Controller#Action
>    projects GET    /projects(.:format)          projects#index
>             POST   /projects(.:format)          projects#create	# project_controller.rbのcreateに書けよって意味。
> new_project GET    /projects/new(.:format)      projects#new
>edit_project GET    /projects/:id/edit(.:format) projects#edit
>     project GET    /projects/:id(.:format)      projects#show
>             PATCH  /projects/:id(.:format)      projects#update
>             PUT    /projects/:id(.:format)      projects#update
>             DELETE /projects/:id(.:format)      projects#destroy
```

##8
routesに表示されていたprojects#indexの作成  
controllerにindexを作成。  
modified:   taskapp/app/controllers/projects_controller.rb  

```ruby:projects_controller.rb
class ProjectsController < ApplicationController
	def index
		@projects = Project.all
	end
end
```

viewファイルを同じ位置に同じindexという名前で作成。  
new file:   taskapp/app/views/projects/index.html.erb  

```erb:index.html.erb
<h1>Projects</h1>
<ul>
<% @projects.each do |project| %>
	<li><%= project.title %></li>
<% end %>
</ul>
```

作成画面  
[http://0.0.0.0:3000/projects](http://0.0.0.0:3000/projects)  

##9
projectsページをrootページに設定  
modified:   taskapp/config/routes.rb  
下記追記。  

```ruby:routes.rb
  root 'projects#index'
```

作成画面  
[http://0.0.0.0:3000](http://0.0.0.0:3000)  

##10
共有テンプレートの編集  
modified:   taskapp/app/views/layouts/application.html.erb  
<%= yield %>周りに下記追記。  

```ruby:application.html.erb
<%= image_tag "logo.png" %>

<%= yield %>

<%= link_to "Home", "/" %>
<%= link_to "Projects", projects_path %>
```

app/assets/images/logo.png画像の表示  
rootページリンクのHome文字  
projectsページリンクのProjects文字  

```sh:
rake routes
>      Prefix Verb   URI Pattern                  Controller#Action
>    projects GET    /projects(.:format)          projects#index
>             POST   /projects(.:format)          projects#create
> new_project GET    /projects/new(.:format)      projects#new
>edit_project GET    /projects/:id/edit(.:format) projects#edit
>     project GET    /projects/:id(.:format)      projects#show
>             PATCH  /projects/:id(.:format)      projects#update
>             PUT    /projects/:id(.:format)      projects#update
>             DELETE /projects/:id(.:format)      projects#destroy
```

リンクパスは上記rake routesにて表示されるPrefixに_pathとしてつなげればなる。  
作成画面  
[http://0.0.0.0:3000](http://0.0.0.0:3000)  


##11 Projectsの詳細を表示しよう

project一覧から個別ページへのリンクを設定しましょう。  
 1. リンク設定。  
 2. ページに渡す値の生成。  
 3. 詳細ページhtmlの作成。  
が手順になります。  

```erb:index.html.erb
	<li><%= link_to project.title, project_path(project.id) %></li>
```

project_pathはroutesにあるように、引数にidを待ちます。

```sh:
rake routes
...
>     project GET    /projects/:id(.:format)      projects#show
...
```

controllerの修正をしていきます。  
routesにあるようにshowメソッドの追加です。  

```ruby:projects_controller.rb
class ProjectsController < ApplicationController
	def index
		@projects = Project.all
	end

	def show
		@project = Project.find(params[:id])
	end
end
```

show.html.erbを作成します。  
projectのtitleのみ表示します。  

```erb:show.html.erb
<h1><%= @project.title %></h1>
```


##12 新規作成フォームを作ろう

projects#newの値をPOSTでprojects#createが受け取る手順になります。  
まずはindexにnewページへのリンク作成とnewページにform作成をします。  

```sh:
rake routes
...
>             POST   /projects(.:format)          projects#create
> new_project GET    /projects/new(.:format)      projects#new
...
```

indexにnewページへのリンク作成。

```erb:index.html.erb
<p><%= link_to "add New", new_project_path %></p>
```


```ruby:projects_controller.rb
class ProjectsController < ApplicationController
	def index
		@projects = Project.all
	end

	def new
		@project = Project.new
	end
end
```

```erb:new.html.erb
<h1>Add New</h1>

<%= form_for @project do |f| %>

	<p>
		<%= f.label :title %><br>
		<%= f.text_field :title %>
	</p>

	<p>
		<%= f.submit %>
	</p>
<% end %>
```

##13 データを保存してみよう

create部分を実装しデータを保存します。  
privateに記述たように、セキュリティ的にparamsをpermitで制限するのが最近の作り方になります。  

```ruby:projects_controller.rb
...
	def create
		@project = Project.new(project_params)
		@project.save
		redirect_to projects_path
	end

	private
		def project_params
			params[:project].permit(:title)
		end
end
```

##14 Validationを設定しよう

ModelにValidationを設定し、適切なデータが保存されるようにします。  

```ruby:models/project.rb
class Project < ActiveRecord::Base
	validates :title, presence: true
end
```

models/project.rb に上記を記述します。  
presence: trueにすることで空の値を許可しなくなります。  
この状態で登録ボタンを押すと登録はされないようになりますが、一覧画面に戻ってしまいます。  
ですのでcontrollerのcreateを修正していきます。

```ruby:projects_controller.rb
...
	def create
		@project = Project.new(project_params)
		if @project.save
			redirect_to projects_path
		else
			render "new"
		end
	end
```

@project.saveは成否をbooleanで返すので、  
falseの場合は現在のページを戻す挙動に変更します。


##15 エラーメッセージを表示しよう

引き続きerror messageを表示してみましょう。

```erb:new.html.erb
...
	<p>
		<%= f.label :title %><br>
		<%= f.text_field :title %>
		<% if @project.errors.any? %>
		<%#= @project.errors.inspect %>
		<%= @project.errors.messages[:title][0] %>
		<% end %>
	</p>
...
```

new.html.erb  
@project.errors.inspectによりerrorsの中身が確認できるので、  
@project.errors.messages[:title][0] を入力すべき事が確認できるはずです。  

```
can't be blank
```

このメッセージを変更するには


```ruby:models/project.rb
class Project < ActiveRecord::Base
#	validates :title, presence: true
	validates :title, presence: {message: "入力してください"}

end
```

上記のように編集しましょう。  
入力文字数のチェックも下記のようにすれば対応できます。

```ruby:models/project.rb
class Project < ActiveRecord::Base
	validates :title,
	presence: {message: "入力してください"},
	length: {minimum: 3, message: "短すぎ！"}
end
```


##16 編集フォームを作ろう

編集フォームするために編集する場所はroutingで確認できます。
下記の#edit1つと#update2つになります。

```sh:
rake routes
...
>edit_project GET    /projects/:id/edit(.:format) projects#edit
>     project GET    /projects/:id(.:format)      projects#show
>             PATCH  /projects/:id(.:format)      projects#update
>             PUT    /projects/:id(.:format)      projects#update
...
```

まずは[edit]リンクを作成していきましょう。

```erb:index.html.erb
<h1>Projects</h1>
<ul>
<% @projects.each do |project| %>
	<li>
		<%= link_to project.title, project_path(project.id) %>
		<%= link_to "[edit]", edit_project_path(project.id) %>
	</li>
<% end %>
</ul>

<p><%= link_to "Add New", new_project_path %></p>
```

[edit]の1行を追加しました。  
  
次に編集ページを作成していきましょう。  
controllerにメソッドの追加と、edit.html.erbの作成です。

```ruby:projects_controller.rb
...
	def edit
		@project = Project.find(params[:id])
	end
...
```

edit.html.erbはnew.html.erbをコピーしましょう。titleの編集のみで大丈夫で問題なく表示されるはずです。  
また、下部のボタン文字も自動的にupdateに変更されているはずです。


##17 データを更新しよう

ではupdateの処理を追加していきましょう。  
controllerにメソッド追加になります。


```ruby:projects_controller.rb
...
	def update
		@project = Project.find(params[:id])
		if @project.update(project_params)
			redirect_to projects_path
		else
			render "edit"
		end
	end
...
```

メソット内容は上記で完了になります。  
うまくいきますが、newとeditの際のフォームhtml部分が同じことに気づくはずです。  
そうゆうところは共通化していきましょう。

```erb:edit.html.erb
<h1>Edit</h1>

<%= render "form" %>

```

newとedit.html.erbは上記の様にすっきりされ、  
下記の_form.html.erbに共通部分を記述します。


```erb:_form.html.erb
...
	def update
		@project = Project.find(params[:id])
		if @project.update(project_params)
			redirect_to projects_path
		else
			render "edit"
		end
	end
...
```

これで共通化は完了です。


##18 データを削除しよう

続いて削除にとりかかりましょう。  
同じようにindex.htmlにリンクの作成と  
controllerにメソッドの追加、  

```erb:index.html.erb
...
		<%= link_to project.title, project_path(project.id) %>
		<%= link_to "[edit]", edit_project_path(project.id) %>
		<%= link_to "[delete]", project_path(project.id), method: :delete, data: {confirm:"消していいん？"} %>
...
```

下に作成したリンク部分になります。  
confirmでalert確認が入ります。

```ruby:projects_controller.rb
...
	def destroy
		@project = Project.find(params[:id])
		@project.destroy
		redirect_to projects_path
	end
...
```

これでdestroy処理が完了になります。


##19 before_actionを使ってみよう

controller部分に重複した記述があるので共通化していきましょう。  

```ruby:projects_controller.rb
class ProjectsController < ApplicationController

	before_action :set_project, only: [:show, :edit, :update, :destroy]

	def index
	...
	...
		def set_project
			@project = Project.find(params[:id])
		end
	...
```

のように記述すると、:show, :edit, :update, :destroy]内にある  
@project = Project.find(params[:id]) の記述を削除できます。

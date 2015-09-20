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
% rbenv install 2.0.0-p247    # rubyインストール
% rbenv local 2.0.0-p247      # このディレクトリのみ2.0.0-p247で動作させる
% ruby --version              # 確認
% gem install rails --version "=4.0.0"  # railsインストール
% rails _4.0.0_ new myapp     # rails4.0.0にてプロジェクト作成
# 起動
% cd myapp
% rails s
```

作成画面  
[http://0.0.0.0:3000](http://0.0.0.0:3000)


##4

```sh:
% rails generate scaffold User name:string score:integer  # db設定作成
% rake db:migrate  # dbに反映
```

作成画面  
[http://0.0.0.0:3000/users](http://0.0.0.0:3000/users)


##5

```sh:
% rails _4.0.0_ new taskapp
% rails g model Project title:string   # modelは単数頭大文字、stringはdefaultなので省略可
% rake db:migrate  # dbに反映
```

##6

db

```sh:
% rails db  # db内確認
sqlite>
.schema   # 5でmigrateしたschemaを確認
.exit     # 終了
```

console

```sh:
% rails console # コンソール開始
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
% rails db  # db内確認
select * from projects; 確認
```


##7

controllerの作成

```sh:
% rails g controller Projects   # Projectsとsが付き複数形になっている
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
% rake routes
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
% rake routes
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
% rake routes
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
% rake routes
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


##20 Tasksの設定をしていこう

projectが出来てきたので、その中身にTaskを表示していきましょう。
手順としては今までどおり  
 1. modelの作成  
 2. dbのmigrate  
 3. controllerの作成  
 4. routingの設定  
となります。  
ではまずmodelを用意します。

```sh:
# string型のtitleは型宣言の省略が出来ます。
# 他テーブルへの参照は
#   project:references
# のように Table名:references で出来ます。
% rails g model Task title done:boolean project:references
      invoke  active_record
      create    db/migrate/20150916133801_create_tasks.rb
      create    app/models/task.rb
      invoke    test_unit
      create      test/models/task_test.rb
      create      test/fixtures/tasks.yml
```

dbのmigrateをする前に、  
db/migrate/20150916133801_create_tasks.rbで作成されたmigrateファイルを少し修正しておきます。

```ruby:db/migrate/20150916133801_create_tasks.rb
class CreateTasks < ActiveRecord::Migration
  def change
    create_table :tasks do |t|
      t.string :title
#      t.boolean :done
      t.boolean :done, default:false
      t.references :project, index: true

      t.timestamps
    end
  end
end
```

編集が済んだらdbのmigrateをしていきます。

```sh:
% rake db:migrate
==  CreateTasks: migrating ====================================================
-- create_table(:tasks)
   -> 0.0082s
==  CreateTasks: migrated (0.0083s) ===========================================
```

続いてcontrollerの作成です。

```sh:
% rails g controller Tasks
      create  app/controllers/tasks_controller.rb
      invoke  erb
      create    app/views/tasks
      invoke  test_unit
      create    test/controllers/tasks_controller_test.rb
      invoke  helper
      create    app/helpers/tasks_helper.rb
      invoke    test_unit
      create      test/helpers/tasks_helper_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/tasks.js.coffee
      invoke    scss
      create      app/assets/stylesheets/tasks.css.scss
```

上記のように作成されたはずです。  
routingは長くなりそうなので次回になります。


##21 Associationの設定をしよう

modelの関連付けを忘れていたので設定していきます。  
Taskに関しては  
% rails g model Task title done:boolean project:references  
のコマンドでapp/models/task.rbにbelogn_toが記述されます。  

```ruby:app/models/task.ruby
class Task < ActiveRecord::Base
  belongs_to :project
end
```

しかし、app/models/project.rbには関連付けがされていなので記述します。

```ruby:app/models/project.rb
class Project < ActiveRecord::Base
	has_many :tasks,
	validates :title,
	presence: {message: "入力してください"},
	length: {minimum: 3, message: "短すぎ！"}
end
```

続いてroutingです。

```ruby:config/routes.rb
...
#  resources :projects
  resources :projects do
    resources :tasks, only: [:create, :destroy]
  end
...
```

実際に必要となるroutingは作成&削除時のみなのでonlyで記述しておきます。  
rake routesで確認してみましょう。

```sh:
% rake routes                                                        
       Prefix Verb   URI Pattern                               Controller#Action
project_tasks POST   /projects/:project_id/tasks(.:format)     tasks#create
 project_task DELETE /projects/:project_id/tasks/:id(.:format) tasks#destroy
     projects GET    /projects(.:format)                       projects#index
              POST   /projects(.:format)                       projects#create
  new_project GET    /projects/new(.:format)                   projects#new
 edit_project GET    /projects/:id/edit(.:format)              projects#edit
      project GET    /projects/:id(.:format)                   projects#show
              PATCH  /projects/:id(.:format)                   projects#update
              PUT    /projects/:id(.:format)                   projects#update
              DELETE /projects/:id(.:format)                   projects#destroy
         root GET    /                                         projects#index
```
project_tasksのcreateとdestroyの2行が追加されているはずです。


##22 Tasksの新規作成フォームを作ろう

プロジェクト内にtaskを表示していきましょう。  
具体的にはviews/projects/show.html.erbにformと一覧の作成になります。


```ruby:views/projects/show.html.erb
<h1><%= @project.title %></h1>

<ul>
	<% @project.tasks.each do |task| %>
	<li><%= task.title %></li>
	<% end %>
	<li>
		<%= form_for [@project, @project.tasks.build] do |f| %>
		<%= f.text_field :title %>
		<%= f.submit %>
		<% end %>
	</li>
</ul>
```

次はsubmitボタンから実行されるcreateにactionを作成していきます。


##23 Tasksを保存していこう

taskの保存とvalidationをしていきましょう。  
controllerの記述に入ります。  
projects_controller.rbをコピーして作っていきます。

```ruby:tasks_controller.rb
class TasksController < ApplicationController

	def create
		@project = Project.find(params[:project_id])
		@task = @project.tasks.create(task_params)
		redirect_to project_path(@project.id)
	end

	private
		def task_params
			params[:task].permit(:title)
		end

end
```

空欄入力の拒否は必要ですよね。

```ruby: app/models/task.rb
class Task < ActiveRecord::Base
	belongs_to :project
	validates :title, presence: true
end
```

これで完了になります。


##24 Tasksの削除をしてみよう

まずはhtmlのdeleteリンクを他のページからコピーして編集していきましょう。

```ruby:views/projects/show.html.erb
...
	<% @project.tasks.each do |task| %>
	<li>
		<%= task.title %>
		<%= link_to "[delete]", project_task_path(task.project_id, task.id), method: :delete, data: {confirm:"消していいん？"} %>
	</li>
	<% end %>
...
```

あとはroutesにあるようにdestroyメソッドを追加してきましょう。  
こちらもprojects_controllerからコピー&編集していきます。

```ruby:tasks_controller.rb
...
	def destroy
		@task = Task.find(params[:id])
		@task.destroy
		redirect_to project_path(params[:project_id])
	end
...
```

redirectのproject_pathはコピーしたままだとprojects_pathになっており、project一覧ページに遷移してしまうので注意が必要です。


##25 check_box_tagを使おう

task管理なので各タスクにcheckboxを用意しましょう。

```erb:views/projects/show.html.erb
...
		<%= check_box_tag "", "", task.done, {"data-id" => task.id, "data-project-id" => task.project_id}%>
...
<script>
$(function(){
	$("input[type=checkbox]").click(function(){
		$.post("/projects/xxx/tasks/xxx/toggle");
	});
})
</script>
```

jsでajax通信を実装予定です。


##26 toggleアクションを作ろう

jsにdata属性を記述していきます。

```erb:views/projects/show.html.erb
...
<script>
$(function(){
	$("input[type=checkbox]").click(function(){
		$.post("/projects/"+$(this).data("project_id")+"/tasks/"+$(this).data("id")+"/toggle");
	});
})
</script>
```

次はroutesにtoggle処理が定義されてないので。記述しましょう。  
routes.rbにあるサンプルのように記述していきます。  


```ruby:routes.rb
...
  post 'projects/:project_id/tasks/:id/toggle' => 'tasks#toggle'
...
  # Example of regular route:
  #   get 'products/:id' => 'catalog#view'
...
```

次にcontrollerにtoggle処理を追加していきましょう。

```ruby:tasks_controller.rb
...
	def toggle
		@task = Task.find(params[:id])
		@task.done = !@task.done
		@task.save
	end
...
```

routesも通ってるかコマンドで確認してみましょう。

```sh:
% rake routes                                                         [2:37:50]
       Prefix Verb   URI Pattern                                      Controller#Action
project_tasks POST   /projects/:project_id/tasks(.:format)            tasks#create
 project_task DELETE /projects/:project_id/tasks/:id(.:format)        tasks#destroy
     projects GET    /projects(.:format)                              projects#index
              POST   /projects(.:format)                              projects#create
  new_project GET    /projects/new(.:format)                          projects#new
 edit_project GET    /projects/:id/edit(.:format)                     projects#edit
      project GET    /projects/:id(.:format)                          projects#show
              PATCH  /projects/:id(.:format)                          projects#update
              PUT    /projects/:id(.:format)                          projects#update
              DELETE /projects/:id(.:format)                          projects#destroy
              POST   /projects/:project_id/tasks/:id/toggle(.:format) tasks#toggle
         root GET    /                                                projects#index
```

最後の方にちゃんと通ってますね。


##27 Tasksの状況を切り替えよう

ちゃんと動くか確認していきましょう。  
jsコンソールを表示してerrorメッセージが無いか確認していきます。

```
jquery.js?body=1:9632 POST http://0.0.0.0:3000/projects/xxx/tasks/xxx/toggle 404 (Not Found)
```

エラーが出ていますね。
xxxの記述は修正したはずなので旧htmlのキャッシュがきいてるようです。再読み込みしましょう。

```
POST http://0.0.0.0:3000/projects/undefined/tasks/2/toggle 500 (Internal Server Error)
```

undifinedはおかしいので変数名エラーでした。  

```erb:views/projects/show.html.erb
...
		<%= check_box_tag "", "", task.done, {"data-id" => task.id, "data-project_id" => task.project_id}%>
...
```

htmlのdata-project-idを  
data-project_idに変更しました。


```
POST http://0.0.0.0:3000/projects/1/tasks/2/toggle 500 (Internal Server Error)

// error preview
Missing template tasks/toggle, application/toggle with {:locale=>[:en], :formats=>[:html, :text, :js, :css, :ics, :csv, :png, :jpeg, :gif, :bmp, :tiff, :mpeg, :xml, :rss, :atom, :yaml, :multipart_form, :url_encoded_form, :json, :pdf, :zip], :handlers=>[:erb, :builder, :raw, :ruby, :jbuilder, :coffee]}. Searched in: * "/Users/uriu/Documents/dev/ruby/StudyOnRails4/taskapp/app/views"
```

templateが無いよと言われています。  
ですが今回tottleを押しても画面が変わるわけではないのでtemplateを使わない設定にしていきましょう。

```ruby:tasks_controller.rb
...
	def toggle
		render nothing: true
		@task = Task.find(params[:id])
		@task.done = !@task.done
		@task.save
	end
...
```

toggle頭に1行追加しました。  
エラーはもう表示されなくなりました。

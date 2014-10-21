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

```ruby:
class ProjectsController < ApplicationController
	def index
		@projects = Project.all
	end
end
```

viewファイルを同じ位置に同じindexという名前で作成。  
new file:   taskapp/app/views/projects/index.html.erb

```erb:
<h1>Projects</h1>
<ul>
<% @projects.each do |project| %>
	<li><%= project.title %></li>
<% end %>
</ul>
```
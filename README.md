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
```


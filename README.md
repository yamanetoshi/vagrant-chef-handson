Vagrant + Chef Handson 材料
============================

はじめに
-------------------

このドキュメントは DevOps Okinawa の勉強会における初心者向けの資料となります。標題の通り、Vagrant と Chef に関する材料となります。
また、この資料では Vagrant については既に導入や仮想リソースの起動ができるようになっていることを前提としています。Vagrant の導入などについては、[Vagrant 導入など](vagrant.md) を参照して下さい。

レシピを作って実行する流れ
--------------------

伊藤直也さんの入門 Chef-Solo によると以下な形とのことです。

- vagrant up
- Chef リポジトリを作成
- knife solo prepare
- cookbook 作成、レシピ編集
- Chef Solo 実行
- レシピを育てる

### vagrant up

レシピを適用する仮想リソースの kickoff。

    $ cd /some/where

新規に仮想サーバを作るなら init して Vagrantfile を編集しましょう。

    $ vagrant init
    $ vagrant ssh-config --host debian >> ~/.ssh/config
    $ vagrant up

### Chef リポジトリを作成

Chef リポジトリを作成し、Git リポジトリも作成。

    $ knife solo init chef-repo
    $ cd chef-repo
    $ git init
    $ git add .
    $ git commit -m 'first commit '

### knife solo prepare

仮想リソースの名前は debian としています。knife solo prepare して環境設定。ここで作成される JSON ファイルをリポジトリに追加します。

    $ knife solo prepare debian
    $ git add nodes/debian.json
    $ git commit -m 'add node json file'

### cookbook 作成、レシピ編集

cookbook を作成し、レシピの編集をします。

    $ knife cookbook create nginx -o site-cookbooks
    $ vi site-cookbooks/nginx/recipes/default.rb
	$ vi nodes/debian.json

### Chef Solo 実行

レシピができたら適用してみる。

    $ knife solo cook debian

動作の確認に serverspec が使えれば良いですね。問題無ければ commit を作りましょう。

    $ git add site-cookbooks/nginx
	$ git commit -m 'Add nginx recipe'

### レシピを育てる

ここまでの一連の流れを繰返してレシピを育てていくことになります。適用の試行錯誤において sahara を使って rollback したり、ということも必要になってくると思われます。

cookbook とか recipe とか
----------------

これから書く。

- OpsCode Community にある cookbook の取得の方法
- td-agent のレシピを取得して読む
- http://docs.opscode.com/resource.html

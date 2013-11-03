Vagrant 導入など
=======================

導入について
------------

Vagrant の導入については以下を参考にするのが良いと思われます。

- [入門Chef 補完 – chef〜vagrant saharaインストール](http://hirofukami.com/2013/10/11/install-chef-solve-way/)

特に Vagrant については上記ドキュメントにもある通り、http://downloads.vagrantup.com から取得しなければなりません。

sahara の導入
--------------

また、sahara についても上記ドキュメントにある通り

    $ vagrant plugin install sahara

というコマンドで導入となります。

Vagrant による仮想リソースの起動
---------------

まず最初に仮想リソースの OS イメージの取得をしておく必要があります。

    $ vagrant init "Debian7" "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_debian-7.1.0_provisionerless.box"

上記は Debian GNU/Linux の例ですが、[Vagrantbox.es](http://www.vagrantbox.es) で Vagrant 用の OS イメージが公開されていますので、自分の好みのものを取得して使えるようにしておきましょう。

仮想リソースには既に名前が付いていますが IP アドレスなどを付与したい場合には上記コマンドで作成される Vagrantfile という名前のファイルに以下な記述がありますので

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    # config.vm.network :private_network, ip: "192.168.33.10"

一番下の行のコメント記号 ("#") を削除することで private な IP アドレスを付与することが可能です。また Vagrantfile で指定ができる機能については別途確認をしておきましょう。

これで起動の準備ができましたので、以下のコマンドで仮想リソースを起動してみましょう。

    $ vagrant up
    Bringing machine 'default' up with 'virtualbox' provider...
    [default] Setting the name of the VM...
    [default] Clearing any previously set forwarded ports...
    [default] Creating shared folders metadata...
    [default] Clearing any previously set network interfaces...
    [default] Preparing network interfaces based on configuration...
    [default] Forwarding ports...
    [default] -- 22 => 2222 (adapter 1)
    [default] Booting VM...
    [default] Waiting for VM to boot. This can take a few minutes.
    [default] VM booted and ready for use!
    [default] Mounting shared folders...
    [default] -- /vagrant

起動完了したら接続できます。

    $ vagrant ssh
    Linux debian-7 3.2.0-4-amd64 #1 SMP Debian 3.2.46-1+deb7u1 x86_64
    
    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.
    
    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Sat Nov  2 06:26:04 2013 from 10.0.2.2

また、以下のコマンドの出力を ~/.ssh/config に追加しておくことで

    $ vagrant ssh-config --host debian

以下な形での接続も可能です。

    $ ssh debian

knife-solo というツールを使う場合、通常の形で ssh 接続ができるようになっている必要がありますので何らかの形で設定を行なっておく必要があります。	
	
仮想リソースの停止や破棄
---------------

仮想リソースの一時停止は

    $ vagrant halt

破棄は

    $ vagrant destroy

となります。また、一度 vagrant init した後は vagrant up のみで起動が可能です。
	
sahara について
----------------

sahara というプラグインは仮想リソースの状態を保存しておき、その時点までロールバックする、という事ができます。使用方法を以下に列挙しておきます。

- sahara をインストール

    $ vagrant gem install sahara

- sandbox モードを有効にする

    $ vagrant sandbox on

- ロールバックする

    $ vagrant sandbox rollback

- 仮想リソースの状態変更を確定させる

    $ vagrant sandbox commit

- sandbox モードのを解除

    $ vagrant sandbox off

# itamaeLesson


## vagrantでitamae用仮想サーバ作成

##### vagrantfile作成と更新
```
$ mkidr itamaetest
$ vagrant init
```

##### 名前とbox(OS)を指定
```
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "itamaetest"
  config.vm.box_url = "https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.0.0/centos-6.6-x86_64.box"
```

##### プライベートIP指定
```
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "private_network", ip: "192.168.33.11"
```

##### vagrant起動
```
$ vagrant up
```

##### SSH接続用にコンフィグ追加
```
$ vagrant ssh-config --host itamaetest >> ~/.ssh/config
```

(追加されるコンフィグ)  

```
Host itamaetest
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/fumiaki/GoogleDrive/Develop/Vagrant/itamaetest/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

```
$ ssh itamaetest
[vagrant@localhost ~]$
```

## itamaeインストール
```
$ sudo gem install itamae
```

## receip.rbの作成
```
package 'php' do
  action :install
end

package 'httpd' do
  action :install
end

service 'httpd' do
  action [:enable, :start]
end

```

## itamae実行
```
$ itamae ssh -u vagrant -h 192.168.33.11 recipe.rb
```

実行後、仮想サーバ側でhttpdとphpがインストールされている

```
vagrant@localhost ~]$ httpd -v
Server version: Apache/2.2.15 (Unix)
Server built:   Aug 24 2015 17:52:49
[vagrant@localhost ~]$ php -v
PHP 5.3.3 (cli) (built: Jul  9 2015 17:39:00)
Copyright (c) 1997-2010 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies
```

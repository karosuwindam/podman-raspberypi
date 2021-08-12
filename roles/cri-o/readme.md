# cri-oについて

このタスクを実行するとRaspberryPi用のCRI-Oをインストールする

[CRI-Oのリポジトリについて](http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/)

[CRI-O RUNCのリポジトリについて](https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/)

# 実行コマンドについて

sources.listの更新を行う
```
sudo sh -c "echo 'deb http://deb.debian.org/debian buster-backports main' >> /etc/apt/sources.list"
```

buster-backportsのlibseccomp2をインストールする
```
sudo apt-get update
sudo apt-get -y -t buster-backports install libseccomp2
```

以下のコマンドで、インストールに使用するレポジトリを指定する
```
os=Debian_10
version="1.21"
```

以下のコマンドで、レポジトリを登録する
```
echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$os/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$version/$os/ /" | sudo tee  /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$version.list
```

apt-keyの追加
```
curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$version/$os/Release.key |sudo  apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$os/Release.key | sudo apt-key add -
```

以下のコマンドで、cri-oとcri-orunc、cri-toolsをインストールする
```
sudo apt-get update
sudo apt-get install cri-o cri-o-runc cri-tools -y
```

以下のコマンドで、電源投入時にモジュールbr_netfilterとoverlayが起動するように設定する
```
sudo sh -c "cat <<EOF > /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
"
```


以下のコマンドで、crioの自動起動を有効にする
```
sudo systemctl enable crio
```

以上のコマンドを実行後、再起動することで、CRI-Oが使用できるようになる。


また、cri-toolsがインストールしているので、以下のコマンドで動作を確認できる
```
$ sudo crictl ps
CONTAINER           IMAGE                                                              CREATED             STATE               NAME                      ATTEMPT             POD ID
```
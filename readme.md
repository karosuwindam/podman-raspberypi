# AnsibleでPodman実行環境をRaspberryPi 4上に展開する。


以下のコマンドを実行してansibleのインストールと、ssh-keyを使用して、接続できるようにしておく
```
sudo apt install -y ansible
ssh-keygen
ssh-copy-id localhost
```


以下のコマンドを実行してインストールする
```
./start.sh
```

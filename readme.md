# AnsibleでPodmanやCRI-O、KuberentesをRaspberryPi 4上で展開する

以下のコマンドを実行してansibleのインストールと、ssh-keyを使用して、接続できるようにしておく
```
sudo apt install -y ansible
ssh-keygen
ssh-copy-id localhost
```

# AnsibleでPodman実行環境をRaspberryPi 4上に展開する。

以下のコマンドを実行してインストールする
```
./start.sh
```

# AnsibleでCRI-Oをセットアップする

以下のコマンドを実行して、インストールする
```
./setup_crio.sh
```

完了後再起動することで、CRI-Oが動作する

# AnsibleでKubernetesをセットアップする

以下のコマンドを実行して、インストールする
```
./setup_k8s.sh
```

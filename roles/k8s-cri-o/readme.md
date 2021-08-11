# k8s-cri-oについて

このタスクを使用すると、CRI-O用のkuberentes 1.21をインストールする

# 実行コマンドについて

以下のコマンドで、IPフォワードの登録を行う
```
sudo sh -c "cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF
"
sudo sysctl --system
```

Kuberentesのapt-keyを登録する
```
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg|sudo apt-key add -
```

リポジトリの登録
```
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kube.list
```

以下のコマンドで、kuberentesの3コマンド、kubeadmとkubelet、kubectlをインストールする。なお、バージョンは1.21を指定する
```
sudo apt-get update
sudo apt-get install -y kubelet=1.21.3-00 kubeadm=1.21.3-00 kubectl=1.21.3-00
```

cri-o用のkubeletの設定を行う
```
sudo wget -O /etc/default/kubelet https://gist.githubusercontent.com/haircommander/2c07cc23887fa7c7f083dc61c7ef5791/raw/73e3d27dcd57e7de237c08758f76e0a368547648/cri-o-kubeadm
```

cmdline.txtをkkubernetesが起動できるように書き換える
```
sudo sh -c "echo 'console=serial0,115200 console=tty1 root=PARTUUID=48bdd2cf-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait quiet splash plymouth.ignore-serial-consoles cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1' > /boot/cmdline.txt"
```

SWAPをOFFにする
```
sudo dphys-swapfile swapoff
sudo systemctl stop dphys-swapfile
sudo systemctl disable dphys-swapfile
```

再起動後、以下のコマンドで、初期化できることを確認する

単数のマスター制御
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
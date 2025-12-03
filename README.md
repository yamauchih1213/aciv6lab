# 使い方
* まず、ラボのWin11クライアントのPowerShellで、最初から存在するUbuntuのWSLに入る。
```Powershell
wsl -d Ubuntu-24.04
```
* Ubuntu上でこのリポジトリの内容をcloneし、AlmaLinux(Ansibleセットアップ済)のイメージを展開する。
```Bash
cd
git clone https://github.com/yamauchih1213/aciv6lab.git
bash 00setup.sh
exit
```
* 再び、ラボのWin11クライアントのPowerShellで、展開したイメージをインポートし、起動する。
```Powershell
wsl --import AlmaLinux-9 .\AlmaLinux-9 C:\dcloud\AlmaLinux-9.tar
wsl -d AlmaLinux-9
```
* AlmaLinuxのWSLでユーザをスイッチする。
```Bash
su - admin
```
* 以降、任意のplaybookを置いて実行して、dCloudのAPICをAnsibleで自由に使ってください。
* アクセスするAPICは「apic1.corp.pseudoco.com」です。


# 参考1: WSLイメージの作り方
このリポジトリに置いているAlmaLinuxのWSLイメージは以下の方法で作成しています。

## ラボのWin11クライアント上のPowerShellでの操作
```Powershell
wsl --install AlmaLinux-9
```
* この流れで自動で起動する。
* ローカルユーザのID/PASSの入力を求められるので適当に設定する。( admin/C1sco12345など )

## WSL内のBashでの操作
```Bash
sudo ls
sudo dnf update -y
sudo dnf install python3-pip -y
sudo pip3 install --upgrade pip
sudo pip3 install ansible==2.9.23
ansible-galaxy collection install cisco.aci
pip3 install lxml
pip3 install xmljson
sudo dnf install vim-minimal -y
sudo dnf install git -y
```
* 必要に応じSSH鍵を生成してくだださい。(githubに置くなど)
```Bash
ssh-keygen -t rsa -b 4096
```
* なお、完成したイメージをエクスポート/圧縮/50M単位に分割したものが、このリポジトリのファイルです。
```Powershell
wsl -t AlmaLinux-9
wsl --export AlmaLinux-9 C:\dcloud\AlmaLinux-9.tar
```
```Bash
gzip -c /mnt/c/dcloud/AlmaLinux-9.tar | split -b 50M - AlmaLinux-9.tar.gz.part_
```

# 参考2: 各種コマンド(WSL関連)
* 利用可能なWSLディストリビューションを一覧表示する。
```Powershell
wsl -l
```
* 起動する。
```Powershell
wsl -d AlmaLinux-9
```
* 停止する。
```Powershell
wsl -t AlmaLinux-9
```
* 削除する。
```Powershell
wsl --unregister AlmaLinux-9
```

# 参考3: 各種コマンド(Linux関連)
* ファイルを圧縮してサイズで分割する。(例として50Mバイト)
```Bash
gzip -c AlmaLinux-9.tar | split -b 50M - AlmaLinux-9.tar.gz.part_
```
* 分割したファイルを結合して解凍する。(例として上記コマンドで分割した場合)
```Bash
cat AlmaLinux-9.tar.gz.part_a* | gunzip > AlmaLinux-9.tar
```

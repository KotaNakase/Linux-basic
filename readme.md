# Linux学習
## セクション2
### Linuxとは
* **Linuxカーネル**  
OSの基本機能をもつソフトウェア
* **inuxディストリビューション**  
Linuxの配布形態のこと  
・RedHat系(CentOS)  
・Debian系(Raspy)  
・Slackware系
> Linuxカーネル+その多機能・ソフトウェアを付与→Linuxディストリビューション

* **CentOS**  
RadHat Enterprise Linux(RHEL)との互換性を目指した**Linuxディストリビューション**(RedHat系に分類される)

* CUI  
コマンド文字でコンピュータを操作すること

* GUI  
マウスやグラフィックを使用して操作すること


---
## セクション3

---
## セクション4

---
## セクション5
### LAMP環境構築
webサーバ構築に使用される組み合わせとして一般的なセット  
* L...Linux(OS)  
* A...Apache(webサーバ)  
* M...MySQL(データベースサーバ)  
* P...PHP(プログラミング言語)

* **yum(Yellowdog Updater Modified)**  
パッケージ管理システム  
上記のソフトウェアもyumを使用してインストールする

#### リポジトリ
ソフトウェアが格納されている集まりのようなもの  
※外部から下記の二つのレポジトリをインストールし、さらにそれらのレポジトリ内から必要なソフトウェア(PHP等)をインストールする  
* EPELリポジトリ(Extra Packages for Enterprise Linux)  
RHEL向けの追加パッケージ
* Remiレポジトリ  
MySQLやPHPの新しいバージョンを集めたリポジトリ

1. ソフトウェアのアップデート  
sudo yum update で実行  
※updateがうまくいかない場合、以下の手順でキャッシュの削除を行う。  
* du -sh /var/cache/yum (キャッシュの確認)
* sudo rm -rf /var/cache/yum/* (キャッシュの削除)

2. EPELリポジトリのインストール  
sudo yum install epel-release で実行

3. remiレポジトリのインストール  
sudo yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm で実行

4. インストール後の確認  
yum repolist  
上記のレポジトリがインストールされていることを確認

#### **Apacheのインストール**  
sudo yum install httpd  
バージョン確認は httpd -v

* OSの再起動時にApacheも起動するように設定  
yum systemctl enable httpd

* 設定内容の確認(再起動時にApacheが起動しているか)
1. sudo reboot
2. systemctl status httpd  
runningと表示されていれば成功  
**※httpdのdについて**  
デーモンといい、常駐プログラムの呼び名のこと。  
httpリクエストに応えてくれる。常に準備状態とするためにOS起動時に起動している必要がある。  
Linuxでインストールする時は「**httpd**」でインストールする。

* Apacheの設定  
ポート80,443の解放  
sudo firewall-cmd --permanent --zone=public --add-service=http  
sudo firewall-cmd --permanent --zone=public --add-service=https

SELinux...サーバへの不正アクセスや情報漏洩を防ぐセキュリティ対策の仕組み  
意図的に使用しないケースが多い(らしい)  
SELinux自体の難解さによるため。また、事後防衛的な手段であるため。  
画像SELinux貼る

* SELinuxの有効/無効確認  
getenforce  
**Enforcing**→有効  
**Disabled**→無効

* SELinuxの有効化/無効化  
sudo vi /etc/selinux/config  
実行後OSを再起動(sudo reoot)  

#### **Apache動作確認**
* 仮想マシンのIPアドレス確認  
ip addr 又は ip a  
画像ipaddr貼る

* 検索するとApacheのテストページが開く  
ホストのPCから閲覧可能

* /var/www/html/内にhtmlファイルを作成する  
上記のアドレスは less /etc/httpd/conf/httpd.conf から確認(/DocumentRootに記載されている)

* ファイルの所有権をrootからApacheに移行  
ファイル作成にあたってrootで作成することは基本ない  


#### **PHPインストール**
sudo yum --enablerepo=remi-php71 install php php-devel php-mysql php-gd php-mbstring  

「php」PHP本体  
「php-devel」PHP開発に必要  
「php-mysql」MySQL使用に必要  
「php-gd」画像変換に必要  
「php-mbstring」日本語(マルチバイト文字)の使用に必要

* システムの再起動  
sudo systemctl restart httpd


#### **PHP動作確認**
sudo vi /var/www/html/index.php  
\<?php  
phpinfo();

phpの設定情報が確認できる

PHPプログラムの実行  
\<?php  
echo 'Hello World';

#### **MySQLインストール**
sudo yum remove mariadb-libs  
デフォルトで導入されているmariaDBを削除することでMySQLとの競合を防ぐ

関連ファイルも削除する  
sudo rm -rf /var/lib/mysql

MySQLのリポジトリの設定  
$sudo yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

パッケージ情報閲覧  
yum info mysql-community-server

**※Shift+PgUpで上下移動可**

MySQLインストール  
sudo yum install mysql-community-server

* 下記エラー発生時  
**Public key for mysql-community-client-8.0.28-1.el7.x86_64.rpm is not insalled**  
https://blog.proglus.jp/5095/#i リンクを参照

* インストール後の確認  
mysqld --version(**mysql -vで確認できると思った**)

* MySQLへの初期ログインパスワード  
sudo cat /var/log/mysqld.log | grep 'temporary password'  
(logファイルから'temporary password'を探す)

ログイン後新規パスワードを設定する

MySQLにログイン

mysql -u root -p  

データベース確認(MySQLの書き方)  
show databases

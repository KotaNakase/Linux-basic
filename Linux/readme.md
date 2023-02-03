# Linux基礎・LAMP環境構築
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
#### VirtualBOXのインストール
* 既存OS上に仮想のPC環境を作成してOSをインストール・動作させられるソフトウェア⇔AWSのEC2のようなもの

* スナップショット(VirtualBox)
OSの状態を保存することができる(セーブポイントのようなもの)

* CentOS内画面操作中(キャプチャ中)に画面外を操作したい場合
右Ctrlを押すことで可能

* 作成した仮想マシンにCentOSをインストールしていく

---
## セクション4
#### Linuxコマンド
画面のコマンド入力・結果をクリア
```
clear
```
ディレクトリ移動
```
cd
```
権限・タイムスタンプ等の詳細情報を確認できる
```
ls -l
```
-aオプションで隠しファイルが確認できる
```
ls -a
```
上記二つのオプションをまとめて実行が可能
```
ls -la
```
ディレクトリ作成
```
mkdir
```
絶対パスでディレクトリを作成
```
mkdir -p /root/lesson4/lesson5
```
相対パスでも可能
```
mkdir -p lesson2/lesson3
```
ディレクトリ削除  
※ディレクトリ内にファイルが存在しない場合のみ成功するコマンド
```
rmdir
```
ディレクトリ削除を再帰的(-r)に行うことができる
```
rm -r ディレクトリ名
```
ディレクトリ削除を確認なしで行える
```
rm -rf ディレクトリ名
```
mydir内のファイルも併せてコピー先(mydir2)に複製する
```
cp -r mydir mydir2
```
ファイル/ディレクトリ移動・ファイル名変更
```
mv
```
ファイル編集
```
vi ファイル名
```
新規ファイル作成も同時に行える
* iをおして編集モード
* escをおしてコマンド入力モード
* :w入力で保存
* :q入力でviコマンド終了
* :wqで同時にコマンド入力が可能
* :q!で変更内容を保存しないでviコマンド終了

カーソル移動
* w 単語の先頭に移動(★→☆)  
例)
```
☆this is★ an apple
```

* 行数+Gで指定の行に移動

次のページ
```
Ctrl+f
```
前のページ
```
Ctrl+B
```
一行カット
```
dd
```
一行コピー
```
yy
```
ペースト
```
p
```
行数表示
```
:set nu
```
行数非表示
```
:set nonu
```
ユーザー追加
```
useradd 名前
```
ユーザー確認
```
cat /etc/passwd
```
パスワード設定
```
passwd ユーザー名
```
ユーザー切り替え  
ハイフンによってそのユーザーのホームディレクトリに移動する
```
su - ユーザー名
```
-rでホームディレクトリごと削除できる
```
userdel -r ユーザー名
```
root権限(スーパーユーザー)で実行
```
sudo
```
wheelグループに追加
```
usermod -aG wheel ユーザー名
```
権限付与
```
visudo
```
作成したユーザー→rootユーザー
終了する時はexit
```
sudo su -
```
システムの再起動
```
sudo reboot
```
今すぐにシャッドダウンする  
-rでリブート、-hでシャッドダウン
```
sudo shutdown -r now
sudo shutdown -h now
```
13時にシャットダウン
```
sudo shutdown -h 13:00
```
ログインしている他のユーザーにメッセージのみ表示する
```
shutdown -k "コメント"
```
ログインしている他のユーザーにメッセージを表示し、10分後にシャットダウンする
```
shutdown -h +10 "コメント"
```

履歴確認  
指定数だけ履歴確認
```
history 数字
```
指定番号のコマンド実行
```
!数字
```
履歴削除  
指定した番数の履歴を削除
```
history -d 数字
```
コマンドの場所を探す※パスが通っていないときに使用する
```
which コマンド
```
コマンドのヘルプを開く
```
man コマンド
```
アクセス権変更
```
chmod 数値 ファイル名
```
ファイル・ディレクトリの所有者を変更
```
sudo chown ユーザー名:ユーザーグループ ファイル名
```
-Rを付与することでディレクトリ内ファイルも所有者が変更される
```
sudo chown -R 
```
Linuxにおけるグループ
権限管理のためにグループに所属させてそのグループにまとめて権限を付与する
複数グループに所属可

何のグループに所属しているか確認
```
groups ユーザー名
id ユーザー名
```
上記二つのどちらでも確認可能

グループ削除
```
groupdel グループ名
```

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
![](.\selinux.png)

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
![](.\ipaddr.png)

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
mysqld --version

* MySQLへの初期ログインパスワード  
sudo cat /var/log/mysqld.log | grep 'temporary password'  
(logファイルから'temporary password'を探す)

ログイン後新規パスワードを設定する

MySQLにログイン

**mysql -u root -p**

* データベース確認(MySQLの書き方)  
show databases

* MySQLの文字コードの設定(UTF-8)  
sudo vi /etc/my.cnf  

character-set-server = utf8

入力後MySQLを再起動して設定を反映  
sudo systemctl restart mysqld

OS起動時にMySQLが起動するよう設定  
sudo systemctl enable mysqld

上記設定の確認  
systemctl list-unit-files -t service | grep mysqld
mysqld.serviceが**enabled**になっていれば成功

---
## セクション6
#### **WordPressとは**
オープンソースのブログソフトウェア。コンテンツマネジメントシステム(CMS:Content Management System)のひとつ。(CMSについては別途記載予定)  
Webサイトを構成する機能(コンテンツ)を一元管理・保存するシステムのこと。(レイアウト情報・画像添付・書式・…等のコンテンツを指す)  
今回は、数あるCMSのうちのWordPressを使用する

* PHPによって開発されている
* データベースにMySQLを使用

**↑そのため、これまでLAMP環境を構築してきた**

#### **MySQL側の設定**
* MySQLにユーザーを追加  
create user 'mybloguser'@'localhost' identified with mysql_native_password by 'パスワード';  
※localhostからログインできる、mybloguserという名前のユーザーを追加

* MySQLにデータベースを追加  
create database wp_myblog;

* 上記で作成したユーザーがwp_myblogを使用できるように設定  
grant all privileges on wp_myblog.* to 'mybloguser'@'localhost';

* 設定の反映  
flush privileges;

#### WordPressのインストール
* インストールするソフトウェアの確認  
yum list | grep wget

* インストール  
sudo yum install wget  
which wgetでインストール先を確認

* wordpressのURLを取得  
https://ja.wordpress.org/latest-ja.tar.gz

* wordpressインストール用のファイルをダウンロード  
cd /tmp  
wget 上記URL

* ダウンロードファイルの解凍  
sudo tar -zxvf latest-ja.tar.gz -C /var/www/

* 解凍ファイルは/var/www内で確認  
![](.\Wordpress.png)

wordpress内に必要なファイルが入っている

wordpressフォルダの所有者をApacheに変更  
sudo chown -R apache:apache wordpress/

* 設定ファイル編集  
バックアップ作成を行ってから設定ファイルを編集する  
sudo cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.org  
sudo vi /etc/httpd/conf/httpd.conf

* ドキュメントルートをwordpressに変更(見に行く先を変更)  
/var/www/html　→　/var/www/wordpress  
\<Directory "/var/www">　→　\<Directory "/var/www/wordpress">

* 設定反映
sudo systemctl restart httpd

ブラウザ上で初期設定を進める

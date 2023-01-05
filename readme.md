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

* **リポジトリ**  
ソフトウェアが格納されている集まりのようなもの  
※外部から下記の二つのレポジトリをインストールし、さらにそれらのレポジトリ内から必要なソフトウェア(PHP等)をインストールする  
* EPELリポジトリ(Extra Packages for Enterprise Linux)  
RHEL向けの追加パッケージ
* Remiレポジトリ  
MySQLやPHPの新しいバージョンを集めたリポジトリ

1. ソフトウェアのアップデート  
sudo yum update で実行

2. EPELリポジトリのインストール  
sudo yum install epel-release で実行

3. remiレポジトリのインストール  
sudo yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm で実行

4. インストール後の確認  
yum repolist

* **Apacheのインストール**  
sudo yum install httpd  
バージョン確認は httpd -v

* OSの再起動時にApacheも起動するように設定




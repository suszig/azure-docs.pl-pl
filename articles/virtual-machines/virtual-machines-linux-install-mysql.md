<properties
    pageTitle="MySQL を Linux VM にセットアップする | Microsoft Azure "
    description="Azure 上の Linux 仮想マシン (Ubuntu または RedHat ファミリ OS) に MySQL スタックをインストールする方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="mingzhan"/>


#MySQL を Azure でインストールする方法

この記事では、インストールして、Linux を実行する Azure の仮想マシンに MySQL を構成する方法を学習します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


仮想マシン上で MySQL を ##Install

> [AZURE.NOTE] このチュートリアルを完了するために Linux を実行する Microsoft Azure 仮想マシンが既に必要があります。 参照してください、
[Azure Linux VM チュートリアル](virtual-machines-linux-tutorial.md) で Linux VM を作成および設定する `mysqlnode` VM 名として、 `azureuser` 続行する前にユーザーとして。

この例では、MySQL ポートとしてポート 3306 を使用します。  

putty を使用して作成した Linux VM に接続します。 初めて Azure Linux VM を使用する場合は、putty を使用する方法を参照してください。 Linux VM に接続 [ここ](virtual-machines-linux-use-ssh-key.md)します。

この記事では、例として MySQL5.6 のインストールにパッケージのリポジトリを使用します。 実際、MySQL5.6 は、MySQL5.5 よりもパフォーマンスが向上しています。  詳細については [ここ](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/)します。


###How Ubuntu 上で mysql 5.6 をインストールするには
ここでは、Azure の Ubuntu Linux VM を使用します。

-ステップ 1: MySQL Server 5.6 をインストールします。
    切り替える `root` ユーザー。

           #[azureuser@mysqlnode:~]sudo su-

   mysql-server 5.6 をインストールする

           #[root@mysqlnode ~] # get apt 更新
           #[root@mysqlnode ~] # get apt y mysql server-5.6 をインストールします。

   インストール中に、次の MySQL ルート パスワードを設定するように依頼までダイアログ ウィンドウ ・ poping が表示され、ここで、パスワードを設定する必要があります

    ![。イメージ](./media/virtual-machines-linux-install-mysql/virtual-machines-linux-install-mysql-p1.png)


    パスワードの確認を入力します

    ![。イメージ](./media/virtual-machines-linux-install-mysql/virtual-machines-linux-install-mysql-p2.png)

-手順 2: ログインの MySQL サーバー

   MySQL サーバー のインストールが完了したら、MySQL サービスが自動的に開始されます。 MySQL サーバーには、ユーザー `root` としてログインできます。
    以下のコマンドを使用してログインし、パスワードを入力します。

             #[root@mysqlnode ~] # mysql uroot ~ p

-ステップ 3: 実行中の MySQL サービスを管理します。

   (a) MySQL サービスの状態を取得する

            #[root@mysqlnode ~] # サービスの mysql の状態

   (b) MySQL サービスを開始する

            #[root@mysqlnode ~] # サービスの mysql の開始

   (c) MySQL サービスを停止する

            #[root@mysqlnode ~] # サービスの mysql の停止

   (d) MySQL サービスを再起動する

            #[root@mysqlnode ~] サービス mysql を再起動する #


###How CentOS、Oracle Linux のように Red Hat の OS のファミリで MySQL をインストールするには
ここでは CentOS または Oracle Linux と Linux VM を使用します。

-手順 1. MySQL Yum リポジトリを追加します。
   切り替える `root` ユーザー。

           #[azureuser@mysqlnode:~]sudo su-

   MySQL リリース パッケージをダウンロードして、インストールします。

           #[root@mysqlnode ~] # wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
           #[root@mysqlnode ~] # yum localinstall-y mysql-コミュニティ-リリース-el6-5.noarch.rpm

-Mysql 5.6 パッケージをダウンロードするため、MySQL リポジトリにファイルを下ステップ 2. で編集します。

            #[root@mysqlnode ~] # vim/etc/yum.repos.d/mysql-community.repo

   このファイルの各値を以下のように更新します。

       \ # *MySQL 5.6 を使用するには、有効にする*

        [mysql56 コミュニティ]
       名前 = MySQL 5.6 Community Server

       baseurl http://repo.mysql.com/yum/mysql-5.6-community/el/6/$ basearch =/

       有効になっている = 1

       gpgcheck = 1

       gpgkey = ファイル://など/pki/rpm-gpg/RPM-GPG-キー-mysql

-ステップ 3: インストール MySQL MySQL リポジトリから
   MySQL をインストールします。

          #[root@mysqlnode ~] #yum mysql コミュニティ サーバーのインストール

   MySQL RPM パッケージとすべての関連するパッケージがインストールされます。

-ステップ 4: 実行中の MySQL サービスを管理します。

    (a) MySQL server のサービスの状態を確認します。

          #[root@mysqlnode ~] #service mysqld ステータス

   (b)、既定のポートの MySQL サーバーが実行されているかどうかを確認します。

          #[root@mysqlnode ~] #netstat – tunlp|grep 3306
    (c) MySQL サーバーを起動します。

          #[root@mysqlnode ~] #service mysqld の開始

   (d) MySQL サーバーを停止する

          #[root@mysqlnode ~] #service mysqld の停止

   (e) 次回システム起動時に MySQL を開始するよう設定する 

          #[root@mysqlnode ~] [#chkconfig mysqld


SUSE Linux に MySQL をインストールする ###How
ここでは、OpenSUSE と Linux 仮想マシンを使用します。

-手順 1: ダウンロードし、MySQL Server のインストールr
    切り替える `root` ユーザーを次のコマンドを実行します。 

          #sudo su-

   MySQL Server パッケージをダウンロードしてインストールします。

          #[root@mysqlnode ~] # zypper 更新

          #[root@mysqlnode ~] # zypper の mysql サーバー mysql 説明 mysql のインストール

-手順 2: 実行中の MySQL サービスを管理します。e
    (a) MySQL server の状態を確認します。

          #[root@mysqlnode ~] # rcmysql ステータス

   (b) MySQL Server の既定のポートを確認する

          #[root@mysqlnode ~] # netstat – tunlp|grep 3306


   (c) MySQL サーバーを開始する

          #[root@mysqlnode ~] # rcmysql の開始

   (d) MySQL サーバーを停止する

          #[root@mysqlnode ~] # rcmysql の停止

   (e) 次回システム起動時に MySQL を開始するよう設定する 

          #[root@mysqlnode ~] # insserv mysql

### Next のステップ
詳細の使用状況および MySQL に関する情報を見つける [ここ](https://www.mysql.com/)します。.

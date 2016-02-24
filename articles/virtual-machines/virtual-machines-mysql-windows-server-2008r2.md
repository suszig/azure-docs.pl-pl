<properties
    pageTitle="MySQL を実行する VM の作成 | Microsoft Azure"
    description="Windows Server 2012 R2 を実行するクラシック デプロイモデルで作成された Azure 仮想マシンを作成し、そこで MySQL データベースのインストールと構成を行います。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/09/2015"
    ms.author="cynthn"/>


# Windows Server 2012 R2 を実行するクラシック デプロイ モデルで作成された仮想マシンに MySQL をインストールする

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


[MySQL](http://www.mysql.com) は一般的なオープン ソースの SQL データベース。 使用して、 [Azure クラシック ポータル](http://manage.windowsazure.com), 、イメージ ギャラリーから Windows Server 2012 R2 を実行する仮想マシンを作成することができます。 次に、MySQL Server としてこれをインストールと構成できます。

Linux の MySQL をインストールする方法の詳細についてを参照してください: [Azure で MySQL をインストールする方法](virtual-machines-linux-install-mysql.md)します。

このチュートリアルでは、次の操作方法について説明します。

- Azure クラシック ポータルを使用して Windows Server 2012 R2 を実行する仮想マシンを作成します。

- MySQL 5.6.23 の Community バージョンを MySQL Server としてこの仮想マシン上にインストールと実行します。


## Windows Server を実行する仮想マシンの作成

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## データ ディスクの接続

仮想マシンを作成したら、追加のデータ ディスクを必要に応じて接続できます。 運用環境のワークロードとオペレーティング システムを含む OS ドライブ (c:) の容量の不足を回避する推奨されています。

参照してください [Windows 仮想マシンにデータ ディスクをアタッチする方法](storage-windows-attach-disk.md) の空のディスクを接続する手順に従います。 ホスト キャッシュ設定を設定する **None** または **読み取り専用**します。

## 仮想マシンへのログオン

次に、MySQL をインストールするために、仮想マシンにログオンします。

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

##仮想マシンに MySQL Community Server をインストールして実行する

MySQL Server の Community バージョンをインストール、構成、と実行するには、次の手順を実行します。

> [AZURE.NOTE] 次の手順は、5.6.23.0 の Community バージョン MySQL と Windows Server 2012 R2。 異なるバージョンの MySQL または Windows Server を使用しているユーザーは、同一の結果にならない場合があります。

1.  リモート デスクトップを使用して仮想マシンに接続したら、以下の] をクリックして **Internet Explorer** スタート画面からです。
2.  選択、 **ツール** (歯車のアイコン) には、右上隅にあるボタンをクリックし、をクリックし、 **インターネット オプション**します。 をクリックして、 **セキュリティ** ] タブをクリックして、 **信頼済みサイト** アイコンをクリックして、 **サイト** ] ボタンをクリックします。 信頼済みサイトの一覧に http://*.mysql.com を追加します。 をクリックして **閉じる**, 、順にクリック **OK**します。
3.  Internet Explorer のアドレス バーに「http://dev.mysql.com/downloads/mysql/」と入力します。
4.  MySQL サイトで MySQL Installer for Windows の最新バージョンを見つけ、インストールします。 MySQL Installer を選ぶ際、すべてのファイル セットが揃っているバージョンをダウンロードし (ファイル サイズが 282.4 MB の mysql-installer-community-5.6.23.0.msi など)、このインストーラーを保存します。
5.  インストーラーのダウンロードが完了したら、クリックして **実行** セットアップを起動します。
6.   **使用許諾契約書** ] ページでライセンス条項に同意してクリックして **次**します。
7.   **Choosing a Setup Type** ] ページでクリックし、をクリックし、セットアップの種類 **次**します。 次の手順では、選択範囲の **サーバーのみ** セットアップの種類。
8.   **インストール** ] ページで [ **Execute**します。 インストールが完了したら、クリックして **次**します。
9.   **成果物の構成** ] ページで [ **次**します。
10.  **Type and Networking** ] ページで、必要な場合は、TCP ポートを含む、必要な構成の種類と接続オプションを指定します。 選択 **Show Advanced Options**, 、] をクリックし、 **次**します。

    ![](./media/virtual-machines-mysql-windows-server-2008r2/MySQL_TypeNetworking.png)

11.  **Accounts and Roles** ] ページで、強力な MySQL ルート パスワードを指定します。 必要に応じて、別の MySQL ユーザー アカウントを追加し、クリックして **次**します。

    ![](./media/virtual-machines-mysql-windows-server-2008r2/MySQL_AccountsRoles_Filled.png)

12.  **Windows サービス** ] ページで、必要に応じて、Windows サービスとして MySQL Server を実行するため、既定の設定に対する変更を指定してクリックして **次**します。

    ![](./media/virtual-machines-mysql-windows-server-2008r2/MySQL_WindowsService.png)

13.  **詳細オプション** ] ページで、必要に応じてログ オプションに対する変更を指定して、をクリックし、 **次**します。

    ![](./media/virtual-machines-mysql-windows-server-2008r2/MySQL_AdvOptions.png)

14.  **Apply Server Configuration** ] ページで [ **Execute**します。 構成手順が完了すると、クリックして **完了**します。
15.  **成果物の構成** ] ページで [ **次**します。
16.  **インストールの完了** ] ページで [ **Copy Log to Clipboard** 後で、内容を検討し、たい場合 **完了**します。
17. スタート画面から次のように入力します。 **mysql**, 、クリックして **MySQL 5.6 Command Line Client**します。
18. 手順 11. で指定したルート パスワードを入力すると、MySQL を構成するためのコマンドを実行できるプロンプトが表示されます。 コマンドと構文の詳細については、次を参照してください。 [MySQL のリファレンス マニュアル](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html)します。

    ![](./media/virtual-machines-mysql-windows-server-2008r2/MySQL_CommandPrompt.png)

19. また、C:\Program Files (x86)\MySQL\MySQL Server 5.6\my-default.ini ファイルに入力することにより、ベース ディレクトリやデータ ディレクトリ、ドライブなど、サーバー構成に関する既定の設定についても構成できます。 詳細については、次を参照してください。 [5.1.2 Server Configuration Defaults](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html)します。

## エンドポイントを構成する

インターネット上の MySQL クライアント コンピューターで MySQL Server サービスを利用可能にするには、MySQL Server サービスがリッスンしている TCP ポートにエンドポイントを構成し、別の Windows ファイアウォール ルールを作成する必要があります。 別のポートを指定しない限り、これは、TCP ポート 3306、 **Type and Networking** ページ (前の手順の手順 10)。


> [AZURE.NOTE] インターネット上のすべてのコンピューターは、MySQL サーバー サービスを使用できるようにこのため、これについては、セキュリティへの影響を慎重に検討する必要があります。 Access Control リスト (ACL) でエンドポイントを使用することができる発信元 IP アドレスのセットを定義できます。 詳細については、次を参照してください。 [仮想マシンに対してエンドポイントを設定する方法](virtual-machines-set-up-endpoints.md)します。


MySQL Server サービスのエンドポイントを構成するには、次のようにします。

1.  Azure クラシック ポータルで、クリックして **仮想マシン**, 、MySQL 仮想マシンの名前をクリックし、 **エンドポイント**します。
2.  コマンド バーで、クリックして **追加**します。
3.   **仮想マシンにエンドポイントを追加** ] ページで、右矢印をクリックします。
4.  既定の MySQL TCP ポート 3306 を使用している場合はクリックして **MySQL** で **名**, 、チェック マークをクリックします。
5.  別の TCP ポートを使用している場合は、一意の名前を入力します。 **名前**します。 選択 **TCP** プロトコルでは、両方にポート番号を入力 **パブリック ポート** と **プライベート ポート**, 、チェック マークをクリックします。

## Windows Firewall ルールを追加して MySQL トラフィックを許可する

インターネットからの MySQL トラフィックを許可する Windows ファイアウォール ルールを追加するには、MySQL サーバー仮想マシンで管理者特権の Windows PowerShell コマンド プロンプトで次のコマンドを実行します。

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## リモート接続をテストする


Azure 仮想マシンで実行する MySQL Server サービスへのリモート接続をテストするには、最初に、MySQL Server を実行する仮想マシンが属しているクラウド サービスに対応する DNS 名を特定する必要があります。

1.  Azure クラシック ポータルで、クリックして **仮想マシン**, 、MySQL server 仮想マシンの名前をクリックして、クリックして、 **ダッシュ ボード**します。
2.  仮想マシンのダッシュ ボードに注意してください、 **DNS 名** ] の [値、 **概要** セクションです。 たとえば次のようになります。

    ![](./media/virtual-machines-mysql-windows-server-2008r2/MySQL_DNSName.png)

3.  MySQL または MySQL クライアントを実行するローカル コンピューターで、次のコマンドを実行して MySQL ユーザーとしてログインします。

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    たとえば、MySQL のユーザー名が dbadmin3 で、仮想マシンの DNS 名が testmysql.cloudapp.net の場合、実際に実行するコマンドは次のようになります。

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## その他のリソース

MySQL の詳細については、次を参照してください。、 [MySQL のドキュメント](http://dev.mysql.com/doc/)します。


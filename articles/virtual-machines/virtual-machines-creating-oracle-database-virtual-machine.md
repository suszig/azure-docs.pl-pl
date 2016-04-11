<properties
    pageTitle="Azure クラシック ポータルで Oracle データベース VM を作成する |Microsoft Azure"
    description="従来のデプロイ モデルと Azure ポータルで、Oracle Database を使用して仮想マシンを作成する方法について説明します。"
    services="virtual-machines"
    authors="bbenz"
    documentationCenter=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="Windows"
    ms.workload="infrastructure-services"
    ms.date="06/22/2015"
    ms.author="bbenz" />

#Azure で Oracle Database 仮想マシンを作成する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


次の例は、Windows Server 2012 で実行しているマイクロソフト提供の Oracle Database のイメージに基づいて、Azure で仮想マシン (VM) を作成する方法を示しています。 次の 2 つの手順が含まれます。 最初に VM を作成し、VM 内に Oracle Database を作成します。 この例で示されているのは Oracle Database バージョン 12c ですが、手順はバージョン 11g とほぼ同じです。

##Azure で Oracle Database VM を作成するには

1.  ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  クリックして **Marketplace**, 、] をクリックして **コンピューティング**, 、し、入力 **Oracle** 検索ボックスにします。

3.  使用可能な Oracle データベースから 1 つ選択イメージを含む **バージョン 11 g、バージョン 12 c、Standard Edition、Enterprise Edition、または基本的なオプションまたは高度なオプションのバンドルのいずれかです。**(最小の推奨サイズ) などを選択すると、イメージに関する情報を確認し、クリックして **次**します。

4.  指定する **ホスト名** VM にします。

5.  指定する **ユーザー名** VM にします。 なお、このユーザーは VM にリモートでログインするためのもので、Oracle データベースのユーザー名ではありませんのでご注意ください。

6.  VM のパスワードを指定し確認するか、または SSH 公開キーを入力します。

7.  選択、 **価格レベル**します。  既定をクリックして、すべての構成オプションを表示することをお勧めの価格レベルが表示されることに注意してください **をすべて表示** 右上にします。

8.  必要に応じてオプションの構成を設定します。このとき、次に注意してください。

    a. ままにして **ストレージ アカウント** としてでは、VM 名で新しいストレージ アカウントを作成します。

    b. ままにして **可用性セット** 「未構成」とします。

    c. いずれかを入れないように **エンドポイント** この時点でします。

9.  リソース グループを選択または作成します。

10. 選択、 **サブスクリプション**します。

11. 選択、 **場所**します。

12. クリックして **作成**, と VM の作成のプロセスが開始します。 VM の状態が **を実行している**, 、次の手順に進みます。


##Azure で Oracle Database VM を使ってデータベースを作成するには

1.  ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  クリックして **仮想マシン**します。

3.  ログインする VM の名前をクリックします。

4.  クリックして **接続**します。

5.  表示される画面で必要に応じて入力して、VM に接続します。 管理者名とパスワードの入力画面が表示されたら、VM の作成時に指定した値を使用します。

6.  という環境変数を作成 **ORACLE_HOSTNAME** 、値、仮想マシンのコンピューター名に設定します。 環境変数は次の手順で作成できます。

    a.  Windows では、次のようにクリックします。 **開始**, 、型 **コントロール パネルの [**, 、] をクリック、 **コントロール パネルの [** アイコン] をクリック **システムとセキュリティ**, 、] をクリック **システム**, 、順にクリック **システムの詳細設定**します。

    b.  クリックして、 **詳細** タブをクリックし、をクリックして **環境変数**します。

    c.  下にある、 **システム変数** ] をクリックして **新規** 、変数を作成します。

    d.   **新しいシステム変数** ] ダイアログ ボックスに、入力 **ORACLE_HOSTNAME** 、変数の名前にし、値として、仮想マシンのコンピューター名を入力します。 コンピューター名を指定するをコマンド プロンプトを開いて実行 **SET COMPUTERNAME** (コマンドの出力は、コンピューター名が含むされます)。

    e.  をクリックして **OK** 変数と閉じる、新しい環境を保存、 **新しいシステム変数** ] ダイアログ ボックス。

    f.  コントロール パネルで開かれた他のダイアログ ボックスを閉じます。

7.  Windows では、次のようにクリックします。 **開始**, 、し、入力 **Database Configuration Assistant**します。 クリックして、 **Database Configuration Assistant** アイコン。

8.  内で、 **Database Configuration Assistant** ウィザード、ダイアログ ボックスのステップごとに、必要に応じて値を指定します。

    a.  **手順 1:** ] をクリックして **Create Database** ] をクリックし、 **次**します。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image5.png)

    b. **手順 2:** 値を入力 **グローバル データベース名**します。 入力し、確認の値 **管理パスワード**します。 このパスワードは、Oracle データベース、 **システム** ユーザーです。 クリア **コンテナー データベースとして作成**します。 クリックして **次**します。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image6.png)

    c. **手順 3:** 前提条件の確認を続行する、自動的にまで進んで **手順 4**します。

    d. **手順 4:** レビュー、 **データベースの作成 – 概要** オプション、およびクリック **完了**します。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image7.png)
    e. **手順 5:** 、 **進行状況] ページ** データベース作成の状態が報告されます。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image8.png)
    f. 使用するオプションがあります、データベースを作成した後、 **パスワード管理** ] ダイアログ ボックス。 お客様の要件に必要な場合、パスワード設定を変更し終了する] ダイアログ ボックスを閉じて、 **Database Configuration Assistant** ウィザード。

##データベースがインストールされていることを確認するには

1.  VM にまだログインしたまま、SQL Plus コマンド プロンプトを開始します。 Windows では、次のようにクリックします。 *開始** を入力し、 **SQL Plus**します。 クリックして、 **SQL Plus** アイコン。

2.  メッセージが表示されたら、ユーザーの名前を使用してログイン **システム** と Oracle データベースの作成時に指定するパスワード。

3.  SQL Plus コマンド プロンプトで、次のコマンドを実行します。

        **select \* from GLOBAL\_NAME;**

    結果に、作成したデータベースのグローバル名が表示されます。

    ![](media/virtual-machines-creating-oracle-database-virtual-machine/image9.png)

##データベースのリモート アクセスを許可する
データベースのリモート アクセスを許可するには (たとえば、Java コードを実行しているクライアント コンピューターから)、データベースのリスナーを開始し、仮想マシンのファイアウォールでポート 1521 を開き、ポート 1521 のパブリック エンドポイントを作成する必要があります。

### データベースのリスナーを開始する
1.  VM にログインします。

2.  コマンド プロンプトを開きます。

3.  コマンド プロンプトで、次のコマンドを実行します。

        **lsnrctl start**

> [AZURE.NOTE] 実行することができます **lsnrctl ステータス** リスナーの状態を確認します。 リスナーを停止するには、実行できるように **lsnrctl stop**します。

### 仮想マシンのファイアウォールでポート 1521 を開く

1.  クリックして、Windows では、仮想マシンにログインしたまま **開始**, 、型 **セキュリティが強化された Windows ファイアウォール**, 、順にクリックし、 **セキュリティが強化された Windows ファイアウォール** アイコン。 開き、 **セキュリティが強化された Windows ファイアウォール** 管理コンソールです。

2.  ファイアウォール管理コンソール内をクリックして **受信の規則** 左側のウィンドウで (表示されない場合 **受信の規則**, 、左側のウィンドウの最上位ノードを展開し)、順にクリック **新しいルール** 右側のウィンドウでします。

3.   **規則の種類**, [ **ポート** ] をクリックし、 **次**します。

4.   **プロトコルとポート**, を選択 **TCP**, を選択 **特定のローカル ポート**, 、入力 **1521年** ] をクリックし、ポートの **次**します。

5.  選択 **接続を許可する** ] をクリック **次**します。

6.  クリックして、ルールを適用するプロファイルの既定値のまま **次**します。

7.  ルールと、必要に応じて、説明の名前を指定し、クリックして **完了**します。

### ポート 1521 のパブリック エンドポイントを作成する

1.  ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  クリックして **参照**します。

3.  クリックして **仮想マシン**します。

4.  仮想マシンを選択します。

5.  クリックして **設定**します。

6.  クリックして **エンドポイント**します。

7.  クリックして **追加**します。

8.  エンドポイントの名前を指定します。

    a. 使用 **TCP** プロトコルです。

    b. 使用 **1521年** 、パブリック ポートです。

    c. 使用 **1521年** プライベート ポートにします。

9.  残りのオプションはそのままにします。

10. Click **OK**.

##Oracle Database Enterprise Manager のリモート アクセスを有効にする
Oracle Database Enterprise Manager のリモート アクセスを有効にする場合は、ファイアウォールでポート 5500 を開き、Azure クラシック ポータルで 5500 に対して仮想マシンのエンドポイントを作成します (ポート 1521 を開き、1521 に対してエンドポイントを作成する上記手順を使用)。 続いて、Oracle Enterprise Manager をリモート コンピューターから実行するには、ブラウザーを開いて `http://<<unique_domain_name>>:5500/em` の形式の URL にアクセスします。

> [AZURE.NOTE] 値を指定することができます *\ < \ < unique\_domain\_name\ > \ >* 内で、 [Azure クラシック ポータル](https://ms.portal.azure.com/) をクリックして **仮想マシン** し、Oracle Database を実行している仮想マシンを選択) します。

##基本的なオプションと高度なオプションのバンドルを構成する
選択した場合、 **と基本的なオプションの Oracle データベース** または **Oracle Database と高度なオプションのバンドル**, 、次の手順は、Oracle のインストールにアドオン機能を構成します。 Windows でこれらを設定する手順については、Oracle のマニュアルを参照してください。個々のコンポーネントのニーズ次第で構成は大きく変わります。

 **Oracle Database と基本的なオプションのバンドル** Oracle Database Enterprise Edition とライセンスに含まれるインスタンスが含まれます [Partitioning](http://www.oracle.com/us/products/database/options/partitioning/overview/index.html), 、[Active Data Guard](http://www.oracle.com/us/products/database/options/active-data-guard/overview/index.html), 、[データベース用の Oracle チューニング パック](http://docs.oracle.com/html/A86647_01/tun_ovw.htm), 、[データベースの Oracle Diagnostics Pack](http://docs.oracle.com/cd/B28359_01/license.111/b28287/options.htm#CIHIHDDJ), 、および [Oracle データベース用の管理パックのライフ サイクル](http://www.oracle.com/technetwork/oem/lifecycle-mgmt-495331.html)します。

 **Oracle Database と高度なオプションのバンドル** 、基本的なオプションのバンドル内のすべてのオプションのライセンスに含まれるインスタンスが含まれています plus [高度な圧縮](http://www.oracle.com/us/products/database/options/advanced-compression/overview/index.html), 、[セキュリティが強化](http://www.oracle.com/us/products/database/options/advanced-security/overview/index.html), 、[ラベル セキュリティ](http://www.oracle.com/us/products/database/options/label-security/overview/index.html), 、[データベース資格情報コンテナー](http://www.oracle.com/us/products/database/options/database-vault/overview/index.html), 、[Advanced Analytics](http://www.oracle.com/us/products/database/options/advanced-analytics/overview/index.html), 、[OLAP](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CIHGDEEF), 、[とグラフでは、空間](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CIHGDEEF), 、[メモリ内データベース キャッシュ](http://www.oracle.com/technetwork/products/timesten/overview/timesten-imdb-cache-101293.html), 、[データ マスキング パック](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CHDGEEBB), 、および Oracle テスト データの管理パック (Pack の一部として、データ マスク) します。

##その他のリソース
これで仮想マシンの設定とデータベースの作成が終わりました。追加情報については、次のトピックを参照してください。

-   [Oracle 仮想マシン イメージ - 他の考慮事項](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md)

-   [Oracle Database 12c ドキュメント ライブラリ](http://www.oracle.com/pls/db1211/homepage)

-   [Java アプリケーションから Oracle Database に接続する](http://docs.oracle.com/cd/E11882_01/appdev.112/e12137/getconn.htm#TDPJD136)

-   [Azure の Oracle 仮想マシン イメージ](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

-   [Oracle Database 2 日間 DBA 12c リリース 1](http://docs.oracle.com/cd/E16655_01/server.121/e17643/toc.htm)


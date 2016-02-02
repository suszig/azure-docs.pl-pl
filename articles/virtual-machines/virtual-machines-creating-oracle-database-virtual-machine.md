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


# Azure で Oracle Database 仮想マシンを作成する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


次の例は、Windows Server 2012 で実行しているマイクロソフト提供の Oracle Database のイメージに基づいて、Azure で仮想マシン (VM) を作成する方法を示しています。 次の 2 つの手順が含まれます。 最初に VM を作成し、VM 内に Oracle Database を作成します。 この例で示されているのは Oracle Database バージョン 12c ですが、手順はバージョン 11g とほぼ同じです。

## Azure で Oracle Database VM を作成するには

1.  ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  **[Marketplace]**、**[Compute]** をクリックし、続いて検索ボックスに**「Oracle」**と入力します。

3.  使用できる Oracle Database のイメージを 1 つ選択します **(バージョン 11g、バージョン 12c、Standard Edition、Enterprise Edition、または基本的なオプションまたは高度なオプションのバンドルのいずれか)**。選択したイメージに関する情報 (最小の推奨サイズなど) を確認し、**[次へ]** をクリックします。

4.  VM の**ホスト名**を指定します。

5.  VM の**ユーザー名**を指定します。 なお、このユーザーは VM にリモートでログインするためのもので、Oracle データベースのユーザー名ではありませんのでご注意ください。

6.  VM のパスワードを指定し確認するか、または SSH 公開キーを入力します。

7.  **[価格レベル]** を選択。 既定では推奨される価格レベルが表示されます。すべての構成オプションを見るには、右上の **[すべて表示]** をクリックします。

8.  必要に応じてオプションの構成を設定します。このとき、次に注意してください。

    a. VM 名で新しいストレージ アカウントを作成するには、**[ストレージ アカウント]** をそのままにします。

    b. **[可用性セット]** を「未構成」のままにします。

    c. この時点では**エンドポイント**を追加しないでください。

9.  リソース グループを選択または作成します。

10. **[サブスクリプション]** を選択します。

11. **[場所]** を選択します。

12. **[作成]** をクリックすると、VM を作成するプロセスが開始します。 VM の状態が **[実行中]** になったら、次の手順に進みます。


## Azure で Oracle Database VM を使ってデータベースを作成するには

1.  ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  **[Virtual Machines]** をクリックします。

3.  ログインする VM の名前をクリックします。

4.  **[接続]** をクリックします。

5.  表示される画面で必要に応じて入力して、VM に接続します。 管理者名とパスワードの入力画面が表示されたら、VM の作成時に指定した値を使用します。

6.  **ORACLE_HOSTNAME** という環境変数を作成します。この値は VM のコンピューター名に設定します。 環境変数は次の手順で作成できます。

    a. Windows で **[スタート]** をクリックし、「**コントロール パネル**」と入力して、**コントロール パネル** アイコンをクリックし、続いて **[システムとセキュリティ]**、**[システム]**、**[システムの詳細設定]** の順にクリックします。

    b. **[詳細設定]** タブをクリックし、続いて **[環境変数]**をクリックします。

    c. **[システム環境変数]** セクションの **[新規]** をクリックして変数を作成します。

    d. **[新しいシステム変数]** ダイアログ ボックスで、変数名に**「ORACLE_HOSTNAME」**を入力し、変数値に VM のコンピューター名を入力します。 コンピューター名を確認するには、コマンド プロンプトを開き **SET COMPUTERNAME** を実行します (このコマンドの出力にコンピューター名が含まれます)。

    e. **[OK]** クリックすると、新しい環境変数が保存され **[新しいシステム変数]** ダイアログ ボックスが閉じます。

    f. コントロール パネルで開かれた他のダイアログ ボックスを閉じます。

7.  Windows で **[スタート]** をクリックし、続いて「**Database Configuration Assistant**」と入力します。 **Database Configuration Assistant** アイコンをクリックします。

8.  **Database Configuration Assistant** ウィザード内で、ダイアログ ボックスの各手順で必要な値を指定します。

    a. **手順 1:** **[データベースを作成する]**、**[次へ]** の順にクリックします。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image5.png)

    b. **手順 2:** **[グローバル データベース名]** の値を入力します。 **[管理パスワード]** の値を入力し、確定します。 このパスワードは、Oracle データベースの**システム** ユーザー用のものです。 **[コンテナー データベースとして作成]** をオフにします。 **[次へ]** をクリックします。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image6.png)

    c. **手順 3:** 前提条件の確認が自動的に始まり、**手順 4** に進みます。

    d. **手順 4:** **[データベースの作成 – 概要]** オプションを確認し、続いて **[完了]** をクリックします。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image7.png)

    e. **手順 5:** **進行状況ページ**にデータベース作成の状態が表示されます。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image8.png)

    f. データベースを作成すると、**[パスワード管理]** ダイアログ ボックスを使えるようになります。 必要に応じてパスワード設定を変更し、続いてダイアログ ボックスが閉じて **[Database Configuration Assistant]** ウィザードを終了します。

## データベースがインストールされていることを確認するには

1.  VM にまだログインしたまま、SQL Plus コマンド プロンプトを開始します。 Windows では、次のようにクリックします。 * 開始**, を入力し、* * SQL Plus**します。 **SQL Plus** アイコンをクリックします。

2.  メッセージが表示されたら、**システム**のユーザー名と Oracle データベースを作成したときに指定したパスワードを使ってログインします。

3.  SQL Plus コマンド プロンプトで、次のコマンドを実行します。

        **select \* from GLOBAL\_NAME;**

    結果に、作成したデータベースのグローバル名が表示されます。

    ![](media/virtual-machines-creating-oracle-database-virtual-machine/image9.png)

## データベースのリモート アクセスを許可する

データベースのリモート アクセスを許可するには (たとえば、Java コードを実行しているクライアント コンピューターから)、データベースのリスナーを開始し、仮想マシンのファイアウォールでポート 1521 を開き、ポート 1521 のパブリック エンドポイントを作成する必要があります。

### データベースのリスナーを開始する

1.  VM にログインします。

2.  コマンド プロンプトを開きます。

3.  コマンド プロンプトで、次のコマンドを実行します。

        **lsnrctl start**


> [AZURE.NOTE] **lsnrctl status** を実行すると、リスナーの状態を確認できます。 リスナーを停止するには、**lsnrctl stop** を実行します。

### 仮想マシンのファイアウォールでポート 1521 を開く

1.  仮想マシンにログインしたままで、Windows で **[スタート]** をクリックし、**「セキュリティが強化された Windows ファイアウォール」**と入力し、続いて**セキュリティが強化された Windows ファイアウォール** アイコンをクリックします。 **[セキュリティが強化された Windows ファイアウォール]** 管理コンソールが開きます。

2.  ファイアウォール管理コンソール内で、左側のウィンドウの **[受信の規則]** をクリックし (**[受信の規則]** が表示されない場合は、左側のウィンドウの上のノードを展開します)、続いて右側のウィンドウの **[新しい規則]** をクリックします。

3.  **[規則の種類]** で、**[ポート]** を選択して **[次へ]** をクリックします。

4.  **[プロトコルとポート]** では、**[TCP]** を選択し、**[特定のローカル ポート]** を選択し、ポートに**「1521」**と入力し、続いて **[次へ]** をクリックします。

5.  **[接続を許可する]** を選択し、**[次へ]** をクリックします。

6.  規則を適用するプロファイルの既定値をそのまま使用し、**[次へ]** をクリックします。

7.  規則の名前とオプションで説明を指定し、続いて **[完了]** をクリックします。

### ポート 1521 のパブリック エンドポイントを作成する

1.  ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  **[参照]** をクリックします。

3.  **[Virtual Machines]** をクリックします。

4.  仮想マシンを選択します。

5.  **[設定]** をクリックします。

6.  **[エンドポイント]** をクリックします。

7.  **[追加]** をクリックします。

8.  エンドポイントの名前を指定します。

    a. プロトコルに **TCP** を使用します。

    b. パブリック ポートに **1521** を使用します。

    c. プライベート ポートに **1521** を使用します。

9.  残りのオプションはそのままにします。

10. **[OK]** をクリックします。

## Oracle Database Enterprise Manager のリモート アクセスを有効にする

Oracle Database Enterprise Manager のリモート アクセスを有効にする場合は、ファイアウォールでポート 5500 を開き、Azure クラシック ポータルで 5500 に対して仮想マシンのエンドポイントを作成します (ポート 1521 を開き、1521 に対してエンドポイントを作成する上記手順を使用)。次に、実行するには、Oracle Enterprise Manager、リモート コンピューターからの形式で URL にブラウザーを開いて `http://<<unique_domain_name>>:5500/em`します。
> [AZURE.NOTE] 値を指定することができます * \<\<unique\_domain\_name\>\ > * 内で、 [Azure クラシック ポータル](https://ms.portal.azure.com/) をクリックして **仮想マシン** し、Oracle Database を実行している仮想マシンを選択) します。

## 基本的なオプションと高度なオプションのバンドルを構成する

**Oracle Database と基本的なオプション** または**Oracle Database と高度なオプションのバンドル** を選択した場合、次の手順で Oracle のインストールにアドオン機能を構成します。 Windows でこれらを設定する手順については、Oracle のマニュアルを参照してください。個々のコンポーネントのニーズ次第で構成は大きく変わります。

**Oracle Database と基本的なオプションのバンドル** Oracle Database Enterprise Edition とライセンスに含まれるインスタンスが含まれます [Partitioning](http://www.oracle.com/us/products/database/options/partitioning/overview/index.html), 、[Active Data Guard](http://www.oracle.com/us/products/database/options/active-data-guard/overview/index.html), 、[データベース用の Oracle チューニング パック](http://docs.oracle.com/html/A86647_01/tun_ovw.htm), 、[データベースの Oracle Diagnostics Pack](http://docs.oracle.com/cd/B28359_01/license.111/b28287/options.htm#CIHIHDDJ), 、および [Oracle データベース用の管理パックのライフ サイクル](http://www.oracle.com/technetwork/oem/lifecycle-mgmt-495331.html)します。

**Oracle Database と高度なオプションのバンドル** 、基本的なオプションのバンドル内のすべてのオプションのライセンスに含まれるインスタンスが含まれています plus [高度な圧縮](http://www.oracle.com/us/products/database/options/advanced-compression/overview/index.html), 、[高度なセキュリティ](http://www.oracle.com/us/products/database/options/advanced-security/overview/index.html), 、[ラベル セキュリティ](http://www.oracle.com/us/products/database/options/label-security/overview/index.html), 、[データベース資格情報コンテナー](http://www.oracle.com/us/products/database/options/database-vault/overview/index.html), 、[Advanced Analytics](http://www.oracle.com/us/products/database/options/advanced-analytics/overview/index.html), 、[OLAP](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CIHGDEEF), 、[とグラフでは、空間](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CIHGDEEF), 、[メモリ内データベース キャッシュ](http://www.oracle.com/technetwork/products/timesten/overview/timesten-imdb-cache-101293.html), 、[データ マスキング パック](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CHDGEEBB), 、および Oracle テスト データの管理パック (Pack の一部として、データ マスク) します。

## その他のリソース

これで仮想マシンの設定とデータベースの作成が終わりました。追加情報については、次のトピックを参照してください。

-   [Oracle 仮想マシン イメージ - 他の考慮事項](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md)

-   [Oracle Database 12c ドキュメント ライブラリ](http://www.oracle.com/pls/db1211/homepage)

-   [Java アプリケーションから Oracle データベースに接続します。](http://docs.oracle.com/cd/E11882_01/appdev.112/e12137/getconn.htm#TDPJD136)

-   [Azure の oracle 仮想マシン イメージ](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

-   [Oracle Database 2 日間 DBA 12c リリース 1](http://docs.oracle.com/cd/E16655_01/server.121/e17643/toc.htm)






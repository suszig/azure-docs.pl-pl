<properties 
   pageTitle="HDInsight での Apache Phoenix および SQuirreL の使用 | Microsoft Azure" 
   description="Apache Phoenix を HDInsight で使用する方法、およびワークステーションに SQuirreL をインストールして HDInsight の HBase クラスターに接続するように構成する方法について説明します。" 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/02/2015"
   ms.author="jgao"/>

# HDInsight での Phoenix、SQuirreL、HBase クラスターの使用  

使用する方法について説明 [Apache Phoenix](http://phoenix.apache.org/) HDInsight、およびインストールして HDInsight での HBase クラスターに接続するワークステーションに SQuirreL を構成する方法です。 Phoenix の詳細については、次を参照してください。 [Phoenix 15 分以内に](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html)します。 Phoenix の文法を参照してください。 [Phoenix の文法](http://phoenix.apache.org/language/index.html)します。

>[AZURE.NOTE] HDInsight での Phoenix のバージョンについて、次を参照してください。 [HDInsight で提供される Hadoop クラスター バージョンの新機能ですか?][hdinsight-versions]します。

##SQLLine の使用
[SQLLine](http://sqlline.sourceforge.net/) は SQL を実行するコマンド ライン ユーティリティです。 

###前提条件
SQLLine を使用するには、以下のものが必要です。

- **HDInsight での HBase クラスター**します。 クラスターの HBase プロビジョニングについては、「 [HDInsight で Apache hbase][hdinsight-hbase-get-started]します。
- **リモート デスクトップ プロトコルを使用して HBase クラスターに接続**します。 手順については、次を参照してください。 [Azure クラシック ポータルを使用して HDInsight で Hadoop の管理のクラスター][hdinsight-manage-portal]します。

**ホスト名を確認するには**

1. 開いている **Hadoop コマンド ライン** デスクトップからです。
2. 次のコマンドを実行して、DNS サフィックスを取得します。

        ipconfig

    書き留めて **接続専用 DNS サフィックス**します。 たとえば、 *myhbasecluster.f5.internal.cloudapp.net*します。 HBase クラスターに接続するときは、FQDN を使用して Zookeeper のいずれかに接続する必要があります。 各 HDInsight クラスターには 3 つの Zookeeper があります。  *Zookeeper0*, 、*zookeeper1*, 、および *zookeeper2*します。 FQDN はのようなものになります *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*します。

**SQLLine を使用するには**

1. 開いている **Hadoop コマンド ライン** デスクトップからです。
2. 次のコマンドを実行して、SQLLine を開きます。

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![hdinsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    サンプルで使用されているコマンド:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
        
        !tables;
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;

詳細については、次を参照してください。 [SQLLine のマニュアル](http://sqlline.sourceforge.net/#manual) と [Phoenix の文法](http://phoenix.apache.org/language/index.html)します。


















##SQuirreL の使用

[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) 、JDBC 準拠データベースの構造を表示するテーブル内のデータの参照などの SQL コマンドにできるようにするグラフィカルな Java プログラムです。これを使用して、HDInsight の Apache Phoenix に接続できます。

このセクションでは、ワークステーションに SQuirreL をインストールし、VPN 経由で HDInsight の HBase クラスターに接続するように構成する方法について説明します。 

###前提条件

手順を実行する前に、以下のものが必要です。

- DNS 仮想マシンで Azure 仮想ネットワークにデプロイされた HBase クラスター。  手順については、次を参照してください。 [HBase クラスターをプロビジョニング Azure Virtual Network で][hdinsight-hbase-provision-vnet]します。 

    >[AZURE.IMPORTANT] 仮想ネットワークに DNS サーバーをインストールする必要があります。 手順については、次を参照してください [2 つの Azure 仮想ネットワーク間の DNS の構成。](hdinsight-hbase-geo-replication-configure-DNS.md)

- HBase クラスターの接続固有の DNS サフィックスを取得します。 そのためには、RDP でクラスターに接続して、IPConfig を実行します。  DNS サフィックスの例を次に示します。

        myhbase.b7.internal.cloudapp.net
- ダウンロードしてインストール [Microsoft Visual Studio Express 2013 for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) ワークステーションにします。 パッケージから makecert を実行して証明書を作成する必要があります。  
- ダウンロードしてインストール [Java ランタイム環境](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) ワークステーションにします。  SQuirreL SQL クライアント バージョン 3.0 以降には、JRE バージョン 1.6 以降が必要です。  


###Azure 仮想ネットワークへのポイント対サイト VPN 接続を構成します。

ポイント対サイト VPN 接続の構成には 3 つの手順があります。

1. [仮想ネットワークと動的ルーティング ゲートウェイの構成](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [証明書の作成](#Create-your-certificates)
3. [VPN クライアントの構成](#Configure-your-VPN-client)

参照してください [を Azure Virtual Network ポイント対サイト VPN 接続を構成する](../vpn-gateway/vpn-gateway-point-to-site-create.md) の詳細。

#### 仮想ネットワークと動的ルーティング ゲートウェイの構成

Azure 仮想ネットワークに HBase クラスターをプロビジョニングしてあることを確認します (このセクションの前提条件を参照)。 次の手順では、ポイント対サイト接続を構成します。

**ポイント対サイト接続を構成するには**

1. サインイン、 [Azure クラシック ポータル][azure-portal]します。
2. 左側で、次のようにクリックします。 **ネットワーク**します。
3. 作成した仮想ネットワーク] をクリックして (を参照してください [HBase クラスターをプロビジョニング Azure Virtual Network で][hdinsight-hbase-provision-vnet])。
4. クリックして **構成** 上部にあります。
5.  **ポイント対サイト接続** ] セクションの [ **ポイント対サイト接続を構成します**します。 
6. 構成 **開始 IP** と **CIDR** VPN クライアントが接続されている場合に、IP アドレスを受信元となる IP アドレス範囲を指定します。 この範囲は、オンプレミスのネットワークに存在する範囲および接続先の Azure 仮想ネットワークと重複することはできません。 たとえば次のようになります。 仮想ネットワークに対して 10.0.0.0/20 を選択した場合は、クライアント アドレス空間の 10.1.0.0/24 を選択できます。  [ポイント対サイト接続を参照してください][vnet-point-to-site-connectivity] 詳細については、ページです。
7. 仮想ネットワーク アドレス空間セクションで、クリックして **ゲートウェイ サブネットの追加**します。
7. クリックして **保存** 、ページの下部にあります。
8. クリックして **はい** 、変更内容を確認します。 システムが変更を完了するまで待ってから、次の手順に進みます。


**動的ルーティング ゲートウェイを作成するには**

1. Azure クラシック ポータルでは、次のようにクリックします。 **ダッシュ ボード** 、ページの上部にあります。
2. クリックして **ゲートウェイの作成** 、ページの下部にあります。
3. クリックして **はい** を確認します。 ゲートウェイが作成されるまで待ちます。
4. クリックして **ダッシュ ボード** 上部にあります。  仮想ネットワークの図が表示されます。

    ![Azure 仮想ネットワークのポイント対サイト仮想ダイアグラム][img-vnet-diagram] 

    図ではクライアント接続が 0 であることが示されています。 仮想ネットワークに接続すると、この値が 1 に更新されます。 

#### 証明書の作成

付属している証明書作成ツール (makecert.exe) を使用して、X.509 証明書を作成する方法の 1 つは、 [Microsoft Visual Studio Express 2013 for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx)します。 


**自己署名ルート証明書を作成するには**

1. ワークステーションからコマンド プロンプト ウィンドウを開きます。
2. Visual Studio ツールのフォルダーに移動します。 
3. 次の例のコマンドを使用すると、ルート証明書が作成されてワークステーションの個人用証明書ストアにインストールされ、後で Azure クラシック ポータルにアップロードする対応する .cer ファイルが作成されます。 

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    .cer ファイルを格納するディレクトリに移動します。HBaseVnetVPNRootCertificate は、証明書に使用する名前です。 

    コマンド プロンプトを閉じないでください。  これは、次の手順で必要になります。

    >[AZURE.NOTE] クライアント証明書の生成元となるルート証明書を作成してこの証明書の秘密キーと共にエクスポートし、回復で安全な場所に保存することもできます。 

**クライアント証明書を作成するには**

- 同じコマンド プロンプトから (ルート証明書を作成したのと同じコンピューターを使用する必要があります。 クライアント証明書はルート証明書から生成する必要があります)、次のコマンドを実行します。

        makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate は、ルート証明書の名前を示します。  この名前は、ルート証明書の名前と一致する必要があります。  

    ルート証明書とクライアント証明書は、どちらもコンピューター上の個人証明書ストアに格納されます。 確認するには、certmgr.msc を使用します。

    ![Azure 仮想ネットワークのポイント対サイト VPN 証明書][img-certificate]

    クライアント証明書は、仮想ネットワークに接続する各コンピューターにインストールする必要があります。 仮想ネットワークに接続する各コンピューターに、一意のクライアント証明書を作成することをお勧めします。 クライアント証明書をエクスポートするには、certmgr.msc を使用します。 

**Azure クラシック ポータルにルート証明書をアップロードするには**

1. Azure クラシック ポータルでは、次のようにクリックします。 **ネットワーク** 左側です。
2. HBase クラスターのデプロイ先の仮想ネットワークをクリックします。
3. クリックして **証明書** 上部にあります。
4. クリックして **アップロード** 下部にある、最後の前に、の手順で作成したルート証明書ファイルを指定します。 証明書がインポートされるまで待ちます。
5. クリックして **ダッシュ ボード** 上部にあります。  仮想図に状態が表示されます。


#### VPN クライアントの構成



**クライアント VPN パッケージをダウンロードしてインストールするには**

1. [概要] セクションで、仮想ネットワークのダッシュ ボード ページからいずれかをクリックして **64 ビット クライアント VPN パッケージのダウンロード** または **32 ビット クライアント VPN パッケージのダウンロード** ワークステーションの OS バージョンに基づいています。
2. クリックして **実行** パッケージをインストールします。
3. セキュリティのプロンプトで次のようにクリックします。 **詳細**, 、] をクリックし、 **実行]**します。
4. クリックして **はい** 2 回クリックします。

**VPN に接続するには**

1. ワークステーションのデスクトップで、タスク バーの [ネットワーク] アイコンをクリックします。 仮想ネットワーク名と共に VPN 接続が表示されます。
2. VPN 接続の名前をクリックします。
3. クリックして **接続**します。

**VPN 接続とドメイン名の解決をテストするには**

- ワークステーションでコマンド プロンプトを開き、HBase クラスターの DNS サフィックスが myhbase.b7.internal.cloudapp.net である次のいずれかの名前に ping を実行します。

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

###ワークステーションへの SQuirreL のインストールと構成

**SQuirreL をインストールするには**

1. SQuirreL SQL クライアントの jar ファイルをダウンロード [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation)します。
2. jar ファイルを開くか実行します。 必要な [Java ランタイム環境](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html)します。
3. クリックして **次** 2 回クリックします。
4. 書き込みアクセス許可し、クリックする場所のパスを指定して **次**します。
    >[AZURE.NOTE] 既定のインストール フォルダーは、C:\Program 3.6 sql Files\squirrel フォルダーにです。  このパスに書き込むには、インストーラーに管理者特権を付与する必要があります。 管理者としてコマンド プロンプトを開き、Java の bin フォルダーに移動して実行します。 
    >
    >     java.exe -jar [the path of the SQuirreL jar file] 
5. クリックして **OK** ターゲット ディレクトリの作成を確認します。
6. 既定の設定では、Base および Standard パッケージがインストールされます。  クリックして **次**します。
7. をクリックして **次へ]** を 2 回クリックして **実行**します。


**Phoenix ドライバーをインストールするには**

Phoenix ドライバーの jar ファイルは、HBase クラスターにあります。 パスは、バージョンに基づく次のようなものです。

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
これを、ワークステーションの [SQuirreL インストール フォルダー]/lib パスにコピーする必要があります。  最も簡単な方法は、RDP でクラスターに接続し、ファイルのコピー/貼り付け (Ctrl + C および Ctrl + V) を使用してワークステーションにコピーします。

**Phoenix ドライバーを SQuirreL に追加するには**

1. ワークステーションから SQuirreL SQL クライアントを開きます。
2. クリックして、 **ドライバー** 左側のタブをクリックします。
2.  **ドライバー** ] メニューをクリックして **新しいドライバー**します。
3. 次の情報を入力します。

    - **名前**: Phoenix
    - **例の URL**: jdbc:phoenix:zookeeper2.contoso hbase-eu.f5.internal.cloudapp.net
    - **クラス名**: org.apache.phoenix.jdbc.PhoenixDriver

    >[AZURE.WARNING] ユーザー URL の例ですべて小文字に変換します。 いずれかがダウンした場合に備えて、完全な zookeeper クォーラムを使用できます。  ホスト名は、zookeeper0、zookeeper1、zookeeper2 です。

    ![HDInsight HBase Phoenix SQuirreL ドライバー][img-squirrel-driver]
4. Click **OK**.

**HBase クラスターの別名を作成するには**

1. Squirrel で] をクリックして、 **エイリアス** 左側のタブをクリックします。
2.  **エイリアス** ] メニューのをクリックして **新しいエイリアス**します。
3. 次の情報を入力します。

    - **名前**: HBase クラスターまたは任意の名前の名前。
    - **ドライバー**: Phoenix。  これは、前の手順で作成したドライバー名と一致する必要があります。
    - **URL**: ドライバーの構成から URL をコピーします。 すべて小文字を使用してください。
    - **ユーザー名**: 任意のテキストを指定できます。  ここでは VPN 接続を使用するため、ユーザー名はまったく使用されません。
    - **パスワード**: 任意のテキストを指定できます。

    ![HDInsight HBase Phoenix SQuirreL ドライバー][img-squirrel-alias]
4. クリックして **テスト**します。 
5. クリックして **接続**します。 接続が確立されると、SQuirreL の表示は次のようになります。

    ![HBase Phoenix SQuirreL][img-squirrel]

**テストを実行するには**

1. クリックして、 **SQL** タブの右側、 **オブジェクト** ] タブをクリックします。
2. 次のコードをコピーして貼り付けます。

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. 実行ボタンをクリックします。

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. 戻り、 **オブジェクト** ] タブをクリックします。
5. エイリアス名を展開し、展開 **テーブル**します。  新しいテーブルが下に一覧表示されます。
 
##次のステップ
この記事では、HDInsight で Apache Phoenix を使用する方法を説明しました。  詳細については、次を参照してください。

- [HDInsight HBase の概要][hdinsight-hbase-overview]:
HBase は、Hadoop 上に構築された Apache オープン ソースの NoSQL データベースです。大量の非構造化データおよび半構造化データに対するランダム アクセスと強力な一貫性を実現します。
- [Azure Virtual Network での HBase クラスターのプロビジョニング][hdinsight-hbase-provision-vnet]:
アプリケーションが HBase と直接通信できるように、仮想ネットワーク統合を使用して、HBase クラスターをアプリケーションと同じ仮想ネットワークにデプロイできます。
- [HDInsight での HBase レプリケーションの構成](hdinsight-hbase-geo-replication.md): 2 つの Azure データ センター間の HBase レプリケーションを構成する方法について説明します。 
- [HDInsight での HBase で Twitter のセンチメントを分析][hbase-twitter-sentiment]:
方法については、リアルタイム [センチメント分析](http://en.wikipedia.org/wiki/Sentiment_analysis) HDInsight の Hadoop クラスターで HBase を使用して、ビッグ データのです。

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: ../hdinsight-hbase-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 


<properties
    pageTitle="HBase のチュートリアル: Hadoop での HBase の使用 | Microsoft Azure"
    description="HDInsight の Hadoop で Apache HBase を使用するには、この HBase チュートリアルの手順に従ってください。 HBase シェルからテーブルを作成し、Hive を使用したクエリを実行します。"
    keywords="apache hbase,hbase,hbase shell,hbase tutorial"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/02/2015"
    ms.author="jgao"/>



# HBase チュートリアル: HDInsight の Hadoop で Apache HBase を使用する (Linux)

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]


HDInsight で HBase クラスターを作成する方法、HBase テーブルを作成する方法、Hive を使用してテーブルを照会する方法について説明します。 HBase の概要については、次を参照してください。 [HDInsight HBase の概要][hdinsight-hbase-overview]します。

> [AZURE.NOTE] このドキュメントの情報は、Linux ベースの HDInsight クラスターに固有です。 Windows ベースのクラスターについては、次を参照してください。 [(Windows) を HDInsight での Hadoop での Apache HBase の使用](hdinsight-hbase-tutorial-get-started.md)します。

###前提条件

この HBase のチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- PuTTY と PuTTYGen (Windows クライアント)。 これらのユーティリティは [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) から入手できます。
- [curl](http://curl.haxx.se/download.html)します。

## HBase クラスターの作成

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Azure プレビュー ポータルを使用して HBase クラスターを作成するには**


1. サインイン、 [Azure プレビュー ポータル][azure-portal]します。
2. クリックして **新規** 左上にあるコーナー、クリックして **データ + 分析**, 、**HDInsight**します。
3. 次の値を入力します。


    - **Cluster Name** - Enter a name to identify this cluster.
    - **Cluster Type** - Select **HBase**.
    - **Cluster Operating System**: Select **Linux**. For creating Windows-based HBase cluster, see  [HBase tutorial: Get started using Apache HBase with Hadoop in HDInsight (Windows)](hdinsight-hbase-tutorial-get-started.md).
    - **Version** - Select an HBase version.
    - **Subscription** - Select your Azure subscription used for creating this cluster.
    - **Resource Group** -  Create a new Azure resource group or select an existing one. For more information, see [Azure Resource Manager Overview](resource-group-overview.md)
    - **Credentials**.  Enter a password for the HTTP web service user. The default username is **admin**. You must also enter an **SSH Username** and either a **PASSWORD** or **PUBLIC KEY**, which will be used to authenticate the SSH user. Using a public key is the recommended approach. For more information on using SSH with HDInsight, see one of the following articles:

        - [Use SSH with Linux-based Hadoop on HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
        - [Use SSH with Linux-based Hadoop on HDInsight from Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    Click **Select** to save the changes.
    - **Data Source** - create a new Azure storage account or select an existing Azure storage account to be used as the default file system for the cluster. The default storage account location determines the location of the cluster location. The default storage account and the cluster must co-locate in the same data center.
    - **Note Pricing Tiers** - Select the number of region servers for the HBase cluster

        > [AZURE.WARNING] For high availability of HBase services, you must create a cluster that contains at least **three** nodes. This ensures that, if one node goes down, the HBase data regions are available on other nodes.

        > If you are learning HBase, always choose 1 for the cluster size, and delete the cluster after each use to reduce the cost.

    - **Optional Configuration** - Configure Azure virtual network, configure Script actions, and add additional storage accounts.

4. クリックして **作成**します。

>[AZURE.NOTE] HBase クラスターを削除した後は、同じ既定の blob コンテナーを使用して、別の HBase クラスターを作成できます。 新しいクラスターでは、元のクラスターで作成した HBase テーブルを選択します。

## HBase シェルの使用

多くの場合、データは次のような表形式で表示されます。

![hdinsight hbase tabular data][img-hbase-sample-data-tabular]

BigTable の実装である HBase では、同じデータが次のように表示されます。

![hdinsight hbase bigtable data][img-hbase-sample-data-bigtable]

次の手順を完了すると、この操作をよく理解できます。  


**HBase シェルを使用するには**

>[AZURE.NOTE] この手順については、Windows コンピューターから次のとおりを説明します。  Linux、Unix または OS X から Linux ベースの HDInsight に接続するための指示のクラスターを参照してください [SSH Linux、Unix、または OS X (プレビュー) から HDInsight 上の Linux ベースの Hadoop の使用](hdinsight-hadoop-linux-use-ssh-unix.md)
1. 開いている **PuTTY**します。  記事の冒頭に掲載されている前提条件を参照してください。
2. 作成プロセスでユーザー アカウントの作成時に SSH キーを指定した場合は、次の手順に従って、クラスターへの認証時に使用する秘密キーを選択する必要があります。

     **カテゴリ**, 、展開 **接続**, 、展開 **SSH**, を選択して **Auth**します。 最後に、クリックして **参照** し、秘密キーが含まれた .ppk ファイルを選択します。

3.  **カテゴリ**, 、クリックして **セッション**します。
4. PuTTY セッション画面の基本オプションで、次の値を入力します。

    - ホスト名: [ホスト名 (または IP アドレス)] フィールドの HDInsight サーバーの SSH アドレス。 SSH アドレスは、クラスター名 **-ssh.azurehdinsight.net**します。 たとえば、 *mycluster-ssh.azurehdinsight.net*します。
    - ポート: 22。 ヘッド ノード 0 の SSH ポートは 22 です。  参照してください [(プレビュー) を Linux で HDInsight を使用する方法については](hdinsight-hadoop-linux-information.md#remote-access-to-services)です。
4. クリックして **開く** 、クラスターに接続します。
5. プロンプトが表示されたら、クラスターの作成時に入力したユーザーを入力します。 ユーザーにパスワードを指定している場合は、その入力も求められます。
6. 次のコマンドを実行します。

        hbase shell

4. 2 つの列ファミリを持つ HBase を作成します。

        create 'Contacts', 'Personal', 'Office'
        list
5. いくつかのデータを挿入します。

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase shell][img-hbase-shell]

6. 1 つの行を取得します。

        get 'Contacts', '1000'

    行は 1 行のみのため、スキャン コマンドを使用した場合と同じ結果が得られます。

    HBase テーブル スキーマの詳細については、次を参照してください。 [HBase スキーマのデザインの概要][hbase-schema]します。 HBase コマンドについて [Apache HBase リファレンス ガイド][hbase-quick-start]します。


6. シェルを終了します。

        exit

**Contacts HBase テーブルにデータを一括で読み込むには**

HBase では、いくつかの方法でテーブルにデータを読み込ことができます。  詳細については、次を参照してください。 [一括読み込みを行う](http://hbase.apache.org/book.html#arch.bulk.load)します。


サンプル データ ファイルがパブリック blob コンテナーにアップロードされて wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt します。  このデータ ファイルの内容は次のとおりです。

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

必要に応じて、自分でテキスト ファイルを作成し、そのファイルを自分のストレージ アカウントにアップロードできます。 手順については、次を参照してください。 [HDInsight での Hadoop ジョブ用データのアップロード][hdinsight-upload-data]します。

> [AZURE.NOTE] この手順では、前の手順で作成した Contacts HBase テーブルを使用します。

1. 開いている **PuTTY**, 、クラスターに接続します。  前の手順の指示を参照してください。
3. 次のコマンドを実行して、データ ファイルをストア ファイルに変換し、Dimporttsv.bulk.output で指定された相対パスに格納します。

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. /Example/data/storeDataFileOutput から HBase テーブルにデータをアップロードするには、次のコマンドを実行します。

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. HBase シェルを開き、スキャン コマンドを使用して、テーブルの内容の一覧を表示することができます。



## Hive を使用して HBase テーブルを照会する

Hive を使用して HBase テーブルのデータを照会できます。 このセクションでは、HBase テーブルにマッピングする Hive テーブルを作成し、作成した Hive テーブルを使用して HBase テーブルのデータを照会します。


1. 開いている **PuTTY**, 、クラスターに接続します。  前の手順の指示を参照してください。
2. Hive シェルを開きます。

    hive
3. HBase テーブルにマッピングする Hive テーブルを作成するのには、次の HiveQL スクリプトを実行します。 ここで、HBase シェルを使用して、先ほど参照したサンプル テーブルが HBase に作成されたことを確認してから、このステートメントを実行してください。

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. 次の HiveQL スクリプトを実行します。 次のように Hive クエリが HBase テーブル内のデータを照会します。

        SELECT count(*) FROM hbasecontacts;

## Curl を使用して HBase REST API を使用する

> [AZURE.NOTE] Curl、またはその他の REST 通信で WebHCat を使用する場合は、HDInsight クラスターの管理者ユーザー名とパスワードを提供することで要求を認証する必要があります。 また、サーバーへの要求の送信に使用する Uniform Resource Identifier (URI) にクラスター名を含める必要があります。
>
> ここでのコマンド **ユーザー名** をクラスターへの認証し、置換するユーザー **パスワード** ユーザー アカウントのパスワードを使用します。 置換 **CLUSTERNAME** 、クラスターの名前に置き換えます。
>
> REST API のセキュリティは、[基本認証](http://en.wikipedia.org/wiki/Basic_access_authentication)を通じて保護されています。 資格情報をサーバーに安全に送信するには、必ずセキュア HTTP (HTTPS) を使用して要求を行う必要があります。

1. コマンド ラインで次のコマンドを使用して、HDInsight クラスターに接続できることを確認します。

        curl -u <UserName>:<Password> -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    次のような応答を受け取ります。

    {"status":"ok","version":"v1"}

  このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-u** - The user name and password used to authenticate the request.
    * **-G** - Indicates that this is a GET request.

2. 次のコマンドを使用して、既存の HBase テーブルを一覧表示します。

        curl -u <UserName>:<Password> -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. 次のコマンドを使用して、2 つの列ファミリがある新しい HBase テーブルを作成します。

        curl -u <UserName>:<Password> -v -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" -H "Accept: application/json" -H "Content-Type: application/json" -d "{\"@name\":\"test\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}"

    スキーマは、JSon 形式で提供されます。

4. 次のコマンドを使用して、一部のデータを挿入します。

        curl -u <UserName>:<Password> -v -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" -H "Accept: application/json" -H "Content-Type: application/json" -d "{\"Row\":{\"key\":\"1000\",\"Cell\":{\"column\":\"Personal:Name\", \"$\":\"John Dole\"}}}"

5. 次のコマンドを使用して、1 行を取得します。

        curl -u <UserName>:<Password> -v -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" -H "Accept: application/json"

## クラスターの状態の確認

HDInsight の HBase には、クラスターを監視するための Web UI が付属します。 この Web UI を使用すると、統計情報やリージョンに関する情報を要求できます。

SSH を使用して、Web 要求などのローカルの要求を HDInsight クラスターにトンネリングすることもできます。 ここでは、最初から HDInsight クラスター ヘッド ノード上にあったかのように、要求が要求済みリソースにルーティングされます。 詳細については、次を参照してください。 [SSH (プレビュー) の Windows から Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel)です。

**SSH トンネリング セッションを確立するには**

1. 開いている **PuTTY**します。  
2. 作成プロセスでユーザー アカウントの作成時に SSH キーを指定した場合は、次の手順に従って、クラスターへの認証時に使用する秘密キーを選択する必要があります。

     **カテゴリ**, 、展開 **接続**, 、展開 **SSH**, を選択して **Auth**します。 最後に、クリックして **参照** し、秘密キーが含まれた .ppk ファイルを選択します。

3.  **カテゴリ**, 、クリックして **セッション**します。
4. PuTTY セッション画面の基本オプションで、次の値を入力します。

    - **ホスト名**: HDInsight サーバーのホスト名 (または IP アドレス) にフィールドの SSH アドレス。 SSH アドレスは、クラスター名 **-ssh.azurehdinsight.net**します。 たとえば、 *mycluster-ssh.azurehdinsight.net*します。
    - **ポート**: 22 です。 ヘッド ノード 0 の SSH ポートは 22 です。  
5.  **カテゴリ** ダイアログ ボックスの左側のセクションで、展開 **接続**, 、展開 **SSH**, 、クリックして **トンネル**します。
6. [SSH ポートの転送を管理するオプション] フォームに次の情報を入力します。

    - **ソース ポート** -を転送する場合、クライアント上のポートです。 (9876 など)。
    - **動的** -動的な SOCKS プロキシを有効します。
7. クリックして **追加** 設定を追加します。
8. クリックして **開く** SSH 接続を開く] ダイアログ ボックスの下部にあります。
9. メッセージが表示されたら、SSH アカウントを使用してサーバーにログインします。 これにより、SSH セッションが確立され、トンネルが有効になります。

**Ambari を使用して Zookeeper の FQDN を確認するには**

1. Https://を参照します。<ClusterName>.azurehdinsight.net/します。
2. クラスター ユーザー アカウントの資格情報を 2 回入力します。
3. 左側のメニューからクリックして **zookeeper**します。
4. 3 つのうちのいずれかを **ZooKeeper Server** 概要のリストからのリンク。
5. コピー **Hostname**します。 (例: zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net)。

**プログラム (Fireｆox) を構成してクラスターの状態を確認するには**

1. FireFox を開きます。
2. クリックして、 **メニューを開く]** ] ボタンをクリックします。
3. クリックして **オプション**します。
4. をクリックして **詳細**, 、] をクリックして **ネットワーク**, 、順にクリック **設定**します。
5. 選択 **手動プロキシ構成**します。
6. 次の値を入力します。

    - **ホストを socks**: localhost
    - **ポート**: Putty SSH トンネリングで構成されている同じポートを使用します。  (9876 など)。
    - **SOCKS v5**: (オン)
    - **リモート DNS**: (オン)
7. クリックして **OK** 変更を保存します。
8. Http://を参照します。<TheFQDN of a ZooKeeper>: 60010/マスターの状態

高可用性クラスターの場合は、Web UI をホストしている現在アクティブな HBase マスター ノードへのリンクがあります。



## 次の手順
この HDInsight の HBase のチュートリアルでは、HBase クラスターの作成方法と、テーブルを作成してそのテーブルのデータを HBase シェルから表示する方法について学習しました。 また、Hive を使用して HBase テーブルのデータを照会する方法、HBase C# REST API を使用して HBase テーブルを作成し、テーブルからデータを取得する方法についても学習しました。

詳細については、次を参照してください。

- [HDInsight HBase の概要][hdinsight-hbase-overview]:
HBase は、Hadoop 上に構築された Apache オープン ソースの NoSQL データベースです。大量の非構造化データおよび半構造化データに対するランダム アクセスと強力な一貫性を実現します。


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png


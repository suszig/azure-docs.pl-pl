<properties 
    pageTitle="Script Action を使用して Hadoop クラスターに Solr をインストールする | Microsoft Azure" 
    description="Solr を使用して HDInsight クラスターをカスタマイズする方法について説明します。 Solr をインストールするスクリプトを使用するために、Script Action の構成オプションを使用します。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/02/2015" 
    ms.author="nitinme"/>

# HDInsight Hadoop クラスターに Solr をインストールして使用する


Script Action を使用して Windows ベースの HDInsight クラスターを Solr でカスタマイズする方法と、Solr を使用してデータを検索する方法について説明します。 Linux ベースのクラスターに Solr を使用する方法の詳細については、次を参照してください。 [のインストールおよび使用 Solr で HDinsight Hadoop クラスター (Linux)](hdinsight-hadoop-solr-install-linux.md)します。
 
使用して Azure HDInsight のクラスター (Hadoop、Storm、HBase、Spark) の任意の型に Solr をインストールできる *Script Action*します。 HDInsight クラスターに Solr をインストールするサンプル スクリプトは読み取り専用の Azure ストレージ blob から入手 [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)します。 

サンプル スクリプトは、HDInsight クラスター version 3.1 でのみ機能します。 HDInsight クラスター バージョンの詳細については、次を参照してください。 [HDInsight クラスター バージョン](hdinsight-component-versioning.md)です。

このトピックで使用するサンプル スクリプトは、既定の構成で Windows ベースの Solr クラスターを作成します。 この構成とは異なるコレクションやシャード、スキーマ、レプリカなどで Solr クラスターを構成する場合には、それに応じてスクリプトと Solr バイナリを変更する必要があります。

**関連記事:**

- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install.md): Azure ポータルを使用して HDInsight クラスターに Solr をインストール
- [HDInsight Hadoop クラスターに Solr をインストールして使用する (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [HDInsight で Hadoop クラスターを作成](hdinsight-provision-clusters.md): HDInsight クラスターの作成に関する一般情報
- [Script Action を使って HDInsight クラスターのカスタマイズ][hdinsight-cluster-customize]: Script Action を使って HDInsight クラスターをカスタマイズする方法の一般情報
- [HDInsight 用の Script Action スクリプトの開発](hdinsight-hadoop-script-actions.md)

## Solr とは何か

[Apache Solr](http://lucene.apache.org/solr/features.html) は、データに対して強力なフルテキスト検索ができるエンタープライズ検索プラットフォームです。 Hadoop が大量のデータの保存と管理を可能にするのに対し、Apache Solr は迅速にデータを取得するための検索機能を提供します。    

## ポータルを使用して Solr をインストールする

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [HDInsight クラスターでの Solr のインストール](hdinsight-hadoop-solr-install.md)

1. 使用して、クラスターの作成を開始、 **カスタム作成** オプション」の説明に従って [作成の Hadoop クラスターの HDInsight](hdinsight-provision-clusters.md#portal)します。 
2.  **Script Action** ページ、ウィザードのをクリックして **[スクリプト アクションの追加** 次に示すように、スクリプト アクションの詳細を提供します。

    ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-solr-install-v1/hdi-script-action-solr.png "Use Script Action to customize a cluster")
    
    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>名前</td>
            <td>スクリプト アクションの名前を指定します。 たとえば、 <b>Solr のインストール</b>します。</td></tr>
        <tr><td>スクリプト URI</td>
            <td>クラスターをカスタマイズするために呼び出すスクリプトの Uniform Resource Identifier (URI) を指定します。 たとえば、 <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>ノードの種類</td>
            <td>カスタマイズ スクリプトが実行されるノードを指定します。 選択する <b>のすべてのノード</b>, 、<b>ヘッド ノードのみ</b>, 、または <b>ワーカー ノードのみ</b>します。
        <tr><td>パラメーター</td>
            <td>スクリプトで必要な場合は、パラメーターを指定します。 Solr をインストールするスクリプトにパラメーターは必要ないため、この部分は空白のままにすることもできます。</td></tr>
    </table>    

    複数のスクリプト操作を追加して、クラスターに複数のコンポーネントをインストールすることができます。 スクリプトの追加後、チェックマークをクリックしてクラスターの作成を開始します。

Azure PowerShell や HDInsight .NET SDK を使用して、HDInsight に Solr をインストールするためにスクリプトを使用することもできます。 これらの手順については、このトピックの後半で説明します。

## Solr を使用する

最初に、いくつかのデータ ファイルに対し Solr のインデックスを作成する必要があります。 これにより、インデックス付きデータに対して、Solr を使用して検索クエリを実行できます。 HDInsight クラスターで Solr を使用するには、次の手順を実行します。

1. **使用してリモート デスクトップ プロトコル (RDP) リモート HDInsight クラスターに Solr をインストールした**します。 Solr のインストールによって作成したクラスターに対し、Azure ポータルでリモート デスクトップを有効にし、クラスターにリモート接続します。 手順については、「[RDP を使用した HDInsight クラスターへの接続](hdinsight-administer-use-management-portal.md#rdp)」をご覧ください。

2. **データ ファイルをアップロードすることによってインデックス Solr**します。 Solr のインデックスを作成する場合、検索が必要なドキュメントをその中に配置します。 Solr のインデックスをリモート クラスターに RDP を使用して、デスクトップに移動し、Hadoop コマンドラインを開きますおよびに移動する **C:\apps\dist\solr-4.7.2\example\exampledocs**します。 次のコマンドを実行します。 
    
        java -jar post.jar solr.xml monitor.xml

    コンソールで、次の出力が表示します。

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Post.jar ユーティリティ 2 つのサンプル ドキュメントで Solr のインデックス **solr.xml** と **monitor.xml**します。 post.jar ユーティリティとサンプル ドキュメントは Solr のインストールで利用できるようになります。

3. **Solr のダッシュ ボードを使用して、インデックス付きドキュメント内で検索する**です。 HDInsight クラスターに RDP セッションで Internet Explorer を開きで Solr のダッシュ ボードを起動 **http://headnodehost:8983/#//**します。 左側のペインから、 **Core Selector** ドロップダウン リストで、[ **collection1**, 、その] をクリック **クエリ**します。 例として、Solr 内のすべてのドキュメントを選択して返すために、次の値を指定します。
    1.  **Q** テキスト ボックスに、入力 **\ *:**\ * します。 これにより、Solr でインデックス付けされたすべてのドキュメントが返されます。 ドキュメント内の特定の文字列を検索する場合には、ここにその文字列を入力することができます。
    2.  **Wt** テキスト ボックスに、出力形式を選択します。 既定値は **json**します。 クリックして **クエリを実行**します。

        ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-solr-install-v1/hdi-solr-dashboard-query.png "Run a query on Solr dashboard")
    
    これによる出力は、Solr のインデックス作成のために使用した 2 つのドキュメントを返します。 出力結果は、以下のようになります。

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }
   

4. **お勧めします。 Solr から HDInsight クラスターに関連付けられている Azure Blob ストレージにインデックス付きデータをバックアップ**します。 インデックス付きデータを Solr のクラスター ノードから Azure BLOB ストレージ にバックアップすることをお勧めします。 そのために、次の手順を実行してください。

    1. RDP セッションから Internet Explorer を開き、次の URL をポイントします。

            http://localhost:8983/solr/replication?command=backup

        次のように、応答が表示されます。

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. リモート セッションで、{{SOLR_HOME}\{Collection}\data に移動します。 このサンプル スクリプトを通じて作成したクラスターにする必要があります **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**します。 この場所には、ような名前で作成されたスナップショット フォルダーを表示する必要があります **スナップショット*。タイムスタンプ** *。
    
    3. スナップショット フォルダーを zip 圧縮して Azure BLOB ストレージにアップロードします。 Hadoop コマンド ラインで、次のコマンドを使用して、スナップショット フォルダーの場所に移動します。

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        このコマンドは、クラスターに関連付けられている既定のストレージ アカウント内のコンテナーの下にある /example/data/ にスナップショットをコピーします。

## Aure PowerShell を使用して Solr をインストールする

参照してください [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell)します。  このサンプルでは、Azure PowerShell を使用して Spark をインストールする方法を示します。 使用するスクリプトをカスタマイズする必要があります。 [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)します。

## .NET SDK を使用して Solr をインストールする

参照してください [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell)します。 このサンプルでは、.NET SDK を使用して Spark をインストールする方法を示します。 使用するスクリプトをカスタマイズする必要があります。 [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)します。

## 関連項目

- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install.md): Azure ポータルを使用して HDInsight クラスターに Solr をインストール
- [HDInsight Hadoop クラスターに Solr をインストールして使用する (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [HDInsight で Hadoop クラスターを作成](hdinsight-provision-clusters.md): HDInsight クラスターの作成に関する一般情報
- [Script Action を使って HDInsight クラスターのカスタマイズ][hdinsight-cluster-customize]: Script Action を使って HDInsight クラスターをカスタマイズする方法の一般情報
- [HDInsight 用の Script Action スクリプトの開発](hdinsight-hadoop-script-actions.md)
- [インストールして HDInsight クラスターで Spark を使用する][hdinsight-install-spark]: Spark のインストールに関するスクリプト アクションのサンプル
- [HDInsight クラスターで R をインストール][hdinsight-install-r]: スクリプト アクションのサンプル R をインストールする方法
- [HDInsight クラスターに Giraph をインストール](hdinsight-hadoop-giraph-install.md): Giraph のインストールに関するスクリプト アクションのサンプル

[powershell-install-configure]: ../install-configure-powershell.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
 


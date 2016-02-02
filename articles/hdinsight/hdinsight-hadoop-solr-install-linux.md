<properties
    pageTitle="Script Action を使用した Linux ベースの HDInsight での Solr のインストール | Microsoft Azure"
    description="Script Action を使用して Linux ベースの HDInsight Hadoop クラスターに Solr をインストールする方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="larryfr"/>


# HDInsight Hadoop クラスターに Solr をインストールして使用する

このトピックでは、スクリプト アクションを使用して Azure HDInsight に Solr をインストールする方法について説明します。 Solr は強力な検索プラットフォームで、Hadoop が管理するデータに対してエンタープライズ レベルの検索機能を提供します。 HDInsight クラスターに Solr をインストールした後で、Solr を使用したデータの検索方法についても説明します。
> [AZURE.NOTE] このドキュメントの手順では、Linux ベースの HDInsight クラスターが必要です。 Windows ベースのクラスターに Solr を使用する方法の詳細については、を参照してください [のインストールおよび使用 Solr で HDinsight Hadoop クラスター (Windows)](hdinsight-hadoop-solr-install.md)。

このトピックで使用するサンプル スクリプトは、既定の構成で Solr クラスターを作成します。 この構成とは異なるコレクションやシャード、スキーマ、レプリカなどで Solr クラスターを構成する場合には、それに応じてスクリプトと Solr バイナリを変更する必要があります。

## <a name="whatis"></a>Solr とは何ですか。

[Apache Solr](http://lucene.apache.org/solr/features.html) がデータに強力なフルテキスト検索ができるエンタープライズ検索プラットフォームです。 Hadoop が大量のデータの保存と管理を可能にするのに対し、Apache Solr は迅速にデータを取得するための検索機能を提供します。 このトピックでは、HDInsight クラスターをカスタマイズして Solr をインストールする方法を説明します。

## スクリプトの機能

このスクリプトは、HDInsight クラスターに次のような変更を加えます。

* Solr をインストール `/usr/hdp/current/solr`
* Solr サービスの実行に使用される新しいユーザー、__solrusr__ を作成します。
* セット __solruser__ の所有者として `/usr/hdp/current/solr`
* 追加、 [新進気鋭](http://upstart.ubuntu.com/) クラスター ノードが再起動した場合に、Solr を起動構成します。 Solr は、インストール後にもクラスター ノード上で自動的に開始されます。

## <a name="install"></a>スクリプト アクションを使用して Solr をインストールします。

HDInsight クラスターに Solr をインストールするサンプル スクリプトは読み取り専用の Azure ストレージ blob から入手 [https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh)します。 このセクションでは、Azure ポータルを使用してクラスターをプロビジョニングする際に、サンプル スクリプトを使用する方法について説明します。
> [AZURE.NOTE] Azure PowerShell または HDInsight .NET SDK を使用し、このスクリプトを使用してクラスターを作成することもできます。 これらのメソッドの使用に関する詳細については、次を参照してください。 [Script Action を使用する HDInsight のカスタマイズ クラスタ](hdinsight-hadoop-customize-cluster-linux.md)します。

1. 」の手順を使用してプロビジョニングを開始 [プロビジョニング Linux ベースの HDInsight クラスター](hdinsight-provision-linux-clusters.md#portal), が、プロビジョニングを完了できません。

2. **[オプションの構成]** ブレードで **[スクリプト アクション]** を選択し、以下の情報を指定します。

    * __[名前]__: スクリプト アクションの表示名を入力します。
    * __スクリプト URI__: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
    * __[ヘッド]__: このオプションをオンにします。
    * __[ワーカー]__: このオプションをオンにします。
    * __[Zookeeper]__: Zookeeper ノードにインストールするには、このオプションをオンにします。
    * __[パラメーター]__: このフィールドは空のままにします。

3. **[スクリプト アクション]** の下部で、**[選択]** を使用して構成を保存します。 最後に、**[オプションの構成]** ブレードの下部にある **[選択]** を使用して、オプションの構成情報を保存します。

4. 」の説明に従って、クラスターのプロビジョニングを引き続き [プロビジョニング Linux ベースの HDInsight クラスター](hdinsight-hadoop-create-linux-clusters-portal.md)します。

## <a name="usesolr"></a>HDInsight で Solr を使用する方法

### データのインデックス作成

最初に、いくつかのデータ ファイルに対し Solr のインデックスを作成する必要があります。 これにより、インデックス付きデータに対して、Solr を使用して検索クエリを実行できます。 以下の手順を使用して Solr にいくつかのサンプル データを追加し、そのデータを照会します。

1. SSH を使用して HDInsight クラスターに接続します。

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    HDInsight での SSH の使用方法の詳細については、以下の記事を参照してください。

    * [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Windows から Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-windows.md)
    > [AZURE.IMPORTANT] このドキュメントの後半の手順で、SSL トンネルを使用して Solr Web UI に接続します。 これらの手順を使用するには、SSL トンネルを確立し、それをブラウザーで使用するように構成する必要があります。
    >
    > 詳細については、を参照してください [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)。

2. 次のコマンドを使用して、Solr のサンプル データのインデックスを作成します。

        cd /usr/hdp/current/solr/example/exampledocs
        java -jar post.jar solr.xml monitor.xml

    コンソールで、次の出力が表示します。

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    post.jar ユーティリティは、**solr.xml** と **monitor.xml** という 2 つのサンプル ドキュメントで Solr のインデックスを作成します。 これらは、Solr 内の __collection1__ に格納されます。

3. 次のコマンドを使用して、Solr によって公開される REST API を照会します。

        curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"

    これに対してクエリを発行します __collection1__ に一致するすべてのドキュメントの __\ *: \ *__ (としてエンコード \*%3A\* クエリ文字列に)、応答を JSON として返されるとします。 応答は次のように表示されます。

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


### Solr ダッシュボードの使用

Solr ダッシュボードは、Web ブラウザーを介して Solr を操作できる Web UI です。 Solr ダッシュボードは HDInsight クラスターからインターネットに直接公開されませんが、SSH トンネルを使用してアクセスする必要があります。 SSH トンネルの使用に関する詳細については、を参照してください [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)。

SSH トンネルを確立したら、Solr ダッシュボードを使用するために次の手順を実行します。

1. ヘッド ノードのホスト名を決定します。

    1. ブラウザーでは、https://CLUSTERNAME.azurehdinsight.net に移動します。 ダイアログ ボックスが表示されたら、認証に管理者のユーザー名とパスワードを使用してサイトに入ります。

    2. ページ上部のメニューから、__[ホスト]__ を選択します。

    3. 「__hn0__」で始まるエントリを選択します。 ページが開くと、一番上にホスト名が表示されます。 ホスト名の形式は「__hn0-PARTOFCLUSTERNAME.randomcharacters.cx.internal.cloudapp.net__」です。 これは Solr ダッシュボードに接続するときに使用するホスト名です。

1. 接続するブラウザーで、 __http://HOSTNAME:8983/#//__, ここで、 __ホスト名__ 前の手順で確認した名前を指定します。

    この要求は、SSH トンネルを介して HDInsight クラスターのヘッド ノードにルーティングする必要があります。 次のようなページが表示されます。

    ![Solr ダッシュボードのイメージ](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

2. 左側のウィンドウで、**[Core Selector]** ボックスを使用して **[collection1]** を選択します。 いくつかのエントリが __[collection1]__ の下に表示されます。

3. __[collection1]__ の下に表示されたエントリから、__[クエリ]__ を選択します。 次の値を使用して、検索ページに値を入力します。

    * **Q** テキスト ボックスに、入力 **\ *:**\ * します。 これにより、Solr でインデックス付けされたすべてのドキュメントが返されます。 ドキュメント内の特定の文字列を検索する場合には、ここにその文字列を入力することができます。

    * **[wt]** ボックスでは、出力形式を選択します。 既定値は、**json** です。

    最後に、検索ページの下部にある **[クエリの実行]** をクリックします。

    ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

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


### Solr の開始と停止

Solar を手動で停止または開始する必要がある場合は、次のコマンドを使用します。

    sudo stop solr
    
    sudo start solr

## インデックス付きデータのバックアップ

インデックス付きデータを Solr のクラスター ノードから Azure BLOB ストレージ にバックアップすることをお勧めします。 そのために、次の手順を実行してください。

1. SSH を使用してクラスターに接続した後、次のコマンドを使用して、ヘッド ノードのホスト名を取得します。

        hostname -f

2. 次を使用し、インデックス付きデータのスナップショットを作成します。 __HOSTNAME__ を前のコマンドで返された名前に変えます。

        curl http://HOSTNAME:8983/solr/replication?command=backup

    次のように、応答が表示されます。

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

2. 次に、__/usr/hdp/current/solr/example/solr__ ディレクトリに移動します。 このディレクトリには、コレクションごとにサブディレクトリがあります。 各コレクションのディレクトリには __data__ ディレクトリがあり、そのコレクションのスナップショットが格納されています。

    たとえば、これまでに説明した手順を使用してサンプル ドキュメントにインデックスを作成した場合、__/usr/hdp/current/solr/example/solr/collection1/data__ ディレクトリには __snapshot.###########__ という名前のディレクトリが含まれています。# はスナップショットの日時です。

3. 次のようなコマンドを使用して、スナップショット フォルダーの圧縮アーカイブを作成します。

        tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855

    これにより、__snapshot.20150806185338855.tgz__ という名前の新しいアーカイブが作成されます。このアーカイブには、__snapshot.20150806185338855__ ディレクトリの内容が含まれています。

3. 次のコマンドを使用して、アーカイブをクラスターのプライマリ ストレージに格納できます。

    hadoop fs -copyFromLocal snapshot.20150806185338855.tgz /example/data
    > [AZURE.NOTE] Solr スナップショットを格納するための専用ディレクトリを作成することもできます。 たとえば、 `hadoop fs/wordcount/apps/solrbackup`します。

Solr のバックアップと復元の操作の詳細については、次を参照してください。 [の作成および SolrCores のバックアップを復元](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups+of+SolrCores)します。


## 関連項目

- [HDInsight での色合いがクラスターのインストールおよび使用](hdinsight-hadoop-hue-linux.md)します。 Hue は、Pig ジョブや Hive ジョブの作成、実行、保存のほか、HDInsight クラスターの既定のストレージの参照を容易にする Web UI です。

- [をインストールし、][hdinsight-install-spark]します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Spark をインストールします。 Spark はビッグ データ分析アプリケーションのパフォーマンスを向上するメモリ内の処理をサポートする、オープン ソースの並列処理のフレームワークです。

- [インストール HDInsight に R をクラスターの ][hdinsight-install-r]します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに R をインストールします。 R は、統計計算用のオープン ソースの言語および環境です。 R は、数百の組み込み統計関数と、関数型プログラミングとオブジェクト指向のプログラミングの特徴を結合した独自のプログラミング言語を提供します。 また、広範なグラフィカル機能も提供します。

- [HDInsight クラスターでの Giraph のインストール](hdinsight-hadoop-giraph-install-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Giraph をインストールします。 Giraph は、Hadoop でグラフの処理を実行するために使用でき、Azure HDInsight で使用できます。

- [HDInsight クラスターでのインストールの色合い](hdinsight-hadoop-hue-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Hue をインストールします。 Hue は Hadoop クラスターとの情報のやりとりに使用される一連の Web アプリケーションです。




[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md 
[hdinsight-install-spark]: hdinsight-hadoop-spark-install-linux.md 
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md 


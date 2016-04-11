<properties
   pageTitle="HDInsight での実行を高速化するための Hive クエリの最適化 | Microsoft Azure"
   description="HDInsight で Hive クエリを最適化する方法について説明します。"
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# HDInsight の Hadoop に対する Hive クエリの最適化

既定では、Hadoop クラスターのパフォーマンスは最適化されていません。 この記事では、クエリに適用できる最も一般的な Hive パフォーマンス最適化の方法について説明します。


[AZURE.INCLUDE [preview-portal](../../includes/hdinsight-azure-preview-portal.md)] 


* [HDInsight の Hadoop の Hive クエリの最適化](hdinsight-hadoop-optimize-hive-query-v1.md)します。

##ワーカー ノードのスケール アウト

クラスター内のノードのワーカーの数を増やすことで、より多くの mapper と reducer を同時に実行できるようになります。 HDInsight でのスケール アウトを向上させる方法が 2 つあります。

- プロビジョニング時に、Azure ポータル、Azure PowerShell またはクロス プラットフォームのコマンド ライン インターフェイスを使用してワーカー ノードの数を指定できます。  詳細については、次を参照してください。 [HDInsight クラスターのプロビジョニング](hdinsight-provision-clusters.md)します。 次の画面は、Azure ポータル上に表示されたワーカー ノード構成を示しています。

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- 実行時に、クラスターを再作成せずにスケールアウトすることもできます。 一般的なイメージを以下に示します。
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

HDInsight でサポートされているさまざまな仮想マシンの詳細については、次を参照してください。 [HDInsight 料金](http://azure.microsoft.com/pricing/details/hdinsight/)します。

##Tez を有効にする

[Apache Tez](http://hortonworks.com/hadoop/tez/) は、MapReduce エンジンに代わる実行エンジンです。

![tez_1][image-hdi-optimize-hive-tez_1]


Tez はより高速です。それは次の理由によります。

- MapReduce エンジンでは、無閉路有効グラフ (DAG) を  1 つのジョブとして実行します。表現される DAG では、マッパーの各セットの後に 1 セットの reducer が続く必要があります。 これにより、複数の MapReduce ジョブが各 Hive クエリでスピンオフされます。 Tez にはこのような制約はありません。複雑な DAG を 1 つのジョブとして処理することができるため、ジョブのスタートアップのオーバーヘッドが最小限に抑えられます。
- **不要な書き込みを回避できます** 複数のジョブがスピンオフされるため、MapReduce エンジンでは同じ Hive クエリを各ジョブの出力は、HDFS に書き込まれます中級者向けデータのです。 Tez は各 Hive クエリのジョブの数を最小限に抑えるので、不要な書き込みを回避することができます。
- **起動時の遅延を最小限に抑えられます** Tez は開始に必要な mapper の数を減らすと、全体的な最適化の向上も期待して起動時の遅延を最小限に抑えることが向上します。
- **コンテナーを再利用** するたびに可能な Tez はコンテナーの起動による遅延が減るようにコンテナーを再利用することです。
- **継続的な最適化手法** 従来の最適化はコンパイル フェーズ中に行わします。 しかし最適化を向上させるための入力に関する詳細は、実行時に入手できます。 Tez は、実行時フェーズでプランをさらに最適化するための継続的な最適化手法を使用します。

これらの概念の詳細については、クリックして [ここで](http://hortonworks.com/hadoop/tez/)

次の設定でクエリにプレフィックスを付けることで、Hive クエリで Tez を有効にできます。

    set hive.execution.engine=tez;

Windows ベースの HDInsight クラスターの場合は、プロビジョニング時に Tez を有効にする必要があります。 Tez が有効になっている Hadoop クラスターのプロビジョニング用のサンプル Azure  PowerShell スクリプトを次に示します。


    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux ベースの HDInsight クラスターがある Tez を既定で有効にします。
    

## Hive パーティション分割

I/O 操作は、Hive クエリを実行するための主なパフォーマンスのボトルネックです。 読み取る必要があるデータの量を削減することで、パフォーマンスを向上することができます。 既定では、Hive クエリは、全 Hive テーブルをスキャンします。 これはテーブルのスキャンのようなクエリでは有効な方法ですが、少量のデータのみのスキャンが必要なクエリ (例えば、フィルターを使用するクエリ) では、不要なオーバーヘッドが生じてしまいます。 Hive パーティション分割では、Hive クエリは Hive テーブルの必要な量のデータだけにアクセスします。

Hive パーティション分割は、生データを新しいディレクトリに再構成することにより実装されます。その際、各パーティションに独自のディレクトリを用意します (パーティションは、ユーザーによって定義されます)。 次の図は、Hive テーブルをパーティション分割列によって *年*します。 年ごとに新しいディレクトリが作成されます。

![パーティション分割][image-hdi-optimize-hive-partitioning_1]

パーティション分割に関するいくつかの考慮事項:

- **過小パーティションではなく** のいくつかの値を持つ列のパーティション分割と、非常にいくつかのパーティションが発生することができます。 たとえば、性別に基づいてパーティションを分割する場合、2 つのパーティション (男性と女性) しか作成されません。したがって、待ち時間の短縮は、最大でも半分にしかなりません。

- **過剰なパーティション分割を行わず** - その他の極端、クラスター namenode にストレスの多くの原因を与えるためのディレクトリの消費量を処理する複数のパーティションが作成され、一意の値 (たとえば userid) の列にパーティションを作成します。

- **データ スキューを回避** -すべてのパーティションが偶数のサイズになるため、賢いパーティショニング キーを選択します。 例のパーティションを *状態* に約 30、カリフォルニアの下のレコードの数が発生する可能性があります倍であるバーモント人口の相違のためです。

パーティション テーブルを作成するには、使用、 *Partitioned By* 句。

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

パーティション テーブルが作成されれば、静的パーティションまたは動的パーティションのいずれかを作成できるようになります。

- **静的パーティション分割** ことを示し、適切なディレクトリに既にシャード化されたデータがある手動でディレクトリの場所に基づいて Hive パーティションを問い合わせることができます。 これを次のコード スニペットに示します。

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **動的なパーティション分割** を自動的にパーティションを作成する Hive をすることを意味します。 ステージング テーブルからパーティショニング テーブルが作成されているので、後は、次のようにパーティション テーブルにデータを挿入するだけです。

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

詳細については、次を参照してください。 [テーブルのパーティション分割されている](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables)します。

##ORCFile 形式の使用

Hive は、さまざまなファイル形式をサポートしています。 次に例を示します。

- **テキスト**: これは既定のファイル形式であり、ほとんどのシナリオで使用できます
- **Avro**: 相互運用性シナリオに適して
- **Orc/parquet**: パフォーマンスに最も適しています

ORC (最適化行多桁式) 形式は、Hive データを格納する非常に効率的な方法です。 他の形式と比べて、ORC には次の利点があります。

- 複合型 (DateTime、複合構造化型、および半構造化型を含む) のサポート
- 最大で 70% の圧縮
- 10,000 行ごとのインデックス (これにより行のスキップが可能)
- 実行時の実行の大幅な削除

ORC 形式を有効にするには、まずテーブルを作成する句を使用して *ORC として保存されている*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

次に、データをステージング テーブルから ORC テーブルに挿入します。 次に例を示します。

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

詳細 ORC 形式について [ここ](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC)します。

##ベクター化

ベクター化により、Hive は、一度に 1 行を処理する代わりに、1024 行を一括処理することができます。 これは、単純な操作がより迅速に行われることを意味します。実行に必要な内部コードが少なくなるためです。

Hive クエリのベクター化プレフィックスを有効にするには、次の設定を使用します。

    set hive.vectorized.execution.enabled = true;

詳細については、次を参照してください。 [クエリの実行をベクター化](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution)します。


##その他の最適化の方法

その他にも考慮できる最適化の方法がいくつかあります。たとえば、次のような方法です。

- **Hive のバケット:** 大規模なクラスター化またはセグメント化するための手法ですがデータのクエリのパフォーマンスを最適化するために設定します。
- **結合の最適化:** の Hive のクエリの実行が結合の効率を向上させユーザー ヒントの必要性を軽減計画を最適化します。 詳細については、次を参照してください。 [結合の最適化](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization)します。
- **Reducer の増加**

##<a id="nextsteps"></a> 次のステップ
この記事ではいくつかの一般的な Hive クエリの最適化方法を説明しました。 詳細については、次の記事を参照してください。

- [HDInsight での Apache Hive の使用](../hdinsight-using-hive.md)
- [HDInsight での Hive を使用したフライト遅延データの分析](hdinsight-analyze-flight-delay-data.md)
- [HDInsight での Hive を使用した Twitter データの分析](hdinsight-analyze-twitter-data.md)
- [HDInsight の Hadoop で Hive クエリ コンソールを使用したセンサー データの分析](hdinsight-hive-analyze-sensor-data.md)
- [Web サイトのログを分析するための HDInsight での Hive の使用](../hdinsight-hive-analyze-web-site-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png


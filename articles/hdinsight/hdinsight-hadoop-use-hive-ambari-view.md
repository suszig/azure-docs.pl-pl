<properties
   pageTitle="Ambari ビューによる HDInsight (Hadoop) での Hive の使用 | Microsoft Azure"
   description="Web ブラウザーから Hive ビューを使用して Hive クエリを送信する方法について説明します。 Hive ビューは、Linux ベースの HDInsight クラスターに付属する Ambari Web UI の要素です。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/03/2015"
   ms.author="larryfr"/>

#HDInsight での Hive View と Hadoop の使用

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ambari は、Linux ベースの HDInsight クラスターに付属する管理および監視ユーティリティです。 Ambari が提供する機能の 1 つに、Hive クエリを実行するときに使用される Web UI があります。 これは、 __Hive ビュー__, 、HDInsight クラスターに用意されている Ambari ビューの一部です。

> [AZURE.NOTE] Ambari には、このドキュメントでは説明を省略できる機能の多くがあります。 詳細については、次を参照してください。 [Ambari Web UI を使用して、クラスターの HDInsight の管理](hdinsight-hadoop-manage-ambari.md)します。

##前提条件

- Linux ベースの HDInsight クラスター。 新しいクラスターを作成する方法の詳細については、次を参照してください。 [Linux ベースの HDInsight の概要](hdinsight-hadoop-linux-tutorial-get-started.md)します。

##Hive ビューを開く

Azure のポータルから Ambari ビューを作成することができます。HDInsight クラスターを選択し、[ __Ambari ビュー__ から、 __クイック リンク__ セクションです。

![quick links section](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Web ブラウザーで https://CLUSTERNAME.azurehdinsight.net に移動して Ambari に直接移動することもできます (ここで __CLUSTERNAME__ 、HDInsight クラスターの名前を指定) し、ページのメニューから四角形のセットを選択 (] の横に、 __Admin__ リンクと、ページの左側のボタン) をリストの使用可能なビューにします。 選択、 __ビュー [ハイブの__です。

![Selecting ambari views](./media/hdinsight-hadoop-use-hive-ambari-view/selecthiveview.png).

> [AZURE.NOTE] Ambari にアクセスするときは、サイトに対する認証するように求められます。 クラスターを作成するときに使用した管理者アカウント名 (既定値は `admin`) とパスワードを入力します。

次のようなページが表示されます。

![Image of the hive view page, containing a query editor section](./media/hdinsight-hadoop-use-hive-ambari-view/hiveview.png)

##テーブルを表示する

 __データベース エクスプ ローラー__ 、ページのセクションで、 __既定__ のエントリ、 __データベース__ ] タブをクリックします。 これにより、既定のデータベース内にあるテーブルの一覧が表示されます。 新しい HDInsight クラスターの 1 つのテーブルが存在する必要があります。 __hivesampletable__します。

![database explorer with the default database expanded](./media/hdinsight-hadoop-use-hive-ambari-view/databaseexplorer.png)

このドキュメントの手順に従って新しいテーブルを追加した際、データベース エクスプローラーの右上隅にある更新アイコンを使用すると、利用可能なテーブルの一覧を更新できます。

##<a name="hivequery"></a>クエリ エディター

クラスターに格納されているデータに対して Hive クエリを実行するには、Hive ビューで次の手順を実行します。

1.  __クエリ エディター__ セクション、ページの次の HiveQL ステートメントをワークシートに貼り付けます。

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;

    これらのステートメントは次のアクションを実行します。

    - **DROP TABLE** -テーブルが既に存在する場合は、テーブルとデータ ファイルを削除します。
    - **CREATE EXTERNAL TABLE** -Hive に新しく「外部」テーブルを作成します。 外部テーブルは、Hive にテーブル定義のみを格納し、データは、元の場所に残します。
    - **ROW FORMAT** -Hive にデータの形式です。 ここでは、各ログのフィールドは、スペースで区切られています。
    - **STORED AS TEXTFILE LOCATION** - Hive にデータを格納格納先 (example/data ディレクトリ) といるテキストとして格納されています。
    - **選択** の各行の t4 列に [ERROR] の値が含まれているすべての行の数を選択します。

    >[AZURE.NOTE] 、または別の MapReduce 操作によってデータの自動アップロード処理などの外部ソースによって更新する基になるデータが予想される場合、外部テーブルを使用する必要がありますが、常に最新のデータを使用するための Hive クエリをします。 外部テーブルを削除しても *いない* データ、テーブル定義のみを削除します。

2. 使用して、 __Execute__ クエリを開始するクエリ エディターの下部にあるボタンをクリックします。 オレンジ色を有効にする必要があり、テキストに変更 __の実行を停止__します。 A __クエリ プロセスの結果__ セクションのクエリ エディターの下に表示し、ジョブに関する情報を表示する必要があります。

    > [AZURE.IMPORTANT] ブラウザーによっては、ログまたは結果の情報を正しく更新しない場合があります。 ジョブを実行したときに、ジョブの実行が続いているにもかかわらずログが更新されたり結果が返されたりしない場合は、Mozilla FireFox または Google Chrome を代わりに使用してください。
    
3. クエリが完了した後、 __クエリ プロセスの結果__ セクションは、操作の結果を表示します。  __の実行を停止__ ボタンが緑色にも変更されます __Execute__ ] ボタンをクリックします。  __結果__ ] タブで、次の情報を含める必要があります。

        sev       cnt
        [ERROR]   3

     __ログ__ 、ジョブによって作成されたログ情報を表示] タブを使用できます。 ログ情報は、クエリに問題が発生した場合のトラブルシューティングに使用できます。
    
    > [AZURE.TIP] 注、 __結果を保存__ の上にあるドロップダウン ダイアログの左、 __クエリ プロセスの結果__ セクションです。 これを使用して、結果をダウンロードするか、CSV ファイルとして HDInsight のストレージに保存することができます。
    
3. このクエリの最初の 4 つの行を選択し、[ __Execute__します。 ジョブが完了したときに結果は生成されません。 これは、使用するため、 __Execute__ ボタン、クエリの一部を選択すると、選択したステートメントのみを実行します。 この場合は、テーブルの行を取得する最後のステートメントが選択範囲に含まれていませんでした。 その行だけを選択して使用する __Execute__, 、期待どおりの結果を表示する必要があります。

3. 使用して、 __新しいワークシート__ の下部にあるボタン、 __クエリ エディター__ 新しいワークシートを作成します。 新しいワークシートに、次の HiveQL ステートメントを入力します。

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

    これらのステートメントは次のアクションを実行します。

    - **テーブル IF NOT EXISTS の作成** -が既に存在しない場合は、テーブルを作成します。 以降、 **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。 外部テーブルとは異なり、内部テーブルを削除すると、基盤となるデータも削除されます。
    - **STORED AS ORC** -Optimized Row Columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
    - **INSERT OVERWRITE ...選択** の行を選択、 **log4jLogs** [ERROR] を含むとにデータを挿入するテーブル、 **errorLogs** テーブルです。
    
    使用して、 __Execute__ このクエリを実行するボタンをクリックします。  __結果__ このクエリによって行が返されませんが、として状態が [は、タブに情報は含まれない __SUCCEEDED__します。

###Hive の設定

選択、 __設定__ エディターの右側にあるアイコン。

![icons](./media/hdinsight-hadoop-use-hive-ambari-view/settings.png)

Settings では、Hive の実行エンジンを Tez (既定値) から MapReduce に変更するなど、さまざまな Hive 設定を変更できます。

###ビジュアルの説明

選択、 __Visual 説明__ エディターの右側にあるアイコン。

![icons](./media/hdinsight-hadoop-use-hive-ambari-view/visualexplainicon.png)
    
これは、 __Visual 説明__ 複雑なクエリのフローを理解するうえで役に立ちますクエリのビューです。 このビューに相当するテキストを表示するにを使用して、 __説明__ クエリ エディターでのボタンをクリックします。

![visual explain image](./media/hdinsight-hadoop-use-hive-ambari-view/visualexplain.png)

###Tez

選択、 __Visual 説明__ エディターの右側にあるアイコン。

![icons](./media/hdinsight-hadoop-use-hive-ambari-view/tez.png)

このクエリのために Tez で使用される有向非巡回グラフ (DAG) が表示されます (利用できる場合)。 過去に実行したクエリ、ビュー、DAG にする場合は、使用、 __Tez ビュー__ 代わりにします。

###通知

選択、 __Visual 説明__ エディターの右側にあるアイコン。

![icons](./media/hdinsight-hadoop-use-hive-ambari-view/notifications.png)

通知とは、クエリを実行するときに生成されるメッセージです。 たとえば、クエリを送信したときや、エラーが発生したときに、通知が届きます。

##保存済みのクエリ

1. クエリ エディターから、新しいワークシートを作成し、次のクエリを入力します。

        SELECT * from errorLogs;
    
    クエリを実行して、動作することを確認します。 結果は次のようになります。

        errorlogs.t1    errorlogs.t2    errorlogs.t3    errorlogs.t4    errorlogs.t5    errorlogs.t6    errorlogs.t7
        2012-02-03  18:35:34    SampleClass0    [ERROR]     incorrect   id  
        2012-02-03  18:55:54    SampleClass1    [ERROR]     incorrect   id  
        2012-02-03  19:25:27    SampleClass4    [ERROR]     incorrect   id

2. 使用して、 __名前を付けて__ エディターの下部にあるボタンをクリックします。 このクエリに名前 __Errorlogs__ 選択 __OK__します。 ワークシートの名前を変更する __Errorlogs__します。
    
3. 選択、 __保存されているクエリ__ ハイブの表示] ページの上部にあるタブをクリックします。 なお __Errorlogs__ 保存されたクエリとして表示されます。 クエリは、削除されるまでこの一覧に表示されます。 名前を選択すると、クエリ エディターにクエリが表示されます。

##クエリの履歴

 __履歴__ ハイブのビューの上部にあるボタンでは、以前に実行したクエリを表示することができます。 これを使用して、以前に実行したクエリの一部を選択できます。 クエリを選択すると、そのクエリがクエリ エディターに表示されます。

##ユーザー定義関数 (UDF)

Hive では拡張も **ユーザー定義関数 (UDF)**します。 UDF では、HiveQL で簡単にモデル化されない機能やロジックを実装することができます。 

[ハイブのビューの上部にある UDF] タブで宣言およびと併用して Udf のセットを保存することは HiveQL ステートメントの一部として、UDF を追加するには、クエリで、 __クエリ エディター__します。

Hive のビューに UDF を追加した後、 __udf を挿入__ の下部にあるボタンは表示、 __クエリ エディター__します。 このボタンを選択すると、Hive ビューで定義した UDF のドロップダウン リストが表示されます。 UDF を選択すると、HiveQL ステートメントがクエリに追加され、UDF が有効になります。

たとえば、次のプロパティを持つ UDF を定義したとします。

* リソース名: myudfs
* リソース パス: wasb:///myudfs.jar
* UDF 名: myawesomeudf
* UDF のクラス名: com.myudfs.Awesome
    
使用して、 __udf を挿入__ ボタンという名前のエントリが表示されます __myudfs__, と他のボックスの一覧の各 UDF は、そのリソースに対して定義されているためです。 この場合、 __myawesomeudf__します。 このエントリを選択すると、クエリの先頭に次の内容が追加されます。

    add jar wasb:///myudfs.jar;

    create temporary function myawesomeudf as 'com.myudfs.Awesome';

これにより、クエリでこの UDF を使用できます。 たとえば、「`SELECT myawesomeudf(name) FROM people;`」のように入力します。

HDInsight において Hive で UDF を使用する方法の詳細については、以下の記事を参照してください。

* [HDInsight における Python と Hive および Pig の使用](hdinsight-python.md)

* [HDInsight にカスタムの Hive UDF を追加する方法](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

##<a id="nextsteps"></a>次のステップ

HDInsight での Hive に関する全般的な情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)


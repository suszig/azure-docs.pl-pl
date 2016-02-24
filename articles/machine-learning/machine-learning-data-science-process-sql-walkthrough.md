<properties
    pageTitle="Cortana Analytics Process の活用: SQL Server を使用する | Microsoft Azure"
    description="Advanced Analytics Process and Technology の活用"  
    services="machine-learning"
    documentationCenter=""
    authors="msolhab"
    manager="paulettm"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2015" 
    ms.author="mohabib;fashah;bradsev"/>


# Cortana Analytics Process の活用: SQL Server を使用する

このチュートリアル, するチュートリアルで構築および公開されているデータセットを使用してモデルを展開する-- [NYC タクシー乗車](http://www.andresmh.com/nyctaxitrips/) データセット。 手順は、Cortana Analytics Process (CAP) ワークフローに従います。


## <a name="dataset"></a>NYC タクシー乗車データセットの説明

NYC タクシー乗車データは、約 20GB の圧縮された CSV ファイル (非圧縮では最大 48 GB) です。1 億 7300 万以上の個々の乗車と、各乗車に支払われた料金で構成されています。 各旅行レコードには、pickup (乗車) と drop-off (降車) の場所と時間、匿名化されたタクシー運転手の (運転) 免許番号、および medallion (タクシーの一意の ID) 番号が含まれています。 データには 2013 年のすべての乗車が含まれ、データは月ごとに次の 2 つのデータセットに用意されています。

1. 「trip_data」の CSV ファイルには、乗車の詳細 (乗客数、乗車地点、降車地点、乗車時間、乗車距離など) が含まれています。 いくつかのサンプル レコードを次に示します。

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 「trip_fare」の CSV ファイルには、各乗車に対して支払われた料金の詳細 (支払いの種類、料金、追加料金と税、チップ、道路などの通行料、および合計支払金額など) が含まれます。 いくつかのサンプル レコードを次に示します。

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

trip\_data と trip\_fare を結合するための一意のキーは、[medallion]、[hack\_licence] および [pickup\_datetime} の各フィールドで構成されています。

## <a name="mltasks"></a>予測タスクの例

基づく次の 3 つの予測問題について説明します *\_amount*, 、つまり。

1. 二項分類: チップがつまり乗車に対して支払われたかどうかを予測する *\_amount* 大きく $0 は、肯定的な例より、 *\_amount* $0 の場合は負の値の例です。

2. 多クラス分類: 乗車で支払われたチップの範囲を予測します。 分割、 *\_amount* を 5 つの箱つまりクラスにします。

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. 回帰タスク: 乗車で支払われたチップの金額を予測します。  


## <a name="setup"></a>設定 Azure データ サイエンス環境のセットアップの高度な分析

わかるように、 [環境の計画](machine-learning-data-science-plan-your-environment.md) ガイドでは、Azure で NYC タクシー乗車データセットを使用するいくつかのオプションがあります。

- Azure BLOB でデータを操作し、Azure Machine Learning でモデリングする
- データを SQL Server データベースに読み込み、Azure Machine Learning でモデリングする

このチュートリアルでは、SQL Server へのデータの並行一括インポート、データの探索、特徴エンジニアリング、および SQL Server Management Studio と IPython Notebook を使用したダウンサンプリングについて説明します。 [サンプル スクリプト](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) と [IPython notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) は GitHub で共有します。 Azure BLOB のデータを操作するサンプルの IPython Notebook は同じ場所で利用できます。

Azure のデータ サイエンス環境をセット アップするには、

1. [ストレージ アカウントの作成](../storage-create-storage-account.md)

2. [Azure ML ワークスペースを作成する](machine-learning-create-workspace.md)

3. [データ サイエンス仮想マシンをプロビジョニング](machine-learning-data-science-setup-sql-server-virtual-machine.md), 、IPython Notebook サーバーとして SQL Server とする機能はします。

    > [AZURE.NOTE] サンプル スクリプトと IPython notebook は、セットアップ プロセス中に、データ サイエンス仮想マシンにダウンロードされます。 仮想マシンのインストール後スクリプトが完了すると、サンプルは仮想マシンのドキュメント ライブラリに入ります。  
    > - サンプルのスクリプト: `C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - サンプルの IPython Notebooks: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > ここで、`<user_name>` は VM の Windows ログイン名です。 サンプル フォルダーは参照 **サンプル スクリプト** と **Sample IPython Notebooks**します。


データセットのサイズ、データ ソースの場所、および選択した Azure ターゲット環境に基づく、このシナリオはのような [シナリオ \#5: ローカル ファイルの大規模データセットは、Azure VM 内の SQL Server を対象](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb)します。

## <a name="getdata"></a>公開されているソースからデータを取得する

取得する、 [NYC タクシー乗車](http://www.andresmh.com/nyctaxitrips/) 公開されている場所からのデータセットを使用することがで説明したメソッドの [と Azure Blob ストレージの間でデータ移動](machine-learning-data-science-move-azure-blob.md) 、新しい仮想マシンにデータをコピーします。

AzCopy を使用してデータをコピーするには

1. 仮想マシン (VM) にログインする

2. VM のデータ ディスクで新しいディレクトリを作成します (注記: VM にデータ ディスクとして付属する一時ディスクは使用しないでください)。

3. コマンド プロンプト ウィンドウで、< path_to_data_folder > を (2) で作成したデータ フォルダーに置き換えて次の Azcopy コマンド ラインでを実行します。

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    AzCopy が完了すると、合計 24 個の圧縮された CSV ファイル (trip\_data に 12 個、trip\_fare に 12 個) がデータ フォルダーに存在するはずです。

4. ダウンロードしたファイルを解凍します。 圧縮されていないファイルが存在するフォルダーに注意してください。 このフォルダーは < path\_to\_data\_files\ > としてに参照されます。

## <a name="dbload"></a>SQL Server データベースにデータを一括インポートする

使用して読み込み/転送する大量のデータを SQL データベースおよび後続のクエリのパフォーマンスが向上する _パーティション テーブルとビュー_します。 このセクションでは」に記載の指示に従って [並行一括インポートを使用して SQL パーティション テーブル データ](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) 新しいデータベースを作成し、データをパーティション分割されたテーブルに並列的にロードします。

1. VM にログオンした状態で、開始 **SQL Server Management Studio**します。

2. Windows 認証を使用して接続します。

    ![SSMS 接続][12]

3. という名前のスクリプト ファイルを開く場合は、SQL Server 認証モードを変更して、SQL ログイン ユーザーの新規作成したがまだ、 **ある change \_auth.sql** で、 **サンプル スクリプト** フォルダーです。 既定のユーザー名とパスワードを変更します。 クリックして **!実行** スクリプトを実行するには、ツールバーのです。

    ![スクリプトの実行][13]

4. SQL Server の既定のデータベースとログ フォルダーを確認または変更し、新規作成したデータベースが確実にデータ ディスクに格納されるようにします。 データウェアハウス用に最適化された SQL Server VM イメージは、データとログ ディスクが事前構成されています。 VM にデータ ディスクが含まれていなかったため、VM のセットアップ プロセス中に新しい仮想ハードディスクを追加した場合は、既定のフォルダーを次のように変更します。

    - 左側のパネルで SQL Server 名を右クリックし、クリックして **プロパティ**します。

        ![SQL Server プロパティ][14]

    - 選択 **データベース設定** から、 **ページの選択** 左側の一覧です。

    - 確認または変更し、 **データベースの既定の場所** に、 **データ ディスク** 任意の場所。 この場所は、既定の位置設定で作成した場合に新しいデータベースが存在する場所です。

        ![SQL Database 既定値][15]  

5. 新しいデータベースと一連のパーティション分割されたテーブルを保持するファイル グループを作成するには、サンプル スクリプトを開きます **create\_db\_default.sql**します。 スクリプトでは、という名前の新しいデータベースを作成します。 **TaxiNYC** と 12 のファイルグループの既定のデータの場所。 各ファイルグループは、1 か月分の trip\_data と trip\_fare data を保持します。 必要な場合は、データベース名を変更します。 クリックして **!実行** 、スクリプトを実行します。

6. 次に、2 つのパーティション テーブルを作成します。1 つは trip\_data 用に、もう 1 つは trip\_fare 用です。 サンプル スクリプトを開きます **create\_partitioned\_table.sql**, は。

    - 月単位でデータを分割するパーティション関数を作成します。
    - 各月のデータを別のファイルグループにマップするパーティション スキームを作成します。
    - パーティション構成にマップされている 2 つのパーティション分割されたテーブルを作成: **nyctaxi \_trip** は trip \_data を保持し、 **nyctaxi \_fare** trip \_fare のデータを保持します。

    クリックして **!実行** にスクリプトを実行し、パーティション分割されたテーブルを作成します。

7.  **サンプル スクリプト** フォルダー、SQL Server テーブルへのデータの並行一括インポートを示すために用意された 2 つのサンプル PowerShell スクリプトがあります。

    - **bcp\_parallel\_generic.ps1** テーブルにデータを並行一括インポートする汎用スクリプトです。 このスクリプトを変更し、スクリプト内のコメント行に示されているとおりに入力変数とターゲット変数を設定します。
    - **bcp\_parallel\_nyctaxi.ps1** 汎用スクリプトの構成済みのバージョンは、両方のテーブル、NYC タクシー乗車データを読み込むに使用されることができます。  

8. 右クリックし、 **bcp\_parallel\_nyctaxi.ps1** スクリプト名およびクリック **編集** PowerShell で開きます。 事前設定された変数を確認し、選択したデータベース名、入力データ フォルダー、対象のログ フォルダー、およびサンプルのフォーマット ファイルへのパスに従って変更 **nyctaxi_trip.xml** と **nyctaxi\_fare.xml** (で提供される、 **サンプル スクリプト** フォルダー)。

    ![データの一括インポート][16]

    また、認証モードを選択することもできます。既定は Windows 認証です。 実行するには、ツールバーの緑色の矢印をクリックします。 スクリプトは、24 の一括インポート操作を並行して開始します。パーティション分割されたテーブルごとに 12 の操作を開始します。 上記でセットされた SQL Server の既定のデータ フォルダーを開いて、データ インポートの進行状況を監視できます。

9. PowerShell スクリプトは、開始時刻と終了時刻を報告します。 一括インポートがすべて完了すると、終了時刻が報告されます。 対象のログ フォルダーをチェックして、一括インポートが成功したこと、つまり、対象のログ フォルダーにエラー報告がされていないことを確認します。

10. これで、データベースは、探索、特徴エンジニア リング、および必要に応じてその他の操作を行える状態になりました。 」の手順に従ってテーブルがパーティション分割されるので、 **pickup \_datetime** フィールドが含まれるクエリ **pickup \_datetime** に条件、 **、** 句は、パーティション スキームからメリットです。

11.  **SQL Server Management Studio**, 、提供されているサンプル スクリプトを調べる **sample \_queries.sql**します。 サンプル クエリのいずれかを実行するには、クエリ行を強調表示し、をクリックして **!実行** ツールバーです。

12. NYC タクシー乗車データは、2 つの個別のテーブルに読み込まれます。 結合操作を向上させるため、テーブルのインデックスを作成することを強くお勧めします。 サンプル スクリプト **create\_partitioned\_index.sql** 複合結合キーにパーティション分割されたインデックスを作成 **medallion、hack \_license、および pickup \_datetime**します。

## <a name="dbexplore"></a>SQL Server でのデータの探索および特徴エンジニアリング

このセクションでは実行データの探索および特徴の生成で直接 SQL クエリを実行して、 **SQL Server Management Studio** 前に作成した SQL Server データベースを使用します。 という名前のサンプル スクリプト **sample \_queries.sql** で提供される、 **サンプル スクリプト** フォルダーです。 既定値と異なる場合は、データベース名を変更するスクリプトを変更します。 **TaxiNYC**します。

この演習では、以下のことを実行します。

- 接続 **SQL Server Management Studio** いずれかの Windows 認証を使用するか、SQL 認証と SQL ログイン名とパスワードを使用します。
- さまざまな期間で、いくつかのフィールドのデータの分布を探索する。
- [経度] フィールドと [緯度] フィールドのデータの品質を調査する。
- 基づくバイナリおよび多クラス分類ラベルを生成する、 **\_amount**します。
- 特徴を生成するとともに、乗車距離を計算または比較する。
- 2 つのテーブルを結合して、モデルのビルドに使用するランダム サンプルを抽出する。

Azure Machine Learning に進む準備ができれば、次のいずれかを実行できます。  

1. 展開し、データのサンプルと Azure Machine Learning で [リーダー] [リーダー] モジュールに直接クエリをコピー アンド ペーストする最終的な SQL クエリを保存または
2. 新しいデータベース テーブルに作成するモデルを使用し、Azure Machine Learning で [リーダー] [リーダー] モジュールで、新しいテーブルを使用する予定のサンプリングおよびエンジニア リング データを保持します。

このセクションでは、最終的なクエリを保存してから、データの抽出とサンプリングを実行します。 2 番目のメソッドの説明については、 [データの探索と特徴エンジニア リング IPython Notebook で](#ipnb) セクションです。

以前、並行一括インポートを使用してデータを入力したテーブルの行と列の数を簡単に確認するには、次を実行します。

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### 探索: medallion (タクシー番号) ごとの乗車回数の分布

この例では、指定した期間内で乗車回数が 100 を超える medallion (タクシー番号) を識別します。 パーティション スキームによって条件が設定されるので、クエリがパーティション分割されたテーブルへのアクセス有益でしょう **pickup \_datetime**します。 データセット全体に対するクエリの実行でも、パーティション テーブルまたはインデックス スキャンを活用できます。

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### 探索: medallion および hack_license ごとの乗車回数の分布

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### データ品質の評価: 経度と緯度が正しくないレコードを確認する

この例では、いずれかの経度または緯度のフィールドに無効な値 (ラジアン角は -90 ～ 90 の間である必要があります)、または座標 (0, 0) のいずれかが含まれているかどうかを調査します。

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### 探索: チップが支払われた乗車と支払われなかった乗車の分布

この例では、特定の期間中 (または、1 年間をカバーする場合はデータセット全体で) チップが支払われた乗車と支払われなかった乗車の数を確認します。 この分布は、後で二項分類のモデリングで使用する二項ラベルの分布を反映しています。

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### 探索: チップのクラス/範囲の分布

この例では、特定の期間中に (または、1 年間をカバーする場合はデータセット全体で) チップの範囲の分布を計算します。 これは、後で多クラス分類のモデリングに使用されるラベル クラスの分布です。

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### 探索: 乗車距離の計算と比較

この例では、pickup (乗車) と drop-off (降車) の経度と緯度を、SQL geography ポイントに変換し、SQL geography ポイントの差を使用して乗車距離を計算し、比較するためにランダムな結果のサンプルを返します。 この例では、前述したデータ品質評価のクエリを使用して、結果を有効な座標のみに限定します。

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### SQL クエリでの特徴エンジニアリング

ラベルの生成と geography 変換探索クエリは、カウントする部分を削除してラベルや特徴を生成することにも使用できます。 その他の特徴エンジニア リングの SQL の例が記載されて、 [データの探索と特徴エンジニア リング IPython Notebook で](#ipnb) セクションです。 SQL Server データベースのインスタンスで直接実行する SQL クエリを使用して、データセット全体または大規模なサブセットで特徴生成クエリを実行するとより効率的です。 クエリを実行することがあります **SQL Server Management Studio**, 、IPython Notebook、または任意の開発ツールまたは環境にローカルやリモート データベースにアクセスできます。

#### モデル作成用にデータを準備する

次のクエリの結合、 **nyctaxi \_trip** と **nyctaxi \_fare** テーブル、二項分類ラベルを生成 **tipped**, 、多クラス分類ラベル **[tip \_class]**, 、結合データセット全体から 1% のランダム サンプルを抽出します。 このクエリをコピーしてに直接貼り付け、 [Azure Machine Learning Studio](https://studio.azureml.net) [リーダー] [リーダー] モジュールの Azure の SQL Server データベース インスタンスから直接データを取り込めます。 このクエリは、座標が正しくないレコード (0, 0) を除外します。

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>IPython Notebook でのデータの探索および特徴エンジニアリング

このセクションでは、以前作成した SQL Server データベースに対して、Python と SQL クエリの両方のクエリを使用して
データの探索および特徴の生成を実行します。 という名前のサンプル IPython notebook **machine-Learning-data-science-process-sql-story.ipynb** で提供される、 **Sample IPython Notebooks** フォルダーです。 このノートブックはできるも [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)します。

ビッグ データを操作する場合に推奨される手順を次に示します。

- メモリ内のデータ フレームに、データの小さなサンプルを読み込みます。
- サンプリングされたデータを使用して、視覚化と探索を行います。
- サンプリングされたデータを使用して、特徴エンジニアリングの実験を行います。
- 大規模なデータの探索、データの操作、および特徴エンジニアリングの場合は、Python を使用して Azure VM の SQL Server データベースに対して直接 SQL クエリを実行します。
- Azure Machine Learning のモデルの構築に使用するサンプルのサイズを決定します。

Azure Machine Learning に進む準備ができたら、次のいずれかを実行します。  

1. 展開し、データのサンプルと Azure Machine Learning で [リーダー] [リーダー] モジュールに直接クエリをコピー アンド ペーストする最終的な SQL クエリを保存します。 この方法では説明して、 [Azure Machine Learning でモデルの作成](#mlmodel) セクションです。    
2. 新しいデータベースのテーブルを作成するモデルの使用を計画し、[リーダー] [リーダー] モジュールで、新しいテーブルを使用するサンプリングおよびエンジニア リング データを保存します。

いくつかのデータの探索、データの視覚化、および特徴エンジニアリングの例を次に示します。 例については、サンプルの SQL IPython notebook を参照してください、 **Sample IPython Notebooks** フォルダーです。

#### データベースの資格情報を初期化する

次の変数で、データベース接続の設定を初期化します。

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### データベース接続を作成する

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### テーブル nyctaxi_trip の行数と列数を報告する

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- 行数の合計 = 173179759  
- 列数の合計 = 14

#### SQL Server データベースから小規模なサンプルのデータを読み込む

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

サンプルのテーブルの読み取り時間は 6.492000 秒です。  
取得した行数と列数 = (84952, 21)

#### 説明的な統計情報

サンプリングされたデータを探索する準備ができました。 はじめに
わかりやすい統計を確認、 **trip \_distance** (またはその他の) フィールドします。

    df1['trip_distance'].describe()

#### 視覚化: ボックス プロットの例

次に、変位置を視覚化するために、乗車距離のボックス プロットを確認します

    df1.boxplot(column='trip_distance',return_type='dict')

![プロット #1][1]

#### 視覚化: 配布プロットの例

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![プロット #2][2]

#### 視覚化: 棒と線のプロット

この例では、乗車距離を 5 つの箱にビン分割し、ビン分割の結果を視覚化します。

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

上記の箱の分布を、次のように棒または線でプロットできます。

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![プロット #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![プロット #4][4]

#### 視覚化: 散布図の例

間の散布図を紹介 **\_time\_in\_secs** と **trip \_distance** かどうかをあります。
相関関係があるか判断できます。

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![プロット #6][6]

同様に確認するには関係 **rate \_code** と **trip \_distance**します。

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![プロット #8][8]

### SQL でのデータのサブサンプリング

構築するモデル データを準備するときに [Azure Machine Learning Studio](https://studio.azureml.net), 、いずれかを決定することがあります、 **リーダー モジュールで直接使用する SQL クエリ** 、単純な [リーダー] [リーダー] モジュールで使用する新しいテーブルにエンジニア リングとサンプリングされたデータを保持または **選択 * < your\_new\_table\_name > から**します。

このセクションでは、サンプリング データとエンジニアリング データを保持するためにテーブルを新規作成します。 モデルを構築するための直接的な SQL クエリの例が提供される、 [データの探索と SQL Server の特徴エンジニア リング](#dbexplore) セクションです。

#### サンプルのテーブルを作成し、結合されたテーブルの 1% を入力します。 存在する場合は最初にテーブルを削除します。

このセクションでテーブルを結合して **nyctaxi \_trip** と **nyctaxi \_fare**, 1% のランダム サンプルを抽出し、名前の新しいテーブルにサンプリングされたデータを永続化、 **\_one\_percent**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### IPython Notebook での SQL クエリを使用したデータの探索

このセクションでは、上記で作成した新しいテーブルに保持されている 1% のサンプリングされたデータを使用して、データの分布を探索します。 使用して必要に応じて、元のテーブルを使用して同様の探索を実行できることに注意してください **TABLESAMPLE** サンプルまたは特定の時間期間を使用して、結果を制限することで、探索を制限する、 **pickup \_datetime** 分割してのように、 [データの探索と SQL Server の特徴エンジニア リング](#dbexplore) セクションです。

#### 探索: 1 日ごとの乗車の分布

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### 探索: medallion (タクシー番号) ごとの乗車回数の分布

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### IPython Notebook での SQL クエリを使用した特徴の生成

このセクションでは、SQL クエリを使って、前のセクションで作成した 1% のサンプルのテーブルを操作して、新しいラベルと特徴を生成します。

#### ラベルの生成: クラスのラベルを生成する

次の例では、モデリング用に 2 セットのラベルを生成します。

1. 二項ラベル **tipped** (チップが支払われるかどうかを予測する)
2. 多クラス ラベル **[tip \_class]** (チップまたは範囲を予測する)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### 特徴エンジニアリング: カテゴリ列の特徴をカウントする

この例では、各カテゴリをデータの出現回数に置き換えて、カテゴリ フィールドを数値フィールドに変換します。

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### 特徴エンジニアリング: 数値型列のビン分割特徴

この例では、連続する数値フィールドを事前設定されたカテゴリの範囲に変換します。つまり、数値フィールドをカテゴリのフィールドに変換します。

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### 特徴エンジニアリング: 10 進数の緯度と経度から抽出する場所特徴

この例では、[緯度] フィールドや [経度] フィールドの 10 進数表記を、国、都市、町、ブロックなどの異なる粒度に細分化します。新しい geo フィールドは実際の場所にマップされていないことに注意してください。 Geocode の場所のマッピングについては、次を参照してください。 [Bing マップ REST サービス](https://msdn.microsoft.com/library/ff701710.aspx)します。

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### 特徴化されたテーブルの最終形式を検証する

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

モデルの構築とでモデルの配置に進む準備ができました [Azure Machine Learning](https://studio.azureml.net)します。 データは、以前特定したどの予測の問題でも使用できる状態になりました。予測の問題とは、

1. 二項分類: 乗車に対してチップが支払われたかどうかを予測します。

2. 多クラス分類: 以前定義したクラスに従って、支払われたチップの範囲を予測します。

3. 回帰タスク: 乗車で支払われたチップの金額を予測します。  


## <a name="mlmodel"></a>Azure Machine Learning でのモデルの作成

モデリングの演習を開始するには、Azure Machine Learning ワークスペースにログインします。 Machine learning ワークスペースを作成がない場合は、次を参照してください。 [Azure ML ワークスペースを作成する](machine-learning-create-workspace.md)です。

1. Azure Machine Learning で開始するを参照してください。 [Azure Machine Learning Studio は何ですか。](machine-learning-what-is-ml-studio.md)

2. ログイン [Azure Machine Learning Studio](https://studio.azureml.net)します。

3. Studio のホーム ページには、豊富な情報、ビデオ、チュートリアル、モジュール リファレンスへのリンク、およびその他のリソースが用意されています。 Azure Machine Learning についての詳細についてを参照してください、 [Azure Machine Learning ドキュメント センター](http://azure.microsoft.com/documentation/services/machine-learning/)します。

一般的なトレーニング実験は以下のもので構成されています。

1. 作成、 **+ 新規** 実験します。
2. Azure ML へのデータの取得。
3. 必要に応じたデータの事前処理、変換、および操作。
4. 必要に応じた特徴の生成。
5. トレーニング/検証/テスト データ セットへのデータの分割 (またはそれぞれに個別のデータセットを用意する)。
6. 解決する学習問題によって変わる、1 つ以上の機械学習アルゴリズムの選択。 例: 二項分類、多クラス分類、回帰。
7. トレーニング データセットを使用した、1 つ以上のモデルのトレーニング。
8. トレーニング済みのモデルを使用した、検証データセットのスコアリング。
9. 学習問題の関連メトリックを計算するためのモデルの評価。
10. モデルの微調整およびデプロイに最適なモデルの選択。

この演習では、既に SQL Server でデータの探索とエンジニアリングを実行し、Azure ML に取り込むサンプルのサイズを決定しました。 決定した 1 つ以上の予測モデルを作成するには、

1. 使用できる [リーダー] [リーダー] モジュールを使用して Azure ML にデータを取得、 **データの入力と出力** セクションです。 詳細については、[リーダー] [リーダー] モジュールのリファレンス ページを参照してください。

    ![Azure ML リーダー][17]

2. 選択 **Azure SQL Database** として、 **データソース** で、 **プロパティ** パネルです。

3. データベースの DNS 名を入力して、 **データベース サーバー名** フィールドです。 形式: `tcp:<your_virtual_machine_DNS_name>,1433`

4. 入力、 **データベース名** 、対応するフィールドです。

5. 入力、 **SQL ユーザー名** で、* * サーバーのユーザー アカウント名とパスワードの **サーバー ユーザー アカウントのパスワード**します。

6. 確認 **サーバー証明書を受け入れる** オプション。

7.  **データベース クエリ** テキスト編集領域、必要なデータベースのフィールド (ラベルなどの計算フィールドを含む) を抽出するクエリを貼り付けるし、データを希望するサンプルのサイズにダウン サンプリングします。

SQL Server データベースから直接データを読み取る、二項分類の実験の例を次の図に示します。 同様の実験を、多クラス分類と回帰問題で構築することができます。

![Azure ML トレーニング][10]

> [AZURE.IMPORTANT] モデリング データの抽出とサンプリングのクエリの例で提供される前のセクションでは、 **クエリで次の 3 つのモデリングの演習用のすべてのラベルが含まれている**します。 各モデリングの演習における重要な (必須) 手順が、 **除外** の他の 2 つの問題およびその他の不要なラベル **ターゲット リーク**します。 例: 二項分類を使用して、ラベルを使用して **tipped** し、フィールドを除外する **[tip \_class]**, 、**\_amount**, 、および **total\_amount**します。 使用しないものは支払われたチップを意味しているため、ターゲットのリークになります。
>
> 不要な列を除外またはターゲット リークをするには、[プロジェクト列] の [プロジェクト列] モジュールまたは [メタデータのエディター] [メタデータ エディター] を使用できます。 詳細については、[プロジェクト列] の [プロジェクト列]「」を参照し、[メタデータ エディター] [メタデータ エディター] ページを参照します。

## <a name="mldeploy"></a>Azure Machine Learning にモデルを配置する

モデルの準備ができたら、実験から直接 Web サービスとして簡単にデプロイできます。 Azure ML web サービスの展開に関する詳細については、次を参照してください。 [Azure Machine Learning web サービスを配置](machine-learning-publish-a-machine-learning-web-service.md)します。

新しい Web サービスをデプロイするには以下のことを実行する必要があります。

1. スコア付け実験の作成。
2. Web サービスのデプロイ。

スコア付け実験を作成する、 **完了** をクリックして実験をトレーニングするには、 **スコアリング実験の作成** 下部の操作バーにします。

![Azure スコアリング][18]

Azure Machine Learning は、トレーニング実験のコンポーネントに基づいて、スコア付け実験の作成を試みます。 特に、以下のことを実行します。

1. トレーニング済みのモデルを保存し、モデルのトレーニング モジュールを削除する。
2. 特定の論理 **入力ポート** 予想される入力データ スキーマを表します。
3. 論理識別 **出力ポート** を予想される web サービスの出力スキーマを表します。

スコア付け実験が作成されたら、それを確認し、必要に応じて調整します。 一般的な調整は、入力データセットまたはクエリを、ラベル フィールドを除外した入力データセットまたはクエリに置き換えることです。これらはサービスが呼び出されると使用できなくなるためです。 入力データセットまたはクエリのサイズを、入力スキーマを示すのに十分な 2、3 個のレコードまで削減することをお勧めします。 出力ポートは、すべての入力フィールドを除外する/のみ含める一般的、 **スコア付けラベル** と **スコア付け確率** [プロジェクト列] の [プロジェクト列] モジュールを使用して出力します。

サンプルのスコア付け実験を次の図に示します。 展開する準備ができたらをクリックして、 **WEB サービスの発行** 下部の操作バー ボタンをクリックします。

![Azure ML 発行][11]

要点をまとめると、このチュートリアルでは、Azure のデータ サイエンス環境を作成し、データの取得からモデルのトレーニング、Azure Machine Learning Web サービスのデプロイまでを通して、大規模な公開されているデータセットを操作しました。

### ライセンス情報

このサンプルのチュートリアルとそれに付随するスクリプトおよび IPython notebooks は、MIT ライセンスの下で Microsoft と共有されています。 詳細については、GitHub のサンプル コードのディレクトリにある LICENSE.txt ファイルを確認してください。

### 参照

•   [Andrés Monroy NYC タクシー乗車ダウンロード ページ](http://www.andresmh.com/nyctaxitrips/)  
•   [NYC のでは、乗車データをタキシング Chris する (whong) によって](http://chriswhong.com/open-data/foil_nyc_taxi/)   
•   [ニューヨーク市タクシーおよびリムジン コミッション調査および統計](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[project-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/


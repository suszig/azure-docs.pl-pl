<properties
   pageTitle="Hadoop のチュートリアル: Windows での Hadoop の使用 | Microsoft Azure"
   description="HDInsight での Hadoop の使用 Windows での Hadoop クラスターの作成、データに対する Hive クエリの実行、Excel での出力の分析を行う方法について説明します。"
   keywords="hadoop tutorial,hadoop on windows,hadoop cluster,learn hadoop, hive query"
   services="hdinsight"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/13/2015"
   ms.author="nitinme"/>


# Hadoop チュートリアル: Windows 上の HDInsight で Hadoop を使用する

> [AZURE.SELECTOR]
- [Windows](../hdinsight-hadoop-tutorial-get-started-windows.md)
- [Linux](../hdinsight-hadoop-linux-tutorial-get-started.md)

Windows と HDInsight の使用を開始での Hadoop について説明するために、このチュートリアルでは 
Hadoop クラスターでの非構造化データで Hive クエリを実行し、分析する方法、 
Microsoft Excel で結果。

大規模な非構造化データ セットがあり、それに Hive クエリを実行する前提としています 
意味のある情報を抽出します。 これを行うことはそれがまさに 
チュートリアルで使用した既存のバックエンドに変更を加えます。 これを実現するには、次の手順を実行します。

   !["Hadoop tutorial: Create an account; create a Hadoop cluster; submit a Hive query; analyze data in Excel.][image-hdi-getstarted-flow]

HDInsight での Hadoop の説明については、このチュートリアルのデモ ビデオをご覧ください。

![初めての Hadoop チュートリアル動画: Hadoop クラスターでの Hive クエリーの送信、Excel によるデータ分析][img-hdi-getstarted-video]

**[YouTube で HDInsight Hadoop チュートリアルを見る](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**

Azure HDInsight の一般的な利用可能にすると共に、Microsoft も用意されています 
HDInsight Emulator for Azure、旧称 *Microsoft HDInsight 開発者プレビュー*します。 
このエミュレーターは開発者シナリオを対象としており、単一ノード デプロイのみをサポートします。 次に 
HDInsight Emulator の使用に関する情報を参照してください [HDInsight Emulator の概要][hdinsight-emulator]します。

### 前提条件

このチュートリアルを開始する前に、以下の条件を満たしている必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **ワークステーション コンピューター** Office 2013 Professional Plus、Office 365 Pro Plus、Excel 2013 Standalone、または Office 2010 Professional Plus とします。

##Hadoop クラスターを作成する

Hadoop を含む Azure コンピューティング リソースを作成するクラスターを作成するときに 
関連アプリケーションを選択します。 このセクションでは、HDInsight バージョン 3.2 クラスターを作成します。 
他のバージョンの Hadoop クラスターも作成できます。 手順については、 
[カスタム オプションを使用して HDInsight クラスターの作成][hdinsight-provision]します。 詳細については 
HDInsight バージョンとその Sla では、次を参照してください。 
[HDInsight コンポーネントのバージョン](hdinsight-component-versioning.md)です。


**Hadoop クラスターを作成するには**

1. サインイン、 [Azure ポータル](https://ms.portal.azure.com/)します。
2. クリックして **新規**, 、] をクリックして **データ分析**, 、] をクリックし、 **HDInsight**します。 ポータルが開いたら、 **新しい HDInsight クラスター** ブレードです。

    ![Create a new cluster in the Azure Portal](./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.1.png "Create a new cluster in the Azure Portal")

3. 次の値を入力または選択します。

    ![クラスターの名前と種類の入力](./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.2.png "Enter cluster name and type")
    
    |フィールド名| 値|
    |----------|------|
    |クラスター名| クラスターを識別するための一意の名前|
    |クラスターの種類| 選択 **Hadoop** このチュートリアルです。 |
    |クラスターのオペレーティング システム| 選択 **Windows Server 2012 R2 Datacenter** このチュートリアルです。|
    |HDInsight のバージョン| このチュートリアルでは最新バージョンを選択します。|
    |[サブスクリプション]| このクラスターに使用する Azure サブスクリプションを選択します。|
    |リソース グループ | 既存の Azure リソース グループを選択するか、新しいリソース グループを作成します。 基本的な HDInsight クラスターには、クラスターとその既定のストレージ アカウントが含まれています。  管理を容易にするために、これら 2 つを 1 つのリソース グループにグループ化できます。|
    |資格情報| クラスターのログイン ユーザー名とパスワードを入力します。 Windows ベースのクラスターでは、2 つのユーザー アカウントを使用できます。  クラスター ユーザー (または HTTP ユーザー) は、クラスターの管理とジョブの送信に使用されます。  必要に応じて、リモート デスクトップ (RDP) ユーザー アカウントを作成し、クラスターにリモート接続することもできます。 リモート デスクトップを有効にした場合は、RDP ユーザー アカウントが作成されます。|
    |データ ソース| 新しい既定の Azure ストレージ アカウントを作成する場合は、[新規作成] をクリックします。 クラスター名を既定のコンテナー名として使用します。 すべての HDinsight クラスターでは、Azure ストレージ アカウントに既定の BLOB コンテナーがあります。  既定の Azure ストレージ アカウントの場所によって、HDInsight クラスターの場所が決まります。|
    |ノード料金レベル| このチュートリアルでは、既定のワーカー ノードとヘッド ノードの価格レベルで 1 つまたは 2 つのワーカー ノードを使用します。|
    |オプションの構成| この部分はスキップします。|

9.  **新しい HDInsight クラスター** ブレードで、いることを確認 **スタート画面にピン** クリックして選択すると、 **作成**します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターが作成中であることを示し、作成が完了すると、[HDInsight] アイコンを表示するように変化します。

    | 作成中 | 作成の完了時 |
    | ------------------ | --------------------- |
    | ![スタート画面の作成中インジケーター](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioning.png) | ![作成されたクラスター タイル](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または **通知** 作成プロセスで確認するためのページの左にあるエントリです。

10. 作成が完了したら、スタート画面でクラスター用のタイルをクリックして、クラスター ブレードを起動します。


## ポータルから Hive クエリを実行する
HDInsight クラスターが作成されたら、サンプルの Hive テーブルを照会する Hive ジョブを実行します。 使用して *hivesampletable*, 、HDInsight クラスターに付属します。 このテーブルには、モバイル デバイスの製造元、プラットフォーム、モデルに関するデータが格納されています。 このテーブルの Hive クエリは、特定の製造元のモバイル デバイスのデータを取得します。

> [AZURE.NOTE] HDInsight Tools for Visual Studio は、Azure SDK for .NET バージョン 2.5 以降付属します。 Visual Studio からこのツールを使用すると、HDInsight クラスターに接続し、Hive テーブルを作成し、Hive クエリを実行できます。 詳細については、次を参照してください。 [Visual Studio の HDInsight Hadoop Tools を使い始める][1]します。

**クラスター ダッシュボードから Hive ジョブを実行するには**

1. サインイン、 [Azure ポータル](https://ms.portal.azure.com/)します。
2. クリックして **すべてを参照** ] をクリックし、 **HDInsight クラスター** 前のセクションで、作成したクラスターなど、クラスターの一覧を表示します。
3. クリックして、Hive ジョブの実行に使用するクラスターの名前をクリックして **ダッシュ ボード** ブレードの上部にあります。
4. 別のブラウザー タブで Web ページが開きます。 Hadoop ユーザー アカウントとパスワードを入力します。 既定のユーザー名は **admin**;、パスワードは、クラスターの作成中に入力します。
5. ダッシュ ボードで、クリックして、 **Hive エディター** ] タブをクリックします。 次の Web ページが開きます。

    ![Hive Editor tab in the HDInsight cluster dashboard.][img-hdi-dashboard]

    ページの上部にいくつかのタブがあります。 既定のタブは **Hive エディター**, 、他のタブで、 **ジョブ履歴** と **ファイル ブラウザー**します。 ダッシュボードでは、Hive クエリの送信、Hadoop ジョブ ログの確認、ストレージ内のファイルの参照などの操作を実行できます。

    > [AZURE.NOTE] Web ページの URL は *& lt;ClusterName & gt;。>.azurehdinsight.net*します。 ポータルからダッシュボードを開く代わりに、URL を使用して Web ブラウザーからダッシュボードを開くこともできます。

6.  **Hive エディター** ] タブの **クエリ名**, 、入力 **HTC20**します。  クエリ名は、ジョブのタイトルです。 クエリ ウィンドウで、次の図に示すように、Hive クエリを入力します。

    ![[Hive エディター] に入力された Hive クエリ][img-hdi-dashboard-query-select]

4. クリックして **送信**します。 結果が返されるまでしばらく時間がかかります。 画面は 30 秒ごとに更新されます。 クリックすることも **更新** 画面を更新します。

    ![クラスターのダッシュボードの下部に表示される Hive クエリの結果。][img-hdi-dashboard-query-select-result]

5. 状態にジョブの完了が表示されたら、画面のクエリ名をクリックして出力を表示します。 書き留めて **ジョブ開始時刻 (UTC)**します。 この情報は後で必要になります。

    ![HDInsight クラスターのダッシュボードの [ジョブ履歴] タブに表示されるジョブ開始時間。][img-hdi-dashboard-query-select-result-output]

    ページにも表示、 **ジョブの出力** と **ジョブ ログ**します。 出力ファイル (\_stdout) とログ ファイル (\_stderr) をダウンロードすることもできます。


**出力ファイルを参照するには**

1. クラスターのダッシュ ボード] をクリックして **ファイル ブラウザー**します。
2. ストレージ アカウント名をクリックして、コンテナー名 (クラスター名と同じ) をクリックして **ユーザー**します。
3. をクリックして **admin** し、GUID を最終更新時刻よりも少し後、ジョブの開始前にメモした時刻) をクリックします。 この GUID をコピーします。 これは、次のセクションで必要になります。


    ![The Hive query output file GUID listed in the File Browser tab.][img-hdi-dashboard-query-browse-output]


##Excel 用 Microsoft Business Intelligence ツールに接続する

Microsoft Excel 用 Power Query アドインを使用すると、HDInsight からのジョブ出力を Excel にインポートして、Microsoft Business Intelligence ツールで結果をさらに分析することができます。

チュートリアルのこの部分を完了するには、Excel 2013 または 2010 がインストールされている必要があります。

**Microsoft Power Query for Excel をダウンロードするには**

- Microsoft Excel 用 Microsoft Power Query をダウンロード、 [Microsoft ダウンロード センター](http://www.microsoft.com/download/details.aspx?id=39379) し、インストールします。

**HDInsight データをインポートするには**

1. Excel を開き、新しいブックを作成します。
3. クリックして、 **Power Query** ] メニューのをクリックして **その他のソース**, 、] をクリックし、 **Azure HDInsight から**します。

    ![Excel PowerQuery Import menu open for Azure HDInsight.][image-hdi-gettingstarted-powerquery-importdata]

3. 入力、 **アカウント名** クリックして、クラスターに関連付けられている Azure Blob ストレージ アカウントの **OK**します。 (これは、このチュートリアルで作成済みのストレージ アカウントです。)
4. 入力、 **アカウント キー** クリックして Azure Blob ストレージ アカウントの **保存**します。
5. 右側のウィンドウで、BLOB 名をダブルクリックします。 既定で、BLOB名はクラスター名と同じです。

6. 検索 **stdout** で、 **名前** 列です。 いることを確認、対応する GUID **フォルダー パス** 先ほどコピーした GUID と一致する列。 一致している場合、出力データは送信したジョブに対応しています。 クリックして **バイナリ** 左側の列で **stdout**します。

    ![コンテンツの一覧の GUID でデータ出力を検索中。][image-hdi-gettingstarted-powerquery-importdata2]

9. をクリックして **を閉じる (& l)** ハイブをインポートする左上隅にあるジョブを Excel に出力します。

##サンプルの実行

HDInsight クラスターには、ポータルからサンプルを直接実行するための概要ギャラリーを含むクエリ コンソールがあります。 サンプルを使っていくつかの基本的なシナリオを検証することで、HDInsight の操作方法を学ぶことができます。 これらのサンプルには、分析対象のデータ、データに対して実行するクエリなど、必要なものがすべて用意されています。 概要ギャラリーのサンプルの詳細については、次を参照してください。 [Learn hadoop in HDInsight Getting Started ギャラリーを使用した HDInsight Hadoop](hdinsight-learn-hadoop-use-sample-gallery.md)します。

**サンプルを実行するには**

1. Azure ポータルのスタート画面で、作成したクラスターのタイルをクリックします。
 
2. 新しいクラスター ブレードでクリックして **ダッシュ ボード**します。 プロンプトが表示されたら、クラスターの管理者のユーザー名とパスワードを入力します。

    ![クラスター ダッシュボードの起動](./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.Cluster.Dashboard.png "Launch cluster dashboard")
 
3. 表示された web ページからをクリックして、 **Getting Started ギャラリー** ] タブの [し [、 **Solutions with Sample Data** カテゴリで、サンプルを実行するをクリックします。 Web ページの指示に従ってサンプルを実行します。 次の表では、いくつかのサンプルを一覧し、各サンプルについて詳しく説明します。

サンプル | 内容
------ | ---------------
[センサー データの分析][hdinsight-sensor-data-sample] | HDInsight を使用して、暖房、換気、空調 (HVAC) システムによって生成された履歴データを処理し、設定した温度を正確に維持できないシステムを識別する方法について説明します。
[Web サイト ログの分析][hdinsight-weblogs-sample] | HDInsight を使用して、Web サイトのログ ファイルを分析する方法について説明します。このサンプルでは、1 日の間に発生した外部 Web サイトからの Web サイトへのアクセス数を調べ、ユーザーが経験した Web サイト エラーの概要を取得します。
[Twitter の傾向分析](hdinsight-analyze-twitter-data.md) | HDInsight を使用して、Twitter の傾向を分析する方法について説明します。



##次のステップ
この Hadoop のチュートリアルでは、HDInsight で Windows ベースの Hadoop クラスターを作成した後、そのデータ上で Hive クエリを実行し、結果を Excel にインポートする方法を説明しました。このデータは、ビジネス インテリジェンス ツールを使用してさらに処理し、グラフィカルに表示することができます。 詳細については、次のチュートリアルをご覧ください。

- [HDInsight Hadoop Tools for Visual Studio の使用開始][1]
- [HDInsight Emulator の概要][hdinsight-emulator]
- [HDInsight での Azure BLOB ストレージの使用][hdinsight-storage]
- [PowerShell を使用した HDInsight の管理][hdinsight-admin-powershell]
- [HDInsight へのデータのアップロード][hdinsight-upload-data]
- [HDInsight での MapReduce の使用][hdinsight-use-mapreduce]
- [HDInsight での Hive の使用][hdinsight-use-hive]
- [HDInsight での Pig の使用][hdinsight-use-pig]
- [Use Oozie with HDInsight (HDInsight での Oozie の使用)][hdinsight-use-oozie]
- [Develop C# Hadoop streaming programs for HDInsight (HDInsight 用 C# Hadoop ストリーミング プログラムの開発)][hdinsight-develop-streaming]
- [Develop Java MapReduce programs for HDInsight (HDInsight 用 Java MapReduce プログラムの開発)][hdinsight-develop-mapreduce]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-versions]: hdinsight-component-versioning.md


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-emulator]: hdinsight-hadoop-emulator-get-started.md
[hdinsight-develop-streaming]: hdinsight-hadoop-develop-deploy-streaming-jobs.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce.md
[hadoop-hdinsight-intro]: hdinsight-hadoop-introduction.md
[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://ms.portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache-hive]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache-mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache-hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight-hbase-custom-provision]: hdinsight-hbase-tutorial-get-started.md


[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: ../install-configure-powershell.md
[powershell-open]: ../install-configure-powershell.md#Install


[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png

[img-hdi-getstarted-video]: ./media/hdinsight-hadoop-tutorial-get-started-windows/hdi-get-started-video.png


[image-hdi-storageaccount-quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.StorageAccount.QuickCreate.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.QuickCreateCluster.png
[image-hdi-getstarted-flow]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GetStartedFlow.png

[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png
 

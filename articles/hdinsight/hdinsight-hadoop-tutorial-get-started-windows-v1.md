<properties
   pageTitle="Hadoop のチュートリアル: Windows での Hadoop の使用 | Microsoft Azure"
   description="HDInsight での Hadoop の使用 Windows での Hadoop クラスターのプロビジョニング、データに対する Hive クエリの実行、Excel での出力の分析を行う方法について説明します。"
   keywords="hadoop tutorial,hadoop on windows,hadoop cluster,learn hadoop, hive query"
   services="hdinsight"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/29/2015"
   ms.author="nitinme"/>


# Hadoop のチュートリアル: Windows 上の HDInsight で Hadoop と Hive クエリを使用する

> [AZURE.SELECTOR]
- [Windows](../hdinsight-hadoop-tutorial-get-started-windows-v1.md)
- [Linux](../hdinsight-hadoop-linux-get-started.md)

Hadoop on Windows を理解して HDInsight の使用を開始できるように、このチュートリアルでは、Hadoop クラスターの非構造化データで Hive クエリを実行し、その結果を Microsoft Excel で分析する方法について説明します。

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [HDInsight での Hadoop の使用 (Windows)](hdinsight-hadoop-tutorial-get-started-windows.md)

## この Hadoop チュートリアルでは何が達成されますか?

大規模な非構造化データ セットがあり、このデータ セットに対して Hive クエリを実行して、意味のある情報を抽出する場合を仮定します。 これが、このチュートリアルで目的としていることです。 これを実現するには、次の手順を実行します。

   ![Hadoop チュートリアル: アカウントの作成、Hadoop クラスターのプロビジョニング、Hive クエリーの送信、Excel によるデータ分析
][image-hdi-getstarted-flow]

HDInsight での Hadoop の説明については、このチュートリアルのデモ ビデオをご覧ください。

![初めての Hadoop チュートリアル動画: Hadoop クラスターでの Hive クエリーの送信、Excel によるデータ分析][img-hdi-getstarted-video]

**[YouTube で HDInsight Hadoop チュートリアルを見る](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**


Azure HDInsight の一般に利用可能にすると共に、Microsoft もは HDInsight Emulator for Azure、旧称 *Microsoft HDInsight 開発者プレビュー*します。 このエミュレーターは開発者シナリオを対象としており、単一ノード デプロイのみをサポートします。 HDInsight Emulator の使用については、次を参照してください。 [HDInsight Emulator の概要][hdinsight-emulator]します。

> [AZURE.NOTE] HBase クラスターをプロビジョニングする方法については、次を参照してください。 [HDInsight のプロビジョニングの HBase クラスター][hdinsight-hbase-custom-provision]します。 2 つのデータベースのどちらかを選ぶ理由については、「<a href="http://go.microsoft.com/fwlink/?LinkId=510237">What Is The Difference Between HBase and Hadoop/HDFS? (HBase と Hadoop/HDFS の違い)</a>」をご覧ください。

## 前提条件

このチュートリアルを開始する前に、以下の条件を満たしている必要があります。


- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **ワークステーション コンピューター** Office 2013 Professional Plus、Office 365 Pro Plus、Excel 2013 Standalone、または Office 2010 Professional Plus とします。

**このチュートリアルを完了する時間を推定:** 30 分



##<a name="storage"></a>Azure Storage アカウントの作成

HDInsight で Hadoop クラスターをプロビジョニングするときに、Azure ストレージ アカウントを指定します。 Hadoop 分散ファイルシステム (HDFS) と同様、このアカウントの特定の Blob Storage コンテナーが、既定のファイル システムとして設定されます。 既定では、HDInsight クラスターは、指定されたストレージ アカウントと同じデータ センターにプロビジョニングされます。 詳細については、次を参照してください [HDInsight で使用する Azure Blob ストレージ。][hdinsight-storage]

>[AZURE.NOTE] 複数の Hadoop クラスターの既定の Blob ストレージ コンテナーを共有しないでください。

このストレージ アカウントに加えて、クラスターの構成をカスタマイズするときに他のストレージ アカウントを追加できます。 この追加のストレージ アカウント用には、同じ Azure サブスクリプションを使用することも異なる Azure サブスクリプションを使用することもできます。 手順については、次を参照してください。 [HDInsight クラスターのプロビジョニングのカスタム オプションを使用して][hdinsight-provision]します。

このチュートリアルでは、既定の BLOB と既定のストレージ アカウントが使用されています。

**Azure ストレージ アカウントを作成するには**

1. サインイン、 [Azure クラシック ポータル][azure-management-portal]します。
2. をクリックして **新規** の左下隅し、図のように、値を入力します。

    ![簡易作成を使用して新しいストレージ アカウントを設定できる Azure クラシック ポータル。][image-hdi-storageaccount-quickcreate]

>[AZURE.NOTE]  クラスターがサポートされている場所にストレージ アカウントを作成することを確認します。 これらは、:  **東アジア**, 、**東南アジア**, 、**北ヨーロッパ**, 、**西ヨーロッパ**, 、**米国東部**, 、**米国西部**, 、**North Central US**, 、**米国中南部**します。

一覧から、新しいストレージ アカウントを選択し、をクリックして **アクセス キーの管理** ページの下部にあります。 書き留めて、 **プライマリ アクセス キー** (または **セカンダリ アクセス キー**: キーのどちらでもかまいません)。  この情報は後で必要になります。 詳細については、次を参照してください。 [をストレージ アカウントを作成する方法][azure-create-storageaccount] します。

##<a name="provision"></a>Hadoop クラスターのプロビジョニング

クラスターをプロビジョニングすると、Hadoop と関連アプリケーションを含む Azure コンピューティング リソースがプロビジョニングされます。 このセクションでは、Hadoop バージョン 2.4 に基づいて HDInsight バージョン 3.1 クラスターをプロビジョニングします。 また、Azure クラシック ポータル、HDInsight PowerShell コマンドレット、HDInsight .NET SDK を使用して、他のバージョン用の Hadoop クラスターを作成することもできます。 手順については、次を参照してください。 [HDInsight クラスターのプロビジョニングのカスタム オプションを使用して][hdinsight-provision]します。 HDInsight バージョンとその Sla については、次を参照してください。 [HDInsight コンポーネントのバージョン](hdinsight-component-versioning.md)です。

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]


**Hadoop クラスターをプロビジョニングするには**

1. サインイン、 [Azure クラシック ポータル][azure-management-portal]します。

2. をクリックして **新規** の左下隅し、図のように、値を入力します。

    ![HDInsight での Hadoop クラスターの作成。][image-hdi-quickcreatecluster]

<!-- COMMENTED OUT TEXT BEGINS --

4. 次の値を入力または選択します。


    <table border="1">
    <tr><th>Name</th><th>Value</th></tr>
    <tr><td>Cluster Name</td><td>Name of the cluster.</td></tr>
    <tr><td>Cluster Size</td><td>Number of data nodes you want to deploy. The default value is 4. But the option to use 1 or 2 data nodes is also available from the drop-down list. Any number of cluster nodes can be specified by using the <strong>Custom Create</strong> option. Pricing details about the billing rates for various cluster sizes are available. Click the <strong>?</strong> symbol above the drop-down list and follow the link that appears.</td></tr>
    <tr><td>Password</td><td>The password for the <i>admin</i> account. The cluster user name "admin" is specified when you are not using the <strong>Custom Create</strong> option. Note that this is NOT the Windows Administrator account for the VMs on which the clusters are provisioned. The account name can be changed by using the <strong>Custom Create</strong> wizard.</td></tr>
    <tr><td>Storage Account</td><td>Click the drop-down list, and select the storage account that you created. <br/>

    When a storage account is chosen, it cannot be changed. If the storage account is removed, the cluster will no longer be available for use.

    The HDInsight cluster is located in the same datacenter as the storage account.
    </td></tr>
    </table>

    Keep a copy of the cluster name. You will need it later in the tutorial.


5. クリックして **HDInsight クラスターの作成**します。 プロビジョニングが完了したとき、[状態] 列を示しています **を実行している**します。

-テキストをコメント終了-->

>[AZURE.NOTE] 次の手順では、version 3.1 で HDInsight クラスターをプロビジョニングします。 他のバージョンとクラスターを作成するには、使用、 **カスタム作成** メソッドから、ポータルまたは Azure PowerShell を使用します。 各バージョンの相違については、次を参照してください。 [HDInsight で提供されるクラスター バージョンの新機能ですか?][hdinsight-versions]します。 使用方法について、 **カスタム作成** オプションを参照してください [HDInsight クラスターのプロビジョニングのカスタム オプションを使用して][hdinsight-provision]します。


##<a name="sample"></a>ポータルからサンプル データを実行します。

正常にプロビジョニングされた HDInsight クラスターは、ポータルからサンプルを直接実行する概要ギャラリーを含むクエリ コンソールを提供します。 サンプルを使っていくつかの基本的なシナリオを検証することで、HDInsight の操作方法を学ぶことができます。 これらのサンプルには、分析対象のデータ、データに対して実行するクエリなど、必要なものがすべて用意されています。 概要ギャラリーのサンプルの詳細については、次を参照してください。 [Learn hadoop in HDInsight Getting Started ギャラリーを使用した HDInsight Hadoop](hdinsight-learn-hadoop-use-sample-gallery.md)します。

**サンプルを実行する**, からでは Azure Classic Portal、サンプルを実行するクラスターの名前をクリックしてをクリックし、 **クエリ コンソール** ページの下部にあります。 表示された web ページからをクリックして、 **Getting Started ギャラリー** ] タブの [し [、 **サンプル** カテゴリで、サンプルを実行するをクリックします。 Web ページの指示に従ってサンプルを実行します。 次の表では、いくつかのサンプルを一覧し、各サンプルについて詳しく説明します。

サンプル | 内容
------ | ---------------
[センサー データの分析][hdinsight-sensor-data-sample] | HDInsight を使用して、暖房、換気、空調 (HVAC) システムによって生成された履歴データを処理し、設定した温度を正確に維持できないシステムを識別する方法について説明します。
[Web サイト ログの分析][hdinsight-weblogs-sample] | HDInsight を使用して、Web サイトのログ ファイルを分析する方法について説明します。このサンプルでは、1 日の間に発生した外部 Web サイトからの Web サイトへのアクセス数を調べ、ユーザーが経験した Web サイト エラーの概要を取得します。
[Twitter の傾向分析](hdinsight-analyze-twitter-data.md) | HDInsight を使用して、Twitter の傾向を分析する方法について説明します。



##<a name="hivequery"></a>ポータルから Hive クエリを実行する
HDInsight クラスターがプロビジョニングされたら、サンプルの Hive テーブルを照会する Hive ジョブを実行します。 使用して *hivesampletable*, 、HDInsight クラスターに付属します。 このテーブルには、モバイル デバイスの製造元、プラットフォーム、モデルに関するデータが格納されています。 このテーブルの Hive クエリは、特定の製造元のモバイル デバイスのデータを取得します。

> [AZURE.NOTE] HDInsight Tools for Visual Studio は、Azure SDK for .NET バージョン 2.5 以降付属します。 Visual Studio からこのツールを使用すると、HDInsight クラスターに接続し、Hive テーブルを作成し、Hive クエリを実行できます。 詳細については、次を参照してください。 [Visual Studio の HDInsight Hadoop Tools を使い始める][1]します。

**クラスター ダッシュボードから Hive ジョブを実行するには**

1. サインイン、 [Azure クラシック ポータル][azure-management-portal]します。
2. クリックして **HDINSIGHT** 左側のペインからです。 前のセクションで作成したクラスターなど、作成したクラスターが一覧表示されます。
3. クリックして、Hive ジョブの実行に使用するクラスターの名前をクリックして **クエリ コンソール** ページの下部にあります。
4. 別のブラウザー タブで Web ページが開きます。 Hadoop ユーザー アカウントとパスワードを入力します。 既定のユーザー名は **admin**;、パスワードは、クラスターをプロビジョニングする際に入力します。 ダッシュボードは、次のようになります。

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


##<a name="powerquery"></a>Excel 用 Microsoft Business Intelligence ツールに接続する

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


##<a name="nextsteps"></a>次のステップ
この Hadoop のチュートリアルでは、HDInsight で Windows ベースの Hadoop  クラスターをプロビジョニングした後、そのデータ上で Hive クエリを実行し、結果を Excel にインポートする方法を説明しました。このデータは、ビジネス インテリジェンス ツールを使用してさらに処理し、グラフィカルに表示することができます。 詳細については、次のチュートリアルをご覧ください。

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
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache-hive]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache-mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache-hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight-hbase-custom-provision]: hdinsight-hbase-tutorial-get-started.md


[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: ../install-configure-powershell.md
[powershell-open]: ../install-configure-powershell.md#Install


[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.dashboard.query.browse.output.png

[img-hdi-getstarted-video]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/hdi-get-started-video.png


[image-hdi-storageaccount-quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.StorageAccount.QuickCreate.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.ClusterStatus.png
[image-hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.QuickCreateCluster.png
[image-hdi-getstarted-flow]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.GetStartedFlow.png

[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows-v1/HDI.GettingStarted.PowerQuery.ImportData2.png
 

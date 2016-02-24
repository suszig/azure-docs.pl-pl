<properties
    pageTitle="HDInsight 用の Hadoop Emulator の概要 |Microsoft Azure"
    description="MapReduce チュートリアルでインストールされるエミュレーターおよび他のサンプルを使用して、Hadoop エコシステムについて学びます。 HDInsight Emulator は、Hadoop サンドボックスに似た動作をします。"
    keywords="emulator,hadoop ecosystem,hadoop sandbox,mapreduce tutorial"
    editor="cgronlun"
    manager="paulettm"
    services="hdinsight"
    authors="nitinme"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="11/29/2015"
    ms.author="nitinme"/>

#  HDInsight Emulator (Hadoop サンドボックス) を使用した Hadoop エコシステム入門

このチュートリアルでは、Microsoft HDInsight Emulator for Azure (旧 HDInsight サーバー開発者プレビュー) における Hadoop クラスターの概要を紹介します。 HDInsight Emulator には、Azure の HDInsight と同じ Hadoop エコシステムのコンポーネントが付属します。 詳細については、展開されたバージョンに関する情報などを参照してください [Azure HDInsight でサポートされている Hadoop のバージョンですか?](hdinsight-component-versioning.md)します。

エミュレーターがインストールされたら、ワード カウントの MapReduce のチュートリアルに従い、サンプルを実行します。

> [AZURE.NOTE] HDInsight Emulator には、Hadoop クラスターのみが含まれています。 HBase や Storm は含まれません。


HDInsight Emulator は、Hadoop サンドボックスによく似たローカル開発環境を提供します。 Hadoop に慣れている場合は、Hadoop 分散ファイル システム (HDFS) を使用して HDInsight Emulator を使い始めることができます。 HDInsight では、既定のファイル システムが Azure BLOB ストレージです。 そのため、最終的には Azure BLOB ストレージを使用してジョブを開発することになります。 Azure BLOB ストレージを HDInsight Emulator で使用するには、エミュレーターの構成を変更する必要があります。

> [AZURE.NOTE] HDInsight Emulator には、単一ノード展開のみを使用できます。


## 前提条件
このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- HDInsight Emulator には 64 ビット版 Windows が必要です。 次の要件のうち 1 つを満たしている必要があります。

    - Windows 10
    - Windows 8
    - Windows Server 2012

- **Azure PowerShell**します。 参照してください [のインストールおよび使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)します。


##<a name="install"></a>HDInsight Emulator のインストール

Microsoft HDInsight Emulator は、Microsoft Web Platform Installer を使用してインストールします。  

> [AZURE.NOTE] HDInsight Emulator には、英語版のオペレーティング システムのみがサポートされています。 インストールされているエミュレーターの以前のバージョンがあれば、エミュレーターの最新バージョンをインストールする前に、コントロール パネルのプログラムと機能] から次の 2 つのコンポーネントをアンインストールする必要があります。
><ul>
<li>Microsoft HDInsight Emulator for Azure または HDInsight 開発者プレビューでは、どちらかがインストールされています。</li>
<li>Hortonworks Data Platform</li>
</ul>


**HDInsight Emulator をインストールするには**

1. Internet Explorer を開きを参照し、 [Microsoft HDInsight Emulator for Azure インストール ページ][hdinsight-emulator-install]します。
2. クリックして **を今すぐインストール**します。
3. クリックして **実行** ページの下部に HDINSIGHT.exe のインストールを求められたらします。
4. をクリックして、 **はい** ボタンをクリックして、 **ユーザー アカウント制御** インストールを完了するポップアップ ウィンドウです。 Web Platform Installer のウィンドウが表示されます。
6. クリックして **インストール** 、ページの下部にあります。
7. クリックして **同意** ライセンス契約の条項に同意することです。
8. Web Platform Installer が示されていることを確認 **次の製品が正常にインストールされている**, 、] をクリックし、 **完了**します。
9. クリックして **終了** Web Platform Installer のウィンドウを閉じます。

**HDInsight Emulator のインストールを確認するには**

インストールが終わると、デスクトップにアイコンが 3 つインストールされます。 3 つのアイコンは次のようにリンクされています。

- **Hadoop コマンド ライン** -HDInsight Emulator で MapReduce、Pig および Hive ジョブを実行する Hadoop コマンド プロンプト。

- **Hadoop NameNode Status** -NameNode は HDFS のすべてのファイルのツリー ベースのディレクトリを維持します。 また、Hadoop クラスターに保持されたすべてのファイルのデータの場所も追跡されます。 クライアントは、すべてのファイルのデータ ノードがどこに格納されているか見つけるために、NameNode と通信します。

- **Hadoop Yarn のステータス** -クラスター内のノードに MapReduce タスクを配分するジョブ トラッカー。

インストールが終わると、いくつかのローカル サービスもインストールされます。 [サービス] ウィンドウのスクリーンショットは次のようになります。

![エミュレーター ウィンドウに一覧表示された Hadoop エコシステムのサービス。][image-hdi-emulator-services]

HDInsight Emulator に関連するサービスは既定で自動的に開始されません。 Hadoop コマンドラインから、サービスを開始するには、実行 **start\_local\_hdp_services.cmd** c:\hdp (既定の場所)。 コンピューターの再起動後にサービスを自動的に開始するには実行 **セット onebox-autostart.cmd**します。  

インストールと HDInsight のエミュレーターの実行に関する既知の問題について、 [HDInsight Emulator リリース ノート](hdinsight-emulator-release-notes.md)します。 インストール ログは **C:\HadoopFeaturePackSetup\HadoopFeaturePackSetupTools\gettingStarted.winpkg.install.log**します。

##<a name="vstools"></a>Visual Studio の HDInsight ツールのエミュレーターを使用します。

Visual Studio の HDInsight ツールを使用して、HDInsight Emulator に接続できます。 Azure HDInsight クラスターで Visual Studio ツールを使用する方法については、次を参照してください。 [Visual Studio の HDInsight Hadoop Tools を使い始める](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)します。

### エミュレーター用の HDInsight ツールのインストール

HDInsight Visual Studio ツールをインストールする方法の手順については、次を参照してください。 [ここ](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md#installation)します。

### HDInsight Emulator への接続

1. Visual Studio を開きます。
2.  **ビュー** ] メニューのをクリックして **サーバー エクスプ ローラー** サーバー エクスプ ローラー] ウィンドウを開きます。
3. 展開 **Azure**, を右クリックして **HDInsight**, 、クリックして **Connect to HDInsight Emulator**します。

     ![Visual Studio ビュー:　メニューの [Connect to HDInsight emulator]。](./media/hdinsight-hadoop-emulator-get-started/hdi.emulator.connect.vs.png)

4. HDInsight Emulator] ダイアログ ボックスへの接続、WebHCat、HiveServer2、WebHDFS のエンドポイントの値を確認し、をクリックし、 **次**します。 エミュレーターの既定の構成を変更しなかった場合は、既定値が有効になります。 変更を加えた場合は、ダイアログ ボックスの値を更新し、[次へ] をクリックします。

    ![設定されている [Connect to HDInsight emulator] ダイアログ ボックス。](./media/hdinsight-hadoop-emulator-get-started/hdi.emulator.connect.vs.dialog.png)

5. 接続が正常に確立されると、クリックして **完了**します。 サーバー エクスプローラーに HDInsight Emulator が表示されるようになります。

    ![接続されている HDInsight ローカル エミュレーター (Hadoop サンド ボックス) が表示されているサーバー エクスプローラー。](./media/hdinsight-hadoop-emulator-get-started/hdi.emulator.vs.connected.png)

接続が正常に確立されたら、Azure HDInsight クラスターで使用するのと同じようにエミュレーターで HDInsight VS ツールを使用できます。 Azure HDInsight クラスターで VS ツールを使用する方法については、次を参照してください。 [HDInsight Hadoop Tools for Visual Studio を使用して](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)します。

## トラブルシューティング: HDInsight Emulator への HDInsight ツールの接続

1. HDInsight Emulator に接続するときにダイアログ ボックスに表示 HiveServer2 が正常に接続されている場合でも必要があります手動で設定する **hive.security.authorization.enabled プロパティ** に **false** C:\hdp\hive-にある Hive 構成ファイルで*バージョン*\conf\hive-site.xml、およびローカル エミュレーターを再起動します。 HDInsight Tools for Visual Studio は、テーブルの上位 100 行をプレビューしている場合のみ、HiveServer2 に接続します。 このようなクエリを使用しない場合は、Hive 構成をそのままにすることができます。

2. HDInsight Emulator を実行しているコンピューターを動的 IP の割り当て (DHCP) を使用している場合は、C:\hdp\hadoop-を更新する必要があります*バージョン*\etc\hadoop\core-site.xml とプロパティの値の変更 **hadoop.proxyuser.hadoop.hosts** (*) にします。 これにより、Hadoop ユーザーは、すべてのホストから接続して Visual Studio で入力されたユーザーに偽装することができます。

        <property>
            <name>hadoop.proxyuser.hadoop.hosts</name>
            <value>*</value>
        </property>

3. Visual Studio が WebHCat サービスに接続しようとすると、エラーが発生する可能性があります ("エラー": “ジョブ job_XXXX_0001 が見つかりませんでした”)。 この場合は、WebHCat サービスを再起動してもう一度実行してください。 WebHCat サービスを再起動するには、開始、 **サービス** MMC を右クリックして **Apache Hadoop Templeton** (これは、前の WebHCat サービスの名前です、)] をクリック **再起動**します。

##<a name="runwordcount"></a>ワード カウント MapReduce のチュートリアル

これで、ワークステーション上に HDInsight Emulator が構成されました。インストールをテストするために MapReduce のチュートリアルを試してみましょう。 まず、HDFS にデータ ファイルをいくつかアップロードした後、それらのファイルに特定の単語が出現する回数を数えるワード カウント MapReduce ジョブを実行します。

ワード カウント MapReduce プログラムにパッケージ化されている *hadoop の mapreduce の例-2.4.0.2.1.3.0-1981.jar*します。 Jar ファイルは、 *C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\hadoop\mapreduce* フォルダーです。

MapReduce ジョブは、次の 2 つの引数を受け取ります。

- 入力フォルダー。 使用する *hdfs://localhost/user/hdiuser* 入力フォルダーとして。
- 出力フォルダー。 使用する *hdfs://localhost/user/hdiuser/wordcount_output* 出力フォルダーとして。 出力フォルダーは既存のフォルダーにすることはできません。既存のフォルダーの場合、MapReduce ジョブは失敗します。 2 回目に MapReduce ジョブを実行する場合は、別の出力フォルダーを指定するか、既存の出力フォルダーを削除します。

**ワード カウント MapReduce ジョブを実行するには**

1. ダブルクリックして、デスクトップから **Hadoop コマンド ライン** Hadoop コマンド ライン ウィンドウを開きます。 現在のフォルダーは次のようになっているはずです。

        c:\hdp\hadoop-2.4.0.2.1.3.0-1981

    そうでない場合は、次のコマンドを実行します。

        cd %hadoop_home%

2. 次の Hadoop コマンドを実行して、入力ファイルと出力ファイルを格納する HDFS フォルダーを作成します。

        hadoop fs -mkdir /user
        hadoop fs -mkdir /user/HDIUser

3. 次の Hadoop コマンドを実行して、いくつかのローカル テキスト ファイルを HDFS にコピーします。

        hadoop fs -copyFromLocal C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\doc\hadoop\common\*.txt /user/HDIUser

4. 次のコマンドを実行して、/user/HDIUser フォルダーにあるファイルを一覧表示します。

        hadoop fs -ls /user/HDIUser

    次のようにファイルが表示されます。

        C:\hdp\hadoop-2.4.0.2.1.3.0-1981>hadoop fs -ls /user/HDIUser
        Found 4 items
        -rw-r--r--   1 username hdfs     574261 2014-09-08 12:56 /user/HDIUser/CHANGES.txt
        -rw-r--r--   1 username hdfs      15748 2014-09-08 12:56 /user/HDIUser/LICENSE.txt
        -rw-r--r--   1 username hdfs        103 2014-09-08 12:56 /user/HDIUser/NOTICE.txt
        -rw-r--r--   1 username hdfs       1397 2014-09-08 12:56 /user/HDIUser/README.txt

5. 次のコマンドを実行して、ワード カウント MapReduce ジョブを実行します。

        C:\hdp\hadoop-2.4.0.2.1.3.0-1981>hadoop jar C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\hadoop\mapreduce\hadoop-mapreduce-examples-2.4.0.2.1.3.0-1981.jar wordcount /user/HDIUser/*.txt /user/HDIUser/WordCount_Output

6. 次のコマンドを実行して、出力ファイルから "windows" を含む単語の個数を表示します。

        hadoop fs -cat /user/HDIUser/WordCount_Output/part-r-00000 | findstr "windows"

    出力は次のようになります。

        C:\hdp\hadoop-2.4.0.2.1.3.0-1981>hadoop fs -cat /user/HDIUser/WordCount_Output/part-r-00000 | findstr "windows"
        windows 4
        windows.        2
        windows/cygwin. 1

Hadoop コマンドの詳細については、次を参照してください。 [Hadoop コマンド マニュアル][hadoop-commands-manual]します。

##<a name="rungetstartedsamples"></a> サンプル web ログ データを分析します。

HDInsight Emulator をインストールすると、Windows 上の Apache Hadoop ベースのサービスについてユーザーが学習できるサンプルもインストールされます。 これらのサンプルは、通常、ビッグ データ セットを処理する際に必要なタスクを対象としています。 上記 MapReduce のチュートリアルでビルドされたサンプルを試すと、MapReduce プログラミング モデルとそのエコシステムに慣れることができます。

サンプル データは、処理する IIS World Wide Web Consortium (W3C) ログ データを軸として整理されています。 データ生成ツールが用意されていて、さまざまなサイズのデータ セットを作成して HDFS や Azure BLOB ストレージにインポートできます (参照 [Azure Blob ストレージの使用 HDInsight](../hdinsight-use-blob-storage.md) の詳細)。 その後、Azure PowerShell スクリプトによって生成されたデータのページ上で MapReduce、Pig、Hive ジョブを実行できます。 Pig スクリプトと Hive スクリプトは MapReduce 上の抽象化レイヤーであり、最終的に MapReduce プログラムにコンパイルされます。 一連のジョブを実行して、これらの異なる技術を使用する効果と、処理タスクの実行にデータ サイズが及ぼす影響を自分の目で確認することができます。

### このセクションの内容

- [IIS W3C ログ データのシナリオ](#scenarios)
- [サンプル W3C ログ データの読み込み](#loaddata)
- [Java MapReduce ジョブの実行](#javamapreduce)
- [Hive ジョブの実行](#hive)
- [Pig ジョブの実行](#pig)
- [サンプルのリビルド](#rebuild)

###<a name="scenarios"></a>IIS W3C ログ データ シナリオ

W3C シナリオでは、IIS W3C ログ データを、1 MB (小)、500 MB (中)、2 GB (大) の 3 つのサイズで生成し、HDFS または Azure BLOB ストレージにインポートします。 3 種類のジョブがあり、それぞれを C#、Java、Pig、および Hive で実装します。

- **totalhits** -所定のページに対する要求の総数を計算します。
- **avgtime** -ページあたりの要求の (秒) にかかった平均時間を計算します。
- **エラー** -状態が 404 または 500 であった要求の 1 時間あたりのページあたりのエラーの数を計算します。

これらのサンプルとそのドキュメントは、主要な Hadoop テクノロジを詳細に調査したり全面的に実装したりするものではありません。 使用するクラスターはノードを 1 つしかもたないため、ノードを増やす効果は、今回のリリースでは確認できません。

###<a name="loaddata"></a>サンプル W3C ログ データの読み込み

Azure PowerShell スクリプトの importdata.ps1 を使用して、データを生成し HDFS にインポートします。

**サンプル W3C ログ データをインポートするには**

1. デスクトップから Hadoop コマンド ラインを開きます。
2. ディレクトリに移動 **C:\hdp\GettingStarted**します。
3. 次のコマンドを実行して、データを生成し HDFS にインポートします。

        powershell -File importdata.ps1 w3c -ExecutionPolicy unrestricted

    代わりに、Azure Blob ストレージにデータを読み込む場合は、「 [Azure Blob ストレージへの接続](#blobstorage)します。

4. Hadoop コマンド ラインから次のコマンドを実行して、HDFS にインポートしたファイルの一覧を表示します。

        hadoop fs -ls -R /w3c

    出力は次のようになります。

        C:\hdp\GettingStarted>hadoop fs -ls -R /w3c
        drwxr-xr-x   - username hdfs          0 2014-09-08 15:40 /w3c/input
        drwxr-xr-x   - username hdfs          0 2014-09-08 15:41 /w3c/input/large
        -rw-r--r--   1 username hdfs  543683503 2014-09-08 15:41 /w3c/input/large/data_w3c_large.txt
        drwxr-xr-x   - username hdfs          0 2014-09-08 15:40 /w3c/input/medium
        -rw-r--r--   1 username hdfs  272435159 2014-09-08 15:40 /w3c/input/medium/data_w3c_medium.txt
        drwxr-xr-x   - username hdfs          0 2014-09-08 15:39 /w3c/input/small
        -rw-r--r--   1 username hdfs    1058423 2014-09-08 15:39 /w3c/input/small/data_w3c_small.txt

5. ファイルの内容を確認するには、次のコマンドを実行して、1 つのデータ ファイルの内容をコンソール ウィンドウに表示します。

        hadoop fs -cat /w3c/input/small/data_w3c_small.txt

これでデータ ファイルが作成されて HDFS にインポートされました。 別の Hadoop ジョブの実行を開始できます。

###<a name="javamapreduce"></a> Java MapReduce ジョブの実行

MapReduce は Hadoop の基本的コンピューティング エンジンです。 既定で Java で実装されていますが、C# を使用する .NET および Hadoop Streaming を活用した例もあります。 MapReduce ジョブを実行する構文は次のとおりです。

    hadoop jar <jarFileName>.jar <className> <inputFiles> <outputFolder>

jar ファイルとソース ファイルは C:\Hadoop\GettingStarted\Java フォルダーにあります。

**Web ページのヒット数を計算する MapReduce ジョブを実行するには**

1. Hadoop コマンド ラインを開きます。
2. ディレクトリに移動 **C:\hdp\GettingStarted**します。
3. 次のコマンドを実行して、出力ディレクトリが存在している場合は削除します。 出力ディレクトリが既に存在していると、MapReduce ジョブは失敗します。

        hadoop fs -rm -r /w3c/output

3. 次のコマンドを実行します。

        hadoop jar .\Java\w3c_scenarios.jar "microsoft.hadoop.w3c.TotalHitsForPage" "/w3c/input/small/data_w3c_small.txt" "/w3c/output"

    コマンドの要素を次の表に示します。
    <table border="1">
    <tr><td>パラメーター</td><td>注</td></tr>
    <tr><td>w3c_scenarios.jar</td><td>この jar ファイルは C:\hdp\GettingStarted\Java フォルダーにあります。</td></tr>
    <tr><td>microsoft.hadoop.w3c.TotalHitsForPage</td><td>型は、次のどちらかに置き換えることができます。
    <ul>
    <li>microsoft.hadoop.w3c.AverageTimeTaken</li>
    <li>microsoft.hadoop.w3c.ErrorsByPage</li>
    </ul></td></tr>
    <tr><td>/w3c/input/small/data_w3c_small.txt</td><td>入力ファイルは、次のどちらかに置き換えることができます。
    <ul>
    <li>/w3c/input/medium/data_w3c_medium.txt</li>
    <li>/w3c/input/large/data_w3c_large.txt</li>
    </ul></td></tr>
    <tr><td>/w3c/output</td><td>これは、出力フォルダー名です。</td></tr>
    </table>

4. 次のコマンドを実行して、出力ファイルの内容を表示します。

        hadoop fs -cat /w3c/output/part-00000

    次のように出力されます。

        c:\Hadoop\GettingStarted>hadoop fs -cat /w3c/output/part-00000
        /Default.aspx   3380
        /Info.aspx      1135
        /UserService    1126

    たとえば Default.aspx ページは 3360 ヒットということがわかります。 上の表に示されている値を入れ替えてコマンドをもう一度実行します。ジョブの種類とデータのサイズに応じて出力が変わることに注目してください。

### <a name="hive"></a>Hive ジョブの実行
Hive クエリ エンジンは、構造化照会言語 (SQL) の達人であれば戸惑うことはありません。 SQL と同様のインターフェイスと HDFS のリレーショナル データ モデルを提供します。 Hive では、SQL によく似た HiveQL と呼ばれる言語を使用します。 Hive は、Java ベースの MapReduce フレームワーク上に抽象化レイヤーを提供します。Hive クエリは、実行時に MapReduce にコンパイルされます。

**Hive ジョブを実行するには**

1. Hadoop コマンド ラインを開きます。
2. ディレクトリに移動 **C:\hdp\GettingStarted**します。
3. 削除するには、次のコマンドを実行、 **/w3c/hive/input** フォルダー フォルダーが存在する場合。 このフォルダーが存在していると、Hive ジョブは失敗します。

        hadoop fs -rmr /w3c/hive/input

4. 作成するには、次のコマンドを実行、 **/w3c/hive/input** フォルダーとし、データ ファイル/hive 入力フォルダーをコピーします。

        hadoop fs -mkdir /w3c/hive
        hadoop fs -mkdir /w3c/hive/input

        hadoop fs -cp /w3c/input/small/data_w3c_small.txt /w3c/hive/input

5. 実行するには、次のコマンドを実行、 **w3ccreate.hql** スクリプト ファイル。 このスクリプトは Hive テーブルを作成し、データを Hive テーブルに読み込みます。

    > [AZURE.NOTE] この段階で、Hive クエリを実行するのに HDInsight Visual Studio ツールを使用することができます。 Visual Studio を開き、新しいプロジェクトを作成および HDInsight テンプレートから選択 **Hive アプリケーション**します。 プロジェクトが開いたら、クエリを新しいアイテムとして追加します。 クエリは、「 **c:/hdp/GettingStarted/Hive/w3c**します。 クエリがプロジェクトに追加されると、置換 **${hiveconf:input}** と **/w3c/hive/input**, 、キーを押します **送信**します。

        C:\hdp\hive-0.13.0.2.1.3.0-1981\bin\hive.cmd -f ./Hive/w3c/w3ccreate.hql -hiveconf "input=/w3c/hive/input/data_w3c_small.txt"

    次のように出力されます。

        Logging initialized using configuration in file:/C:/hdp/hive-0.13.0.2.1.3.0-1981    /conf/hive-log4j.properties
        OK
        Time taken: 1.137 seconds
        OK
        Time taken: 4.403 seconds
        Loading data to table default.w3c
        Moved: 'hdfs://HDINSIGHT02:8020/hive/warehouse/w3c' to trash at: hdfs://HDINSIGHT02:8020/user/<username>/.Trash/Current
        Table default.w3c stats: [numFiles=1, numRows=0, totalSize=1058423, rawDataSize=0]
        OK
        Time taken: 2.881 seconds

6. 実行するには、次のコマンドを実行、 **w3ctotalhitsbypage.hql** HiveQL スクリプト ファイル。

    > [AZURE.NOTE] 前述のように、ツールを使用して、HDInsight Visual Studio にもこのクエリを実行できます。  

        C:\hdp\hive-0.13.0.2.1.3.0-1981\bin\hive.cmd -f ./Hive/w3c/w3ctotalhitsbypage.hql

    コマンドの要素を次の表に示します。
    <table border="1">
    <tr><td>ファイル</td><td>説明</td></tr>
    <tr><td>C:\hdp\hive-0.13.0.2.1.3.0-1981\bin\hive.cmd</td><td>Hive コマンド スクリプト。</td></tr>
    <tr><td>C:\hdp\GettingStarted\Hive\w3c\w3ctotalhitsbypage.hql</td><td> Hive スクリプト ファイルは、次のいずれかに置き換えることができます。
    <ul>
    <li>C:\hdp\GettingStarted\Hive\w3c\w3caveragetimetaken.hql</li>
    <li>C:\hdp\GettingStarted\Hive\w3c\w3cerrorsbypage.hql</li>
    </ul>
    </td></tr>

    </table>

    w3ctotalhitsbypage.hql HiveQL スクリプトの内容は次のとおりです。

        SELECT filtered.cs_uri_stem,COUNT(*)
        FROM (
          SELECT logdate,cs_uri_stem from w3c WHERE logdate NOT RLIKE '.*#.*'
        ) filtered
        GROUP BY (filtered.cs_uri_stem);

    出力の最後は次のようになります。

        MapReduce Total cumulative CPU time: 5 seconds 391 msec
        Ended Job = job_1410201800143_0008
        MapReduce Jobs Launched:
        Job 0: Map: 1  Reduce: 1   Cumulative CPU: 5.391 sec   HDFS Read: 1058638 HDFS Write: 53 SUCCESS
        Total MapReduce CPU Time Spent: 5 seconds 391 msec
        OK
        /Default.aspx   3380
        /Info.aspx      1135
        /UserService    1126
        Time taken: 49.304 seconds, Fetched: 3 row(s)

各ジョブの最初の手順として、テーブルが作成され、先に作成したファイルからそのテーブルにデータが読み込まれています。 作成されたファイルは、次のコマンドを使用して HDFS の /Hive ノードの下を探すと見つかります。

    hadoop fs -lsr /apps/hive/

### <a name="pig"></a>Pig ジョブの実行

Pig 処理と呼ばれるデータ フロー言語を使用して *Pig Latin*します。 Pig Latin 抽象化は MapReduce より高度なデータ構造を提供し、SQL でリレーショナル データベース管理システムを操作する場合と同じように Hadoop を操作できます。


**Pig ジョブを実行するには**

1. Hadoop コマンド ラインを開きます。
2. ディレクトリに移動、 **C:\hdp\GettingStarted** フォルダーです。
3. 次のコマンドを実行して、Pig ジョブを送信します。

        C:\hdp\pig-0.12.1.2.1.3.0-1981\bin\pig.cmd -f ".\Pig\w3c\TotalHitsForPage.pig" -p "input=/w3c/input/small/data_w3c_small.txt"

    コマンドの要素を次の表に示します。
    <table border="1">
    <tr><td>ファイル</td><td>説明</td></tr>
    <tr><td>C:\hdp\pig-0.12.1.2.1.3.0-1981\bin\pig.cmd</td><td>Pig コマンド スクリプト。</td></tr>
    <tr><td>C:\hdp\GettingStarted\Pig\w3c\TotalHitsForPage.pig</td><td> Pig Latin スクリプト ファイルは、次のいずれかに置き換えることができます。
    <ul>
    <li>C:\hdp\GettingStarted\Pig\w3c\AverageTimeTaken.pig</li>
    <li>C:\hdp\GettingStarted\Pig\w3c\ErrorsByPage.pig</li>
    </ul>
    </td></tr>
    <tr><td>/w3c/input/small/data_w3c_small.txt</td><td> このパラメーターはもっと大きなファイルに置き換えることができます。

    <ul>
    <li>/w3c/input/medium/data_w3c_medium.txt</li>
    <li>/w3c/input/large/data_w3c_large.txt</li>
    </ul>

    </td></tr>
    </table>

    出力は次のようになります。

        (/Info.aspx,1135)
        (/UserService,1126)
        (/Default.aspx,3380)

Pig スクリプトはコンパイルすると MapReduce ジョブになり、その際、複数のジョブが生成されることがあります。そのため、Pig ジョブの実行中に複数の MapReduce ジョブが実行されることがあります。

<!---
### <a name="rebuild"></a>Rebuild the samples
The samples currently contain all the required binaries, so building is not required. If you'd like to make changes to the Java or .NET samples, you can rebuild them by using either the Microsoft Build Engine (MSBuild) or the included Azure PowerShell script.


**サンプルをリビルドするには**

1. Hadoop コマンド ラインを開きます。
2. 次のコマンドを実行します。

        powershell -F buildsamples.ps1
--->

##<a name="blobstorage"></a>Azure BLOB ストレージへの接続
HDInsight Emulator は、既定のファイル システムとして HDFS を使用します。 ただし、Azure HDInsight は、既定のファイル システムとして Azure BLOB ストレージを使用します。 ローカル ストレージの代わりに Azure BLOB ストレージを使用するように HDInsight Emulator を構成できます。 次の手順に従って Azure にストレージ コンテナーを作成し、それを HDInsight Emulator に接続します。

>[AZURE.NOTE] HDInsight は Azure Blob ストレージを使用する方法の詳細については、次を参照してください。 [Azure Blob ストレージの使用 HDInsight](../hdinsight-use-blob-storage.md)します。

次の手順を開始する前に、ストレージ アカウントを作成している必要があります。 手順については、次を参照してください。 [をストレージ アカウントを作成する方法](../storage-create-storage-account.md)します。

**コンテナーを作成するには**

1. サインイン、 [Azure ポータル](https://ms.portal.azure.com/)します。
2. をクリックして **新規** 左側で、次のようにクリックします。 **データ + ストレージ**, 、] をクリックし、 **ストレージ**します。
3. [ストレージ アカウント] ブレードで、以下の画面キャプチャに示すように、プロパティを構成します。
    
    ![ストレージ アカウントの作成](./media/hdinsight-hadoop-emulator-get-started/hdi.emulator.create.storage.png)

    選択 **スタート画面にピン**, をクリックして **作成**します。
4. 新しいストレージ アカウント] ブレードで、ストレージ アカウントを作成するとクリックして **コンテナー** コンテナー] ブレードを開くにはクリックして **追加**します。
5. コンテナーの名前を入力し、クリックして **選択**します。

    ![コンテナーを作成する](./media/hdinsight-hadoop-emulator-get-started/hdi.emulator.create.container.png)

Azure ストレージ アカウントにアクセスする前に、構成ファイルにアカウント名とアカウント キーを追加する必要があります。

**Azure ストレージ アカウントへの接続を構成するには**

1. 開いている **C:\hdp\hadoop-2.4.0.2.1.3.0-1981\etc\hadoop\core-site.xml** メモ帳でします。
2. その他の < プロパティ > タグの横にある次の < プロパティ > タグを追加します。

        <property>
            <name>fs.azure.account.key.<StorageAccountName>.blob.core.windows.net</name>
            <value><StorageAccountKey></value>
        </property>

    < StorageAccountName\ > は置き換える必要があり、< StorageAccountKey\ > は、ストレージ アカウント情報に一致する値。

3. 変更を保存します。 Hadoop サービスを再起動する必要はありません。

ストレージ アカウントにアクセスするには次の構文を使用します。

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/

次に例を示します。

    hadoop fs -ls wasb://myContainer@myStorage.blob.core.windows.net/


##<a name="powershell"></a> Azure PowerShell を実行します。
Azure PowerShell コマンドレットの一部は HDInsight Emulator でもサポートされています。 サポートされているコマンドレットは次のとおりです。

- HDInsight ジョブ定義コマンドレット

    - New-AzureHDInsightSqoopJobDefinition
    - New-AzureHDInsightStreamingMapReduceJobDefinition
    - New-AzureHDInsightPigJobDefinition
    - New-AzureHDInsightHiveJobDefinition
    - New-AzureHDInsightMapReduceJobDefinition
- Start-AzureHDInsightJob
- Get-AzureHDInsightJob
- Wait-AzureHDInsightJob

Hadoop ジョブを送信する例を次に示します。

    $creds = Get-Credential (hadoop as username, password can be anything)
    $hdinsightJob = <JobDefinition>
    Start-AzureHDInsightJob -Cluster http://localhost:50111 -Credential $creds -JobDefinition $hdinsightJob

Get-Credential を呼び出すと入力画面が表示されます。 使用する必要があります **hadoop** ユーザー名とします。 パスワードは任意の文字列でかまいません。 クラスター名は、常に **http://localhost:50111**します。

Hadoop ジョブの送信の詳細については、次を参照してください。 [プログラムによる Hadoop の送信ジョブ](hdinsight-submit-hadoop-jobs-programmatically.md)します。 HDInsight 用の Azure PowerShell コマンドレットの詳細については、次を参照してください。 [HDInsight コマンドレット リファレンス][hdinsight-powershell-reference]します。


##<a name="remove"></a>HDInsight Emulator の削除
Emulator をインストールするには、あるコンピューター、コントロール パネルを開くと [ **プログラム**, 、クリックして **プログラムのアンインストール**します。 右クリックし、インストールされているプログラムの一覧から **Microsoft HDInsight Emulator for Azure**, 、] をクリックし、 **アンインストール**します。


##<a name="nextsteps"></a> 次のステップ
この MapReduce チュートリアルでは、HDInsight Emulator (Hadoop サンド ボックス) をインストールして、Hadoop ジョブをいくつか実行しました。 詳細については、次の記事を参照してください。

- [Azure の HDInsight の概要](../hdinsight-get-started.md)
- [Develop Java MapReduce programs for HDInsight (HDInsight 用 Java MapReduce プログラムの開発)](hdinsight-develop-deploy-java-mapreduce.md)
- [HDInsight 用 C# Hadoop ストリーミング MapReduce プログラムの開発](hdinsight-hadoop-develop-deploy-streaming-jobs.md)
- [HDInsight Emulator リリース ノート](hdinsight-emulator-release-notes.md)
- [HDInsight について議論する MSDN フォーラム](http://social.msdn.microsoft.com/Forums/hdinsight)



[azure-sdk]: http://azure.microsoft.com/downloads/
[azure-create-storage-account]: ../storage-create-storage-account.md
[azure-management-portal]: https://manage.windowsazure.com/
[netstat-url]: http://technet.microsoft.com/library/ff961504.aspx

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce.md

[hdinsight-emulator-install]: http://www.microsoft.com/web/gallery/install.aspx?appid=HDINSIGHT
[hdinsight-emulator-release-notes]: hdinsight-emulator-release-notes.md

[hdinsight-storage]: ../hdinsight-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-develop-deploy-streaming]: hdinsight-hadoop-develop-deploy-streaming-jobs.md
[hdinsight-versions]: hdinsight-component-versioning.md

[Powershell-install-configure]: ../install-configure-powershell.md

[hadoop-commands-manual]: http://hadoop.apache.org/docs/r1.1.1/commands_manual.html

[image-hdi-emulator-services]: ./media/hdinsight-hadoop-emulator-get-started/HDI.Emulator.Services.png
 

<properties
   pageTitle="HDInsight での Apache Storm トポロジのデプロイと管理 | Microsoft Azure"
   description="HDInsight で Storm ダッシュボードを使用して Apache Storm トポロジをデプロイ、監視、管理する方法について説明します。Hadoop Tools for Visual Studio を使用します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/18/2015"
   ms.author="larryfr"/>


# Windows ベースの HDInsight での Apache Storm トポロジのデプロイと管理

Storm ダッシュボードでは、Web ブラウザーを使用して Apache Storm トポロジを HDInsight クラスターに簡単にデプロイして実行できます。 また、ダッシュボードを使用して実行中のトポロジを監視、管理できます。 Visual Studio を使用する場合、HDInsight Tools for Visual Studio でも Visual Studio と同様の機能が提供されます。

Storm ダッシュボードと HDInsight Tools の Storm 機能は、共に Storm REST API に依存し、これを使用して独自の監視と管理ソリューションを作成できます。
> [AZURE.IMPORTANT] このドキュメントの手順では、HDInsight クラスター上に Windows ベースの Storm が必要です。 Linux ベースのクラスターを使用する方法の詳細については、を参照してください [展開および管理上の Linux ベースの HDInsight で Apache Storm トポロジ](hdinsight-storm-deploy-monitor-topology-linux.md)。

## 前提条件

* **HDInsight で Apache Storm** -を参照してください <a href="../hdinsight-storm-getting-started/" target="_blank">HDInsight で Apache Storm を使ってみる</a> クラスターを作成する手順について

* **Storm ダッシュボード**用: HTML5 をサポートする最新の Web ブラウザー

* **Visual Studio** 用: Azure SDK 2.5.1 以降と HDInsight Tools for Visual Studio。 参照してください <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">HDInsight Tools for Visual Studio を使用開始</a> をインストールして Visual Studio の HDInsight ツールを構成します。

    下記いずれかのバージョンの Visual Studio

    * Visual Studio 2012 と <a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">アップデート 4</a>

    * Visual Studio 2013 と <a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">アップデート 4</a> または <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio 2013 Community</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>
    > [AZURE.NOTE] 現在、HDInsight Tools for Visual Studio では HDInsight cluster バージョン 3.2 の Storm のみサポートしています。

## Storm ダッシュボード

Storm ダッシュボードは、Storm クラスターで使用できる Web ページです。 URL は **https://&lt;clustername&gt;.azurehdinsight.net/**, ここで、 **clustername** HDInsight クラスターの Storm の名前を指定します。

Storm ダッシュボードの上部で、**[トポロジの送信**] を選択します。 ページの指示に従ってサンプル トポロジを実行するか、作成したトポロジをアップロードして実行します。

![送信トポロジ ページ][storm-dashboard-submit]

### Storm UI

Storm ダッシュボードで、**[Storm UI]** リンクを選択します。 クラスターの情報と、実行中のトポロジに関する情報が表示されます。

![Storm UI][storm-dashboard-ui]
> [AZURE.NOTE] いくつかのバージョンの Internet Explorer では、Storm UI に初めてアクセスした後、Storm UI が更新されない場合があります。 たとえば、送信した新しいトポロジが表示されない場合や、以前に非アクティブ化したトポロジがアクティブと表示される場合があります。 Microsoft はこの問題を確認しており、解決に取り組んでいます。

#### メイン ページ

Storm UI のメイン ページには、次の情報が表示されます。

* **クラスターの概要**: Storm クラスターの基本情報

* **トポロジの概要**: 実行中のトポロジの一覧 このセクションのリンクを使用して、各トポロジの詳細情報を表示します。

* **スーパーバイザの概要**: Storm のスーパーバイザに関する情報

* **Nimbus 構成**: クラスターの Nimbus 構成

#### トポロジの概要

**[トポロジの概要]** セクションのリンクを選択すると、トポロジに関する次の情報が表示されます。

* **トポロジの概要**: トポロジの基本情報

* **トポロジのアクション**: トポロジで実行できる管理アクション

    * **アクティブ化**: アクティブ化が解除されたトポロジの処理を再開します

    * **アクティブ化の解除**: 実行中のトポロジを一時停止します

    * **再調整**: トポロジの並列処理を調整します。 クラスターのノード数を変更した場合は、実行中のトポロジを再調整する必要があります。 この操作で、クラスター内のノード数の増減に合わせて、トポロジの並列処理を調整できます。

        For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology</a>.

    * **強制終了**: 指定したタイムアウト後に Storm トポロジを停止します。

* **トポロジの統計**: トポロジに関する統計。 **[Window]** 列にあるリンクを使用して、ページで残りのエントリの時間枠を設定します。

* **スパウト**: トポロジで使用するスパウト。 このセクションにあるリンクを使用して、各スパウトの詳細情報を表示します。

* **ボルト**: トポロジで使用するボルト。 このセクションにあるリンクを使用して、各ボルトの詳細情報を表示します。

* **トポロジの構成**: 選択したトポロジの構成

#### スパウトとボルトの概要

**[スパウト]** または **[ボルト]** セクションでアイテムを選択すると、そのアイテムに関する次の情報が表示されます。

* **コンポーネントの概要**: スパウトやボルトの基本情報

* **スパウト/ボルトの統計**: スパウトやボルトの統計。 **[Window]** 列にあるリンクを使用して、ページで残りのエントリの時間枠を設定します。

* **入力の状態** (ボルトのみ): ボルトが消費する入力ストリームに関する情報

* **出力の状態**: このスパウトやボルトから出力されるストリームに関する情報

* **エグゼキュータ**: スパウトやボルトのインスタンスに関する情報 特定のエグゼキュータの **[ポート]** エントリを選択して、このインスタンスで生成された診断情報のログを閲覧します

* **エラー**: このスパウトやボルトのエラー情報。

## HDInsight Tools for Visual Studio

HDInsight Tools は、C# またはハイブリッド トポロジを Storm クラスターに送信する際に使用できます。 次の例では、サンプル アプリケーションを使用します。 HDInsight ツールを使用して独自のトポロジを作成する方法の詳細については、次を参照してください。 [Visual Studio の HDInsight ツールを使用して c# を開発トポロジ](hdinsight-storm-develop-csharp-visual-studio-topology.md)します。

次の手順を使用して、サンプルをお使いの HDInsight クラスターにデプロイし、トポロジを表示、管理します。

1. Visual Studio の HDInsight ツールの最新バージョンをまだインストールがいない場合は、次を参照してください。 <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">HDInsight Tools for Visual Studio を使用開始</a>します。

2. Visual Studio で、**[ファイル]**、**[新規]**、**[プロジェクト]** を選択します。

3. **[新しいプロジェクト]** ダイアログで、**[インストール済]**、**[テンプレート]** の順に展開して **[HDInsight]** を選択します。 テンプレートの一覧から **[Storm Sample]** を選択します。 ダイアログ ボックスの下部に、アプリケーションの名前を入力します。

    ![image](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1. **[ソリューション エクスプローラー]** で、プロジェクトを右クリックして **[Submit to Storm on HDInsight]** を選択します。
    > [AZURE.NOTE] メッセージが表示されたら、Azure サブスクリプションのログイン資格情報を入力します。 2 つ以上のサブスクリプションをお持ちの場合は、HDInsight クラスターの Storm があるサブスクリプションにログインします。

2. **[Storm クラスター]** ドロップダウン リストから HDInsight クラスターの Storm を選択して、**[送信]** を選択します。 送信が成功したかどうかは、**[出力]** ウィンドウを使用して確認できます。

3. トポロジが正常に送信されたら、クラスターの **Storm トポロジ**が表示されます。 実行中のトポロジに関する情報を表示するには、一覧からトポロジを選択します。

    ![Visual Studio モニター](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)
    > [AZURE.NOTE] また、**Storm トポロジ**は、**[サーバー エクスプローラー]** で **[Azure]**、**[HDInsight]** の順に展開して、HDInsight クラスターの Storm を右クリックして **[View Storm Topologies]** を選択して表示できます。

    スパウトやボルトのリンクを使用してこれらのコンポーネントに関する情報を表示します。 各アイテムを選択すると新しいウィンドウが開きます。

4. **[トポロジの概要]** ビューで、**[強制終了]** を選択してトポロジを停止します。
    > [AZURE.NOTE] Storm トポロジは停止されるまで、またはクラスターが削除されるまで実行し続けます。

## REST API

Storm UI は、REST API を基に構築されているため、REST API を使用して同様の管理や監視機能を実行できます。 REST API を使用して、Storm トポロジの管理や監視用のカスタム ツールを作成できます。

詳細については、次を参照してください。 [Storm UI REST API](https://github.com/apache/storm/blob/master/docs/documentation/ui-rest-api.md)します。 以下は、HDInsight での Apache Storm で REST API を使用する場合の情報です。

### ベース URI

HDInsight クラスターの REST API のベース URI は **https://&lt;clustername&gt;.azurehdinsight.net/stormui/api/v1/**, ここで、 **clustername** HDInsight クラスターの Storm の名前を指定します。

### 認証

REST API への要求では、HDInsight クラスターの管理者名とパスワードを使用して、**基本認証**を使用する必要があります。
> [AZURE.NOTE] 基本認証はクリア テキストを使用して送信されるため、**常に** HTTPS を使用してクラスターとの通信を保護する必要があります。

### 戻り値

REST API から返される情報は、クラスターと同じ Azure Virtual Network 上にあるクラスターや仮想マシン上でのみ使用できます。 たとえば、Zookeeper サービスに返された完全修飾ドメイン名 (FQDN) は、インターネットからはアクセスできません。

## 次のステップ

これまでに、Storm ダッシュボードを使用してトポロジをデプロイし、監視する方法について説明しました。続けて、次を学習します。

* [Visual Studio の HDInsight ツールを使用して c# トポロジを開発します。](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Maven を使用して Java ベース トポロジを開発します。](hdinsight-storm-develop-java-topology.md)

複数のトポロジの例の一覧は、次を参照してください。 [HDInsight での Storm に関するトポロジ例](hdinsight-storm-example-topology.md)します。


[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png 
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png 
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png 


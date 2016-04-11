<properties
   pageTitle="HDInsight での Apache Storm トポロジのデプロイと管理 | Microsoft Azure"
   description="HDInsight で Storm ダッシュボードを使用して Apache Storm トポロジをデプロイ、監視、管理する方法について説明します。 Hadoop Tools for Visual Studio を使用します。"
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

#Windows ベースの HDInsight での Apache Storm トポロジのデプロイと管理

Storm ダッシュボードでは、Web ブラウザーを使用して Apache Storm トポロジを HDInsight クラスターに簡単にデプロイして実行できます。 また、ダッシュボードを使用して実行中のトポロジを監視、管理できます。 Visual Studio を使用する場合、HDInsight Tools for Visual Studio でも Visual Studio と同様の機能が提供されます。

Storm ダッシュボードと HDInsight Tools の Storm 機能は、共に Storm REST API に依存し、これを使用して独自の監視と管理ソリューションを作成できます。

> [AZURE.IMPORTANT] このドキュメントの手順では、HDInsight クラスターでの Windows ベースの Storm が必要です。 Linux ベースのクラスターを使用する方法の詳細については、次を参照してください [展開し、Linux ベースの HDInsight での Apache Storm トポロジの管理。](hdinsight-storm-deploy-monitor-topology-linux.md)

##前提条件

* **HDInsight で Apache Storm** -を参照してください <a href="../hdinsight-storm-getting-started/" target="_blank">HDInsight で Apache Storm を使ってみる</a> クラスターを作成する手順について

*  **Storm ダッシュ ボード**: HTML5 をサポートする最新の web ブラウザー

*  **Visual Studio** -Azure SDK 2.5.1 以降と HDInsight Tools for Visual Studio です。 HDInsight Tools for Visual Studio のインストールと構成については、「<a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">HDInsight Tools for Visual Studio の使用開始</a>」をご覧ください。

    下記いずれかのバージョンの Visual Studio

    * Visual Studio 2012 (<a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">Update 4</a>)

    * Visual Studio 2013 (<a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">Update 4</a>) または <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio 2013 Community</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [AZURE.NOTE] 現在、HDInsight Tools for Visual Studio では、HDInsight クラスター version 3.2 での Storm のみがサポートされます。

##Storm ダッシュボード

Storm ダッシュボードは、Storm クラスターで使用できる Web ページです。 URL は **https://&lt;clustername>.azurehdinsight.net/**, ここで、 **clustername** HDInsight クラスターの Storm の名前を指定します。

Storm ダッシュ ボードの上部で、次のように選択します。 **トポロジの送信**します。 ページの指示に従ってサンプル トポロジを実行するか、作成したトポロジをアップロードして実行します。

![送信トポロジ ページ][storm-dashboard-submit]

###Storm UI

Storm ダッシュ ボードから選択、 **Storm UI** リンクします。 クラスターの情報と、実行中のトポロジに関する情報が表示されます。

![Storm UI][storm-dashboard-ui]

> [AZURE.NOTE] Internet Explorer のいくつかのバージョンでは、初めてアクセスした後に、Storm UI が更新されないことを検出することがあります。 たとえば、送信した新しいトポロジが表示されない場合や、以前に非アクティブ化したトポロジがアクティブと表示される場合があります。 Microsoft はこの問題を確認しており、解決に取り組んでいます。

####メイン ページ

Storm UI のメイン ページには、次の情報が表示されます。

* **クラスターの概要**: Storm クラスターに関する基本的な情報です。

* **トポロジの概要**: トポロジの実行の一覧です。 このセクションのリンクを使用して、各トポロジの詳細情報を表示します。

* **スーパーバイザの概要**: Storm のスーパーバイザに関する情報。

* **Nimbus 構成**: クラスターの Nimbus 構成します。

####トポロジの概要

リンクを選択すると、 **トポロジの概要** セクションには、トポロジに関する次の情報が表示されます。

* **トポロジの概要**: トポロジに関する基本的な情報です。

* **トポロジのアクション**: トポロジで実行できる管理アクションです。

    * **アクティブ化**: 非アクティブ化されたトポロジの処理を再開します。

    * **非アクティブ化**: 実行中のトポロジを一時停止します。

    * **再調整**: トポロジの並列処理を調整します。 クラスターのノード数を変更した場合は、実行中のトポロジを再調整する必要があります。 この操作で、クラスター内のノード数の増減に合わせて、トポロジの並列処理を調整できます。

        詳細については、「<a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology (Storm トポロジの並列処理)</a>」をご覧ください。

    * **強制終了**: 指定したタイムアウト後に Storm トポロジを停止します。

* **トポロジの統計**: トポロジに関する統計。 内のリンクを使用して、 **ウィンドウ** ] ページで残りのエントリの時間枠を設定する列。

* **スパウト**: トポロジで使用するスパウト。 このセクションにあるリンクを使用して、各スパウトの詳細情報を表示します。

* **ボルト**: トポロジで使用するボルト。 このセクションにあるリンクを使用して、各ボルトの詳細情報を表示します。

* **トポロジの構成**: 選択したトポロジの構成。

####スパウトとボルトの概要

スパウトの例からを選択すると、 **スパウト** または **ボルト** のセクションでは、選択したアイテムに関する次の情報が表示されます。

* **コンポーネントの概要**: スパウトやボルトに関する基本的な情報です。

* **スパウト/ボルトの統計**: スパウトやボルトの統計。 内のリンクを使用して、 **ウィンドウ** ] ページで残りのエントリの時間枠を設定する列。

* **入力の統計** (ボルトのみ): ボルトが消費する入力ストリームに関する情報。

* **出力の統計**: スパウトやボルトをこれから出力されるストリームに関する情報。

* **エグゼキュータ**: スパウトやボルトのインスタンスに関する情報。 選択、 **ポート** このインスタンスで生成された診断情報のログを表示する特定のエグゼキュータのエントリ。

* **エラー**: スパウトやボルトのすべてのエラー情報。

##HDInsight Tools for Visual Studio

HDInsight Tools は、C# またはハイブリッド トポロジを Storm クラスターに送信する際に使用できます。 次の例では、サンプル アプリケーションを使用します。 HDInsight ツールを使用して独自のトポロジを作成する方法の詳細については、次を参照してください。 [Visual Studio の HDInsight ツールを使用して c# を開発トポロジ](hdinsight-storm-develop-csharp-visual-studio-topology.md)します。

次の手順を使用して、サンプルをお使いの HDInsight クラスターにデプロイし、トポロジを表示、管理します。

1. HDInsight Tools for Visual Studio の最新バージョンをまだインストールしていない場合は、「<a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">HDInsight Tools for Visual Studio の使用開始</a>」をご覧ください。

2. Visual Studio を開き、選択 **ファイル** > **新規** > **プロジェクト**します。

3.  **新しいプロジェクト** ] ダイアログ ボックスで、展開 **インストールされている** > **テンプレート**, 、し、[ **HDInsight**します。 テンプレートの一覧から選択 **Storm サンプル**します。 ダイアログ ボックスの下部に、アプリケーションの名前を入力します。

    ![image](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1.  **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **HDInsight での Storm に送信**します。

    > [AZURE.NOTE] メッセージが表示されたら、Azure サブスクリプションのログイン資格情報を入力します。 2 つ以上のサブスクリプションをお持ちの場合は、HDInsight クラスターの Storm があるサブスクリプションにログインします。

2. HDInsight クラスターの Storm を選択して、 **Storm クラスター** クリックしてドロップダウン リスト **送信**します。 使用して、送信が成功したかどうかを監視することができます、 **出力** ウィンドウです。

3. トポロジが正常に送信されたときに、 **Storm トポロジ** クラスターを表示する必要があります。 実行中のトポロジに関する情報を表示するには、一覧からトポロジを選択します。

    ![Visual Studio モニター](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

    > [AZURE.NOTE] 表示することも **Storm トポロジ** から **サーバー エクスプ ローラー** を展開して **Azure** > **HDInsight**, とを HDInsight クラスターの Storm を右クリックして **View Storm Topologies**します。

    スパウトやボルトのリンクを使用してこれらのコンポーネントに関する情報を表示します。 各アイテムを選択すると新しいウィンドウが開きます。

4.  **トポロジの概要** ビューで、 **Kill** トポロジを停止します。

    > [AZURE.NOTE] Storm トポロジは停止しているか、クラスターが削除されるまで実行を続けます。

##REST API

Storm UI は、REST API を基に構築されているため、REST API を使用して同様の管理や監視機能を実行できます。 REST API を使用して、Storm トポロジの管理や監視用のカスタム ツールを作成できます。

詳細については、「[Storm UI REST API](https://github.com/apache/storm/blob/master/docs/documentation/ui-rest-api.md)」をご覧ください。 以下は、HDInsight での Apache Storm で REST API を使用する場合の情報です。

###ベース URI

HDInsight クラスターの REST API のベース URI は **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, ここで、 **clustername** HDInsight クラスターの Storm の名前を指定します。

###認証

REST API に対する要求を使用する必要があります **基本認証**, ため、HDInsight クラスターの管理者名とパスワードを使用します。

> [AZURE.NOTE] 必要があるため、基本認証はクリア テキストを使用して送信される、 **常に** クラスターとの通信をセキュリティで保護に HTTPS を使用します。

###戻り値

REST API から返される情報は、クラスターと同じ Azure Virtual Network 上にあるクラスターや仮想マシン上でのみ使用できます。 たとえば、Zookeeper サービスに返された完全修飾ドメイン名 (FQDN) は、インターネットからはアクセスできません。

##次のステップ

これまでに、Storm ダッシュボードを使用してトポロジをデプロイし、監視する方法について説明しました。続けて、次を学習します。

* [Visual Studio の HDInsight ツールを使用して C# トポロジを開発する](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Maven を使用した Java ベースのトポロジを開発する](hdinsight-storm-develop-java-topology.md)

複数のトポロジの例の一覧は、次を参照してください。 [HDInsight での Storm に関するトポロジ例](hdinsight-storm-example-topology.md)します。

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png



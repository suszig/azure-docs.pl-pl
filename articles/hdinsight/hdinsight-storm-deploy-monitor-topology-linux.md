<properties
   pageTitle="Linux ベースの HDInsight での Apache Storm トポロジのデプロイと管理 | Microsoft Azure"
   description="Linux ベースの HDInsight で Storm ダッシュボードを使用して Apache Storm トポロジをデプロイ、監視、管理する方法について説明します。Hadoop Tools for Visual Studio を使用します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/16/2015"
   ms.author="larryfr"/>


# Linux ベースの HDInsight での Apache Storm トポロジのデプロイと管理

このドキュメントでは、HDInsight クラスターで Linux ベースの Storm を実行している Storm トポロジを管理および監視する方法の基本について説明します。
> [AZURE.IMPORTANT] この記事の手順では、HDInsight クラスター上の Linux ベースの Storm が必要です。 詳細を展開して、Windows ベースの HDInsight でのトポロジの監視については、を参照してください [展開し、Windows ベースの HDInsight での Apache Storm トポロジを管理](hdinsight-storm-deploy-monitor-topology.md)。

## 前提条件

- **HDInsight クラスターで Linux ベースの Storm**: を参照してください [HDInsight で Apache Storm を使ってみる](hdinsight-storm-get-started-linux.md) クラスターを作成する手順について

- **SSH と SCP を熟知していること**: HDInsight で SSH と SCP を使用する方法については、以下を参照してください。
    - **Linux、Unix または OS X クライアント**:を参照してください [SSH Linux、OS X または Unix から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)
    - **Windows クライアント**:を参照してください [SSH Windows から Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

- **SCP クライアント**: Linux、Unix、OS X システムのすべてに付属しています。 Windows クライアントのことをお勧めは PSCP、 [PuTTY のダウンロード ページ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

## Storm トポロジを開始する

1. SSH を使用して、HDInsight クラスターに接続します。 **USERNAME** には、SSH ログイン名を指定します。 **CLUSTERNAME** には、HDInsight クラスター名を指定します。

     ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

 SSH を使用して HDInsight クラスターに接続する方法の詳細については、次のドキュメントを参照してください。

     - **Linux, Unix or OS X clients**: See [Use SSH with Linux-based Hadoop on HDInsight from Linux, OS X or Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
     - **Windows clients**: See [Use SSH with Linux-based Hadoop on HDInsight from Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 次のコマンドを実行してトポロジの例を開始します。

        storm jar storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    クラスターで WordCount トポロジの例が開始されます。 文はランダムに生成され、文中の各単語の出現回数がカウントされます。
    > [AZURE.NOTE] クラスターにトポロジを送信するときにまずを使用する前に、クラスターを含む jar ファイルをコピーする必要があります、 `storm` コマンドです。 これを使用して実行することができます、 `scp` コマンド ファイルが存在するクライアントを実行します。 たとえば、 `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > ワード カウント例とその他の storm starter 例については、クラスターに含まれて `/usr/hdp/current/storm-client/contrib/storm-starter/`します。

## Storm コマンドを使用して監視および管理する

`Storm` ユーティリティでは、コマンドラインからトポロジを実行するために使用することができます。 次に、一般的に使用されるコマンドの一覧を示します。 使用 `-h storm` コマンドの完全な一覧にします。

### トポロジの一覧を表示する

実行中のすべてのトポロジを一覧表示するには、次のコマンドを使用します。

    storm list

次のような情報が返されます。

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### アクティブ化の解除と再アクティブ化

トポロジが強制終了または再アクティブ化されるまで、トポロジのアクティブ化の解除は一時停止されます。 次のコマンドを使用して、アクティブ化の解除と再アクティブ化を行います。

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### 実行中のトポロジを強制終了する

Storm トポロジが開始されると、停止されるまで実行が継続されます。 トポロジを停止するには、次のコマンドを使用します。

    storm stop TOPOLOGYNAME

### 再調整

トポロジを再調整すると、トポロジの並列処理が変更されます。 たとえば、クラスターのサイズを変更してノートを追加すると、再調整によって実行中のトポロジが新しいノードを利用できるようになります。
> [AZURE.WARNING] トポロジを再調整すると、まずトポロジのアクティブ化が解除され、worker がクラスター全体に平均的に再分配され、最終的に、トポロジは再調整の発生前の状態に戻ります。 そのため、トポロジがアクティブだった場合は、アクティブに戻ります。 非アクティブだった場合は、非アクティブのままになります。

    storm rebalance TOPOLOGYNAME

## Storm UI を使用して監視および管理する

Storm UI には、トポロジの実行を操作する Web インターフェイスがあり、HDInsight クラスターに含まれています。
> [AZURE.IMPORTANT] Storm UI は、インターネットでパブリックに使用できません。SSH トンネルを使用して HDInsight クラスター ヘッド ノードにアクセスする必要があります。 詳細については、SSH トンネルの作成とは、次を参照してください。 [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)します。

次の手順で Storm UI を表示します。

1. ブラウザーで HDInsight クラスターの Ambari Web を開きます。 Ambari Web の URL は https://CLUSTERNAME.azurehdinsight.net、ここで __CLUSTERNAME__ 、クラスターの名前を指定します。

2. ページの左側のサービスの一覧で、__[Storm]__ を選択します。 __[クイック リンク]__ で __[Storm UI]__ を選択します。

    ![クイック リンクの Storm UI エントリ](./media/hdinsight-storm-deploy-monitor-topology-linux/ambari-storm.png)

    これにより、Storm UI が表示されます。

    ![Storm UI](./media/hdinsight-storm-deploy-monitor-topology-linux/storm-ui.png)

> [AZURE.NOTE] Storm UI を使用する場合、一部のバージョンの Internet Explorer で最初にアクセスした後に UI が正しく更新されないことがあります。 たとえば、送信した新しいトポロジが表示されない場合や、以前に非アクティブ化したトポロジがアクティブと表示される場合があります。 Microsoft はこの問題を確認しており、解決に取り組んでいます。

### メイン ページ

Storm UI のメイン ページには、次の情報が表示されます。
- **クラスターの概要**: Storm クラスターの基本情報
- **トポロジの概要**: 実行中のトポロジの一覧 このセクションのリンクを使用して、各トポロジの詳細情報を表示します。
- **スーパーバイザの概要**: Storm のスーパーバイザに関する情報
- **Nimbus 構成**: クラスターの Nimbus 構成

### トポロジの概要

**[トポロジの概要]** セクションのリンクを選択すると、トポロジに関する次の情報が表示されます。
- **トポロジの概要**: トポロジの基本情報
- **トポロジのアクション**: トポロジで実行できる管理アクション
  - **アクティブ化**: アクティブ化が解除されたトポロジの処理を再開します
  - **アクティブ化の解除**: 実行中のトポロジを一時停止します
  - **再調整**: トポロジの並列処理を調整します。 クラスターのノード数を変更した場合は、実行中のトポロジを再調整する必要があります。 この操作で、クラスター内のノード数の増減に合わせて、トポロジの並列処理を調整できます。

      詳細については、次を参照してください。 <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Storm トポロジの並列処理を理解する</a>します。

  - **強制終了**: 指定したタイムアウト後に Storm トポロジを停止します。

- **トポロジの統計**: トポロジに関する統計。 **[Window]** 列にあるリンクを使用して、ページで残りのエントリの時間枠を設定します。
- **スパウト**: トポロジで使用するスパウト。 このセクションにあるリンクを使用して、各スパウトの詳細情報を表示します。
- **ボルト**: トポロジで使用するボルト。 このセクションにあるリンクを使用して、各ボルトの詳細情報を表示します。
- **トポロジの構成**: 選択したトポロジの構成

### スパウトとボルトの概要

**[スパウト]** または **[ボルト]** セクションでアイテムを選択すると、そのアイテムに関する次の情報が表示されます。
- **コンポーネントの概要**: スパウトやボルトの基本情報
- **スパウト/ボルトの統計**: スパウトやボルトの統計。 **[Window]** 列にあるリンクを使用して、ページで残りのエントリの時間枠を設定します。
- **入力の状態** (ボルトのみ): ボルトが消費する入力ストリームに関する情報
- **出力の状態**: このスパウトやボルトから出力されるストリームに関する情報
- **エグゼキュータ**: スパウトやボルトのインスタンスに関する情報 特定のエグゼキュータの **[ポート]** エントリを選択して、このインスタンスで生成された診断情報のログを閲覧します
- **エラー**: このスパウトやボルトのエラー情報。

## REST API

Storm UI は、REST API を基に構築されているため、REST API を使用して同様の管理や監視機能を実行できます。 REST API を使用して、Storm トポロジの管理や監視用のカスタム ツールを作成できます。

詳細については、次を参照してください。 [Storm UI REST API](https://github.com/apache/storm/blob/master/docs/documentation/ui-rest-api.md)します。 以下は、HDInsight での Apache Storm で REST API を使用する場合の情報です。
> [AZURE.IMPORTANT] Storm REST API は、インターネットでパブリックに使用できません。SSH トンネルを使用して HDInsight クラスター ヘッド ノードにアクセスする必要があります。 詳細については、SSH トンネルの作成とは、次を参照してください。 [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)します。

### ベース URI

Linux ベースの HDInsight クラスターの REST API のベース URI がでヘッド ノードで使用可能な **https://HEADNODEFQDN:8744/api/v1/**。 ただし、ヘッド ノードのドメイン名は、クラスターの作成時に生成されると、静的ではありません。

クラスター ヘッド ノードの完全修飾ドメイン名 (FQDN) はいくつかの方法で確認できます。

* __から、SSH セッション__: コマンドを使用して `ヘッドノード f` クラスターへの SSH セッションからです。
* __Ambari Web から__: ページの一番上から __[サービス]__ を選択し、__[Storm]__ を選択します。 __[概要__ ] タブで __[Storm UI Server]__ を選択します。 Storm UI と REST API が実行されているノードの FQDN はページの一番上で確認できます。
* __Ambari REST API から__: コマンドを使用して '-u admin:PASSWORD-g"https://CLUSTERNAME curl
.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"' Storm UI REST API がで実行されているノードに関する情報を取得します。 __PASSWORD__ をクラスターの管理者パスワードに替えます。 __CLUSTERNAME__ をクラスター名に置き換えます。 応答の「host_name」エントリにノードの FQDN が含まれます。

### 認証

REST API への要求では、HDInsight クラスターの管理者名とパスワードを使用して、**基本認証**を使用する必要があります。
> [AZURE.NOTE] 基本認証はクリア テキストを使用して送信されるため、**常に** HTTPS を使用してクラスターとの通信を保護する必要があります。

### 戻り値

REST API から返される情報は、クラスターと同じ Azure Virtual Network 上にあるクラスターや仮想マシン上でのみ使用できます。 たとえば、Zookeeper サービスに返された完全修飾ドメイン名 (FQDN) は、インターネットからはアクセスできません。

## 次のステップ

展開とトポロジを Storm ダッシュ ボードを使用して監視について説明する方法を学習できました。 これでハウツー [Maven を使用した開発の Java ベース トポロジ](hdinsight-storm-develop-java-topology.md)します。

複数のトポロジの例の一覧は、次を参照してください。 [HDInsight での Storm に関するトポロジ例](hdinsight-storm-example-topology.md)します。






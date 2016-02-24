<properties
   pageTitle="Apache Ambari Web UI を使用して HDInsight クラスターを監視および管理する | Microsoft Azure"
   description="Ambari を使用して Linux ベースの HDInsight クラスターを監視および管理する方法を説明します。 このドキュメントでは、HDInsight クラスターに含まれている Ambari Web UI を使用する方法について説明します。"
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

#Ambari Web UI を使用した HDInsight クラスターの管理

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari には使いやすい Web UI と REST API が用意されているため、Hadoop クラスターを簡単に管理および監視できます。 Linux ベースの HDInsight クラスターに含まれている Ambari は、クラスターの監視と構成の変更を行うために使用します。

このドキュメントでは、HDInsight クラスターに含まれている Ambari Web UI を使用する方法について説明します。

> [AZURE.NOTE] この記事の情報は、Linux ベースの HDInsight クラスターにのみ適用されます。 Windows ベースの HDInsight クラスターでは、Ambari REST API を使用した監視のみを利用できます。 参照してください [Ambari API を使用した HDInsight でモニターの Windows ベースの Hadoop](hdinsight-monitor-use-ambari-api.md)します。

##<a id="whatis"></a>Ambari とは

<a href="http://ambari.apache.org" target="_blank">Apache Ambari</a> Hadoop の管理を使いやすい web プロビジョニング、管理、および Hadoop クラスターの監視に使用できる UI を提供することで簡単になります。 開発者がこれらの機能をアプリケーションに統合するには、 <a href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md" target="_blank">Ambari REST API を使用します</a>.

Ambari Web UI は既定で Linux ベースの HDInsight クラスターに付属しています。 

##接続

Ambari Web UI は、HTTPS://CLUSTERNAME.azurehdidnsight.net、HDInsight クラスターで使用できる場所 __CLUSTERNAME__ 、クラスターの名前を指定します。 

> [AZURE.IMPORTANT] HDInsight での Ambari に接続するには、HTTPS が必要です。 管理者のアカウント名を使用して ambari も認証する必要があります (既定値は __admin__,、) と、クラスターが作成されたときに指定したパスワードです。

##SSH プロキシ

> [AZURE.NOTE] クラスター用の Ambari がいくつかのリンク (jobtracker などなど)、Ambari Web UI から、インターネット経由で直接アクセスできますが、インターネットでは公開されません。 そのため、これらの機能にアクセスしようとすると、Secure Shell (SSH) トンネルを使用してプロキシ Web トラフィックをクラスター ヘッド ノードに送信しない限り、サーバーが見つからないことを示すエラー メッセージが表示されます。

Ambari を使用する SSH トンネルを作成する方法の詳細については、次を参照してください。 [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)です。

##Ambari Web UI

Ambari Web UI に接続すると、そのページに対する認証が求められます。 クラスターの作成時に利用したクラスター管理者ユーザー (既定では Admin) とパスワードを使用します。

ページが開くと、上部にバーがあります。 ここには、次の情報とコントロールが含まれています。

![ambari ナビゲーション](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari ロゴ** -クラスターを監視するために使用すると、ダッシュ ボードが開きます。

* **クラスター名 # ops** -進行中の Ambari 操作の数が表示されます。 クラスター名を選択するか、 **# ops** バック グラウンド操作の一覧が表示されます。

* **# alerts** -警告または重大なアラート、クラスターに存在する場合。 選択すると、アラートの一覧が表示されます。

* **ダッシュ ボード** -ダッシュ ボードを表示します。

* **サービス** -クラスター内のサービスの情報と構成設定。

* **ホスト** -クラスター内のノードの情報と構成の設定です。

* **アラート** -情報、警告、重大なアラートのログ。

* **Admin** -ソフトウェア スタック/サービス クラスター、サービス アカウント情報、および Kerberos セキュリティにインストールされています。

* **Admin] ボタン** -Ambari の管理、ユーザーによる設定、およびログアウトします。

##Monitoring

###アラート

Ambari には多数のアラートがあり、そのステータスは次のいずれかになります。

* **OK**

* **警告**

* **CRITICAL**

* **UNKNOWN**

以外のアラート **OK** により、 **# alerts** アラートの数を表示するページの上部にあるエントリです。 このエントリを選択すると、アラートとそのステータスが表示されます。

アラートはから表示できますが、いくつか既定のグループに編成、 **アラート** ページです。

![alerts ページ](./media/hdinsight-hadoop-manage-ambari/alerts.png)

使用して、グループを管理する、 **アクション** メニュー **Manage Alert Groups**します。 ここでは、既存のグループの変更や、グループの新規作成を行えます。

![manage alert groups ダイアログ](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

アラート通知を作成することも、 **アクション** メニュー。 使用して通知を送信するトリガーを作成できます **電子メール** または **SNMP** 特定のアラート/重要度の組み合わせが発生したとき。 アラートのいずれかの場合にアラートを送信するなど、 **YARN Default** に設定されているグループ **重大**します。

![Create alert ダイアログ](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

###クラスター

 **メトリック** ダッシュ ボードのタブには、一連ウィジェットをひとめでクラスターの状態を監視するが簡単にするにはが含まれています。 いくつかのウィジェットなど **CPU 使用率**, 、クリックされたときに、追加の情報を提供します。

![metrics のダッシュボード](./media/hdinsight-hadoop-manage-ambari/metrics.png)

 **Heatmaps** タブまでの色分けによる、緑から赤としてメトリックが表示されます。

![heatmaps のダッシュボード](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

詳細については、クラスター内のノードでは、選択 **ホスト**, 、興味のある特定のノードを選択します。

![ホストの詳細](./media/hdinsight-hadoop-manage-ambari/host-details.png)

###サービス

 **サービス** サイド バーで、ダッシュ ボードは、クラスターで実行されているサービスの状態をすばやく解釈できるを提供します。 さまざまなアイコンで、ステータスまたは実行する必要のある操作が示されます。サービスをリサイクルする必要があることを示す黄色のリサイクル記号などがあります。

![サービスのサイド バー](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

サービスを選択すると、サービスの詳細が表示されます。

![サービスの概要情報](./media/hdinsight-hadoop-manage-ambari/service-details.png)

####クイック リンク:

いくつかのサービスの表示、 **クイック リンク** ページの上部にあるリンクです。 これを使用すると、サービスに固有の次のような Web UI にアクセスできます。

* **ジョブ履歴** -MapReduce ジョブの履歴。

* **リソース マネージャー** -YARN リソース マネージャー UI です。

* **NameNode** -Hadoop 分散ファイル システム (HDFS) NameNode UI です。

* **Oozie Web UI** -Oozie UI です。

これらのいずれかのリンクを選択すると、ブラウザーに新しいタブが開き、選択したページが表示されます。

> [AZURE.NOTE] 選択すると、 **クイック リンク** リンクを任意のサービスは、「サーバーが見つかりません」エラーで、クラスターへのプロキシ web トラフィックに Secure Sockets Layer (SSL) トンネルを使用している場合を除き、します。 これは、この情報を表示するために使用される Web アプリケーションがインターネット上で公開されないためです。
>
> HDInsight で SSL トンネルの使用方法の詳細については、次を参照してください [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスするには。](hdinsight-linux-ambari-ssh-tunnel.md)

##管理

###Ambari ユーザー、グループ、およびアクセス許可

ユーザー、グループ、アクセス許可の管理は、HDInsight クラスターでは使用しないことをお勧めします。

###ホスト

 **ホスト** ページには、クラスター内のすべてのホストが一覧表示します。 ホストを管理するには、次の手順に従います。

![hosts ページ](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [AZURE.NOTE] 追加、使用停止、またはホストを任命を HDInsight クラスターで使用しない必要があります。

1. 管理するホストを選択します。

2. 使用して、 **アクション** ] メニューの [実行する操作を選択します。

    * **すべてのコンポーネントを開始** -ホスト上のすべてのコンポーネントを開始します。

    * **すべてのコンポーネントを停止** -ホスト上のすべてのコンポーネントを停止します。

    * **すべてのコンポーネントを再起動して** - 停止し、ホスト上のすべてのコンポーネントを開始します。

    * **メンテナンス モードを有効にする** -ホストのアラートを抑制します。 これは、アラートを生成する操作を実行する場合に有効になります (実行中のサービスが依存しているサービスを再起動する場合など)。

    * **メンテナンス モードをオフにする** -ホストを通常の警告を返します。

    * **停止** -停止 DataNode または NodeManagers ホストします。

    * **開始** -開始 DataNode または NodeManagers ホストにします。

    * **再起動** -を停止して、ホスト上の DataNode または NodeManagers を起動します。

    * **使用停止** -クラスターからホストを削除します。

        > [AZURE.NOTE] この操作は、HDInsight クラスターでは使用しないでください。

    * **[Recommission]** -使用停止にした、ホスト クラスターを追加します。

        > [AZURE.NOTE] この操作は、HDInsight クラスターでは使用しないでください。

###<a id="service"></a>サービス

 **ダッシュ ボード** または **サービス** ] ページで、使用して、 **アクション** を停止し、すべてのサービスを開始するサービスの一覧の下部にボタンをクリックします。

![service actions](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [AZURE.WARNING] 中に __サービスの追加__ が表示されているこのメニューに、使用しないで、HDInsight クラスターにサービスを追加します。 新しいサービスは、クラスターのプロビジョニング中にスクリプト アクションを使用して追加する必要があります。 スクリプト アクションの使用に関する詳細については、次を参照してください。 [カスタマイズを使用した hdinsight の Script Action](hdinsight-hadoop-customize-cluster-linux.md)します。


中に、 **アクション** ボタンがすべてのサービスを再起動、開始、停止、または特定のサービスを再起動する多くの場合。 個々のサービスで操作を実行するには、次の手順に従います。

1.  **ダッシュ ボード** または **サービス** ] ページで、サービスを選択します。

2. 上部にある、 **概要** ] タブで、使用して、 **サービス アクション** ボタンをクリックし、実行するアクションを選択します。 これにより、すべてのノードでサービスが再起動されます。

    ![service action](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

    > [AZURE.NOTE] クラスターの実行中に、一部のサービスを再起動すると、アラートが生成する可能性があります。 これを回避するには、使用することができます、 **サービス アクション** 有効にするボタン **メンテナンス モード** 、再起動を実行する前に、サービスです。

3. アクションを選択すると、 **# op** 、ページの上部にあるエントリが増分され、バック グラウンド操作が行われていることを表示します。 バックグラウンド操作を表示するように設定されている場合は、バックグラウンド操作の一覧が表示されます。

    > [AZURE.NOTE] 有効にした場合 **メンテナンス モード** 、サービスを使用して無効にしてください、 **サービス アクション** 、操作が完了したら] ボタンをクリックします。

サービスを構成するには、次の手順を実行します。

1.  **ダッシュ ボード** または **サービス** ] ページで、サービスを選択します。

2. 選択、 **Configs** ] タブをクリックします。 現在の構成が表示されます。 以前の構成の一覧も表示されます。

    ![構成](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. 構成を変更し、[表示されるフィールドを使用して **保存**します。 または、以前の構成を選択し、[ **現在** を以前の設定にロールバックします。

##Ambari ビュー

Ambari ビューを使用して、Ambari Web UI に UI 要素を接続できるので、 [Ambari ビュー Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views)します。 HDInsight には、Hadoop クラスター タイプの異なる次のビューが用意されています。

* Yarn Queue Manager: このキュー マネージャーでは、シンプルな UI により YARN キューを表示、変更できます。
* Hive ビュー: Hive ビューを使用すると、Web ブラウザーから直接 Hive クエリを実行できます。 クエリの保存、結果の表示、結果のクラスター ストレージへの保存、または結果のローカル システムへのダウンロードを行えます。 ビューの Hive の使用に関する詳細については、次を参照してください。 [HDInsight での Hive のビューを使用](hdinsight-hadoop-use-hive-ambari-view.md)します。
* Tez ビュー: このビューでは、Tez ジョブの実行方法や、ジョブで使用するリソース関する情報を表示することで、ジョブをより良く理解、最適化できます。

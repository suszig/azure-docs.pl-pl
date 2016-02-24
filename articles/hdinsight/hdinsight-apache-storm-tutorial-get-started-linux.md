<properties
    pageTitle="Apache Storm チュートリアル: HDInsight での Linux ベースの Storm の使用 |Microsoft Azure"
    description="Linux ベースの HDInsight での Apache Storm および Storm Starter サンプルを使用したビッグ データ分析の概要 Storm を使用してデータをリアルタイムに処理する方法について説明します。"
    keywords="apache storm,apache storm tutorial,big data analytics,storm starter"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>


# Apache Storm チュートリアル: Storm Starter サンプルを使用した HDInsight でのビッグ データ分析の概要

Apache Storm は、データ ストリームの処理を目的とし、スケーラビリティとフォールト トレランスに優れた、分散型のリアルタイム計算システムです。 Azure HDInsight の Storm を使用して、Storm でリアルタイムで ビッグ データ分析を実行するクラウドベースの Storm クラスターを作成できます。

> [AZURE.NOTE] この記事の手順では、Linux ベースの HDInsight クラスターを作成します。 HDInsight クラスター上の Windows ベースの Storm を作成する手順については、次を参照してください [Apache Storm チュートリアル: データ分析を使用して HDInsight で Storm Starter サンプルを使ってみる。](hdinsight-apache-storm-tutorial-get-started.md)

## 開始する前に

Apache Storm チュートリアルを正常に完了するには、次の条件を満たす必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- **SSH と SCP 使い慣れた**します。 HDInsight での SSH と SCP の使用方法の詳細については、次の記事をご覧ください。

    - **Linux、Unix または OS X クライアント**: を参照してください [SSH Linux、OS X または Unix から HDInsight 上の Linux ベースの Hadoop の使用](hdinsight-hadoop-linux-use-ssh-unix.md)

    - **Windows クライアント**: を参照してください [SSH Windows から Linux ベースの Hadoop の使用](hdinsight-hadoop-linux-use-ssh-windows.md)

## Storm クラスターを作成する

HDInsight の Storm では、ログ ファイルとクラスターに送信されるトポロジを格納する Azure Blob Storage を使用します。 次の手順を使用して、クラスターに使用する Azure ストレージ アカウントを作成します。

1. サインイン、 [Azure ポータル][preview-portal]します。

2. 選択 **新規**, [ __データ分析__, 、し、[ __HDInsight__

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-apache-storm-tutorial-get-started-linux/new-cluster.png)

3. 入力、 __クラスター名__, 選択してから、 __Storm__ の __クラスターの種類__します。 横に緑のチェック マークが表示されます、 __クラスター名__ 表示されている場合。

    ![クラスター名、クラスターの種類、および OS の種類](./media/hdinsight-apache-storm-tutorial-get-started-linux/clustername.png)

    選択 __Ubuntu__ Linux ベースの HDInsight クラスターを作成します。
    
4. 複数のサブスクリプションの場合は選択し、 __サブスクリプション__ エントリをクラスターに使用される Azure サブスクリプションを選択します。

5.  __リソース グループ__, 、既存のリソース グループの一覧を表示し、[でクラスターを作成する 1 つにエントリを選択することができます。 選択または __新規作成__ し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合、既存のリソース グループのいずれかに設定されます。

6. 選択 __資格情報__, を入力し、 __クラスター ログインのパスワード__ の __クラスター ログイン ユーザー名__します。 入力する必要があります、 __SSH ユーザー名__ いずれかと、 __パスワード__ または __公開キー__, 、SSH ユーザーの認証に使用されます。 最後に、使用して、 __選択__ 資格情報を設定] ボタンをクリックします。

    ![[クラスターの資格情報] ブレード](./media/hdinsight-administer-use-portal-linux/clustercredentials.png)

    HDInsight での SSH の使用方法の詳細については、次の記事をご覧ください。

    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md) 

6.  __データソース__, 、既存のデータ ソースを選択するエントリを選択したり、新しいものを作成します。

    ![[データ ソース] ブレード](./media/hdinsight-apache-storm-tutorial-get-started-linux/datasource.png)
    
    現在、HDInsight クラスターのデータ ソースとして Azure Storage アカウントを選択できます。 エントリを理解するために、次を使用して、 __データソース__ ブレードです。
    
    - __選択方法__: これを設定 __すべてのサブスクリプションから__ 、サブスクリプションのストレージ アカウントの参照を有効にします。 設定 __アクセス キー__ を入力する場合、 __ストレージ名__ と __アクセス キー__ の既存のストレージ アカウント。
    
    - __新規作成__: 新しいストレージ アカウントの作成に使用します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。
    
    - __既定のコンテナーを選択して__: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、コンテナーが特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。
    
    - __場所__: ストレージ アカウントとなる地理的領域には、またはに作成されます。
    
        > [AZURE.IMPORTANT] 既定のデータ ソースの場所を選択すると、HDInsight クラスターの場所も設定されます。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。
        
    - __選択__: これを使用してデータ ソースの構成を保存します。
    
7. 選択 __ノード料金レベル__ このクラスター用に作成するノードに関する情報を表示します。 既定では、ワーカー ノードの数設定されます __4__します。 クラスターの推定コストがブレードの下部に表示されます。

    ![[ノード価格レベル] ブレード](./media/hdinsight-apache-storm-tutorial-get-started-linux/nodepricingtiers.png)
    
    使用して、 __選択__ を保存するボタン、 __ノード料金レベル__ 情報。

8. 選択 __オプションの構成__します。 このブレードでは、クラスターのバージョンを選択できるだけでなく、その他のオプションの設定を追加するなど、 __仮想ネットワーク__ を設定するか、 __カスタム メタストア__ Hive と Oozie のデータを保持します。

    ![[オプションの構成] ブレード](./media/hdinsight-apache-storm-tutorial-get-started-linux/optionalconfiguration.png)

9. いることを確認 __スタート画面にピン__ が選択されて、[ __作成__します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

    | プロビジョニング中 | プロビジョニング完了 |
    | ------------------ | --------------------- |
    | ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-apache-storm-tutorial-get-started-linux/provisioning.png) | ![プロビジョニングされたクラスターのタイル](./media/hdinsight-apache-storm-tutorial-get-started-linux/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または __通知__ プロビジョニング プロセスをチェックするページの左にあるエントリです。

##HDInsight での Storm Starter サンプルの実行

 [Storm starter](https://github.com/apache/storm/tree/master/examples/storm-starter) の例は、HDInsight クラスターに含まれています。 次の手順では、WordCount の例を実行します。

1. SSH を使用して HDInsight クラスターに接続します。

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    SSH ユーザー アカウントを保護するためにパスワードを使用している場合は、パスワードの入力を求められます。 公開キーを使用している場合、`-i` パラメーターを使用して、対応する秘密キーを指定することが必要な場合があります。 たとえば、「`ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`」のように入力します。
        
    Linux ベースの HDInsight での SSH の使用方法の詳細については、次の記事をご覧ください。
    
    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows)

2. 次のコマンドを実行してトポロジの例を開始します。

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology wordcount
        
    > [AZURE.NOTE]  `0.9.3.2.2.4.9-1` Storm の新しいバージョンに HDinsight が更新されると、ファイル名の部分を変更することがあります。

    クラスター上で、’wordcount’ というフレンドリ名の WordCount トポロジの例が開始されます。 文はランダムに生成され、文中の各単語の出現回数がカウントされます。

    > [AZURE.NOTE] クラスターにトポロジを送信するときにまずを使用する前に、クラスターを含む jar ファイルをコピーする必要があります、 `storm` コマンドです。 この場合、ファイルが保存されているクライアントから `scp` コマンドを実行します。 たとえば、`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar` のように指定します。
    >
    > WordCount の例と他の Storm スターターの例は、`/usr/hdp/current/storm-client/contrib/storm-starter/`のクラスターに既に含まれています。

##トポロジの監視

Storm UI には、トポロジの実行を操作する Web インターフェイスがあり、HDInsight クラスターに含まれています。

> [AZURE.IMPORTANT] Storm UI は、インターネット経由でパブリックではないと、HDInsight クラスターのヘッド ノードへの SSH トンネルを使用してアクセスする必要があります。 詳細については、次を参照してください。 [使用 SSH トンネリング ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)です。

次の手順で Storm UI を表示します。

1. クラスターへの SSH トンネルを作成した後に https://CLUSTERNAME.azurehdinsight.net、web ブラウザーを開き、 __CLUSTERNAME__ 、クラスターの名前を指定します。 Ambari Web UI が開きます。

    > [AZURE.NOTE] 場合は、ユーザー名とパスワードの入力を求め、クラスター アドミニストレーター (管理) を入力し、クラスターの作成時に使用したパスワードです。 ブラウザーと Ambari Web UI からそれぞれ認証を求められることがあります。この場合は、両方に同じ資格情報を使用します。

2. ページの左上のサービスのリストから選択 __Storm__します。 選択し、 __Storm UI__ から __クイック リンク__します。

    ![クイックリンクの Storm UI エントリ](./media/hdinsight-apache-storm-tutorial-get-started-linux/ambari-storm.png)

    これにより、Storm UI が表示されます。

    ![Storm UI](./media/hdinsight-apache-storm-tutorial-get-started-linux/stormui.png)
    
    > [AZURE.NOTE] サーバーが検出されないエラーが発生した場合は、クラスターへの SSH トンネルを確立がされない可能性があります。 参照してください [使用 SSH トンネリング ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md) の詳細。

4.  **トポロジの概要**, を選択、 **wordcount** 内のエントリ、 **名前** 列です。 これにより、トポロジの詳細が表示されます。

    ![Storm Starter WordCount トポロジの情報が含まれている Storm ダッシュボード。](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    このページには、次の情報が表示されます。

    * **トポロジの統計** -時間枠で整理された、トポロジのパフォーマンスに関する基本的な情報です。

        > [AZURE.NOTE] 特定の時間枠を選択すると、ページの他のセクションに表示される情報の時間枠を変更します。

    * **スパウト** -各スパウトによって返された最後のエラーを含む、スパウト関する基本的な情報です。

    * **ボルト** -ボルトに関する基本的な情報です。

    * **トポロジの構成** -トポロジの構成に関する詳細情報。

    このページには、トポロジで実行できるアクションも表示されます。

    * **アクティブ化** -非アクティブ化されたトポロジの処理を再開します。

    * **非アクティブ化** -実行中のトポロジを一時停止します。

    * **再調整** -トポロジの並列処理を調整します。 クラスターのノード数を変更した場合は、実行中のトポロジを再調整する必要があります。 この操作で、クラスター内のノード数の増減に合わせて、トポロジの並列処理を調整できます。 詳細については、「[Understanding the parallelism of a Storm topology (Storm トポロジの並列処理)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)」をご覧ください。

    * **強制終了** -指定したタイムアウト後に Storm トポロジを停止します。

5. このページからエントリを選択、 **スパウト** または **ボルト** セクションです。 選択したコンポーネントに関する情報が表示されます。

    ![選択したコンポーネントに関する情報が含まれている Storm ダッシュボード。](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    このページには次の情報が表示されます。

    * **スパウト/ボルトの統計** -時間枠で整理された、コンポーネントのパフォーマンスに関する基本的な情報です。

        > [AZURE.NOTE] 特定の時間枠を選択すると、ページの他のセクションに表示される情報の時間枠を変更します。

    * **入力の統計** (ボルトのみ) - ボルトによって使用されるデータを生成するコンポーネントに関する情報。

    * **出力の統計** -このボルトによって出力されるデータに関する情報。

    * **エグゼキュータ** -このコンポーネントのインスタンスに関する情報。

    * **エラー** -このコンポーネントで生成されたエラーです。

5. スパウトやボルトの詳細を表示しているときからエントリを選択して、 **ポート** 内の列、 **エグゼキュータ** を表示するでは、コンポーネントの特定のインスタンスについて詳しく説明します。

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    このデータからあることがわかります単語 **7** が 1493957 回発生しました。 これは、このトポロジが開始されてから発生した回数です。

##トポロジを停止する

戻り、 **トポロジの概要** ワード カウント トポロジに関するページをクリックして、 **Kill** ボタンから、 **トポロジのアクション** セクションです。 メッセージが表示されたら、トポロジを停止するまでの待機秒数として「10」を入力します。 タイムアウト期間後、トポロジは不要になった表示にアクセスすると、 **Storm UI** ダッシュ ボードのセクションです。

##概要

この Apache Storm チュートリアルでは、Storm Starter を使用して、HDInsight クラスターで Storm を作成する方法と、Storm ダッシュボードを使用して Storm トポロジをデプロイ、監視、管理する方法について説明しました。

##<a id="next"></a>次のステップ

* 次のドキュメントには、HDInsight の Storm と使用できるその他のサンプルの一覧が含まれています。

    * [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/

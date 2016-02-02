<properties
   pageTitle="Apache Storm と HBase を使用したセンサー データの分析 | Microsoft Azure"
   description="仮想ネットワークで Apache Storm に接続する方法について説明します。Storm と HBase を使用して、イベント ハブが発するセンサー データを処理して D3.js で視覚化します。"
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
   ms.date="12/04/2015"
   ms.author="larryfr"/>


# Apache Storm、Event Hub、HBase を HDInsight (Hadoop) で使用してセンサー データを分析する

HDInsight で Apache Storm を使用し、Azure Event Hubs からのセンサー データを処理し、D3.js を使用して表示する方法を説明します。 このドキュメントでは、Azure 仮想ネットワークを使用して、HDInsight の Storm を HDInsight の HBase に接続し、HBase にトポロジからのデータを保存する方法についても説明します。
> [AZURE.NOTE] このドキュメントの情報は、HDInsight クラスター上の Windows ベースの Storm の使用に基づいています。 HDInsight での Linux ベースの Storm から Azure Event Hub を扱う方法については、を参照してください [HDInsight で Storm を使用して Azure Event Hubs からイベントを処理する](hdinsight-storm-develop-java-event-hub-topology.md)。

## 前提条件

* Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

* [HDInsight クラスターでの Apache Storm](../hdinsight-storm-getting-started.md)

* [Node.js](http://nodejs.org/): web ダッシュ ボードと Event Hub にセンサー データを送信するために使用します。

* [Java と JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

* [Maven](http://maven.apache.org/what-is-maven.html)

* [Git](http://git-scm.com/)

> [AZURE.NOTE] Java、JDK、Maven、および Git は、使用、 [Chocolatey NuGet](http://chocolatey.org/) パッケージ マネージャー。

## アーキテクチャ

![アーキテクチャ ダイアグラム](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

この例は、次のコンポーネントで構成されています。

* **Azure Event Hub**: センサーから収集されたデータを提供します。 この例では、偽データを生成するアプリケーションが提供されています。

* **HDInsight でのStorm**: Event Hub からのデータのリアルタイムの処理を提供します。

* **HDInsight での HBase** (省略可能): 永続的な NoSQL データ ストアを提供します。

* **Azure Virtual Network サービス** (省略可能、HBase を使用する場合に必要): HDInsight の Storm と HDInsight クラスターの HBase の間のセキュリティで保護された通信を可能にします。

* **ダッシュボードの Web サイト**: データをリアルタイムでグラフにするサンプルのダッシュボード。

    * Websites は Node.js で実装されるため、すべてのクライアント オペレーティング システムでテスト用に実行できます。また Azure Websites にデプロイすることもできます。

    * [Socket.io](http://socket.io/) Storm トポロジと、web サイト間のリアルタイム通信のために使用します。

        > [AZURE.NOTE] This is an implementation detail. You can use any communications framework, such as raw WebSockets or SignalR.

    * [D3.js](http://d3js.org/) 、web サイトに送信されるデータのグラフを使用します。

トポロジは、HDInsight クラスター上の Storm に用意されている **com.microsoft.eventhubs.spout.EventHubSpout** クラスを使用して、Event Hub からデータを読み取ります。 使用して、web サイトとの通信を実現 [socket.io client.java](https://github.com/nkzawa/socket.io-client.java)します。

使用する HBase との通信を実現する、必要に応じて、 [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) Storm の一部として提供されているクラス。

このトポロジのダイアグラムを次に示します。

![トポロジ ダイアグラム](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)
> [AZURE.NOTE] これは、トポロジの非常に簡略化されたビューです。 実行時に、各コンポーネントのインスタンスは、読み取られている Event Hub のパーティションごとに作成されます。 これらのインスタンスはクラスター内のノード間に配布され、次のようにデータはルーティングされます。
>
> * データ、スパウトからパーサーには負荷分散です。
> * データ パーサーからダッシュ ボードと HBase を使用する場合に、同じデバイスからのメッセージは常に同じコンポーネントにフローできるようにデバイス ID によってグループ化されます。

### コンポーネント

* **EventHub スパウト**: スパウトがの一部として提供される、 [HDInsight Storm Examples](https://github.com/hdinsight/hdinsight-storm-examples) GitHub のです。

* **ParserBolt.java**: スパウトによって出力されるデータは未加工の JSON で、一度に複数のイベントが生成されることがあります。 このボルトでは、スパウトによって送出されたデータを読み込み方法とそれを複数のフィールドを含むタプルとして新しいストリームに出力する方法を示します。

* **DashboardBolt.java**: これは、Java の Socket.io クライアント ライブラリを使用して、Web ダッシュボードにリアルタイムでデータを送信する方法を示します。

## 環境を準備する

この例を使用する前に、Storm トポロジが読み取る Azure Event Hub を作成する必要があります。 また、Event Hub からのデータの読み取りに使用されるコンポーネントは、クラスターでのみ使用できるため、HDInsight トポロジに Storm を作成する必要があります。
> [AZURE.NOTE] Event Hub スパウトは、最終的には Maven から使用できます。

### Event Hub の構成

Event Hub は、この例のデータ ソースです。 新しい Event Hub を作成するには、次の手順に従います。

1. [Azure Classic Portal](https://manage.windowsazure.com), 、を選択して **新規 |Service Bus |Event Hub |カスタム作成**します。

2. **[新しい Event Hub の追加]** ダイアログで **Event Hub 名**を入力し、ハブを作成する **[リージョン]** を選択して、新しい名前空間を作成するか、既存の名前空間を選択します。 最後に、矢印をクリックして続行します。

2. **[Event Hub の構成]** ダイアログ ボックスで、**パーティション カウント**と**メッセージ保持**の値を入力します。 この例では、パーティション カウントに 10 を、メッセージ保持に 1 を使用します。

3. Event Hub が作成されたら、名前空間を選択し、**[Event Hubs]** を選択します。 最後に、先ほど作成した Event Hub を選択します。

4. **[構成]** を選択し、次の情報を使用して新しいアクセス ポリシーを 2 つ作成します。

   <table>
    <tr><th>名前</th><th>アクセス許可</th></tr>
    <tr><td>デバイス</td><td>送信</td></tr>
    <tr><td>Storm</td><td>リッスン</td></tr>
    </table>

    アクセス許可の作成後、ページの下部にある **[保存]** アイコンをクリックします。 これにより、このハブへのメッセージの送信や、このハブから受信したメッセージの読み取りに使用される共有アクセス ポリシーが作成されます。

5. ポリシーの保存後、ページの下部にある **[共有アクセス キー生成コンポーネント]** を使用して、**デバイス**と **Storm** のポリシーのキーを取得します。 後で使用されるため、これらを保存します。

### HDInsight クラスターに Storm を作成する

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。

2. 左側のウィンドウにある **[HDInsight]** をクリックし、ページの左下隅にある **[+ 新規]** をクリックします。

3. 2 番目の列にある [HDInsight] アイコンをクリックし、**[カスタム]** を選択します。

4. **[クラスターの詳細]** ページで、新しいクラスターの名前を入力し、**[クラスターの種類]** に **[Storm]** を選択します。 矢印をクリックして続行します。

5. このクラスターで使用する**データ ノード**の数に 1 を入力します。
    > [AZURE.NOTE] この記事で使用するクラスターのコストを最小限に抑えるには、**クラスター サイズ**を 1 に削減し、使用終了後にクラスターを削除します。

6. 管理者の**ユーザー名**と**パスワード**を入力し、矢印をクリックして続行します。

4. **ストレージ アカウント**の場合、**[新しいストレージを作成する]** を選択するか、既存のストレージ アカウントを選択します。 使用する**アカウント名**と**既定のコンテナー**を選択するか入力します。 左下隅にあるチェック マーク アイコンをクリックして、Storm クラスターを作成します。

## EventHubSpout のダウンロードとインストール

1. ダウンロード、 [HDInsight Storm のサンプル プロジェクト](https://github.com/hdinsight/hdinsight-storm-examples/)します。 ダウンロードしたら、**lib/eventhubs/eventhubs-storm-spout-0.9-jar-with-dependencies.jar** ファイルを見つけます。

2. コマンド プロンプトで次のコマンドを使用して、**eventhubs-storm-spout-0.9-jar-with-dependencies.jar** ファイルをローカルの Maven ストアにインストールします。 これにより、後の手順で Storm プロジェクトに参照として容易に追加できます。

        mvn install:install-file -Dfile=target/eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar


## プロジェクトをダウンロードして構成する

次のコマンドを使用して GitHub からプロジェクトをダウンロードします。

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

コマンドが完了すると、次のディレクトリ構造が構成されます。

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this is the Java topology
            conf/
                Config.properties
                hbase-site.xml
            src/
            test/
            dashboard/ - this is the node.js web dashboard
            SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] このドキュメントでは、このサンプルに含まれるコードの詳細については説明していませんが、コードは完全にコメント化されています。

**Config.properties** ファイルを開き、Event Hub を作成するときに使用した以前の情報を追加します。 この情報を追加したら、ファイルを保存します。

    eventhubspout.username = storm
    
    eventhubspout.password = <the key of the 'storm' policy>
    
    eventhubspout.namespace = <the event hub namespace>
    
    eventhubspout.entitypath = <the event hub name>
    
    eventhubspout.partitions.count = <the number of partitions for the event hub>
    
    ## if not provided, will use storm's zookeeper settings
    ## zookeeper.connectionstring=localhost:2181
    
    eventhubspout.checkpoint.interval = 10
    
    eventhub.receiver.credits = 1024

## コンパイルしてローカルでテストする

テストする前に、ダッシュボードを開始し、トポロジの出力を表示し、Event Hub に格納するデータを生成する必要があります。

### Web アプリケーションの開始

1. 新しいコマンド プロンプトまたはターミナルを開き、ディレクトリを **hdinsight-eventhub-example/dashboard** に変更し、次のコマンドを使用して、Web アプリケーションで必要な依存関係をインストールします。

        npm install

2. 次のコマンドを使用して、Web アプリケーションを開始します。

        node server.js

    次のようなメッセージが表示されます。

        Server listening at port 3000

2. Web ブラウザーを開き、入力 **http://localhost:3000/** アドレスとします。 次のようなページが表示されます。

    ![web dashboard](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    このコマンド プロンプトまたはターミナルは開いたままにします。 テストした後で、Ctrl + C を使用して、Web サーバーを停止します。

### データの生成を開始する

> [AZURE.NOTE] このセクションの手順では、任意のプラットフォームで実行できるように、Node.js を使用します。 その他の言語の例については、**Sendevent** ディレクトリをご覧ください。


1. 新しいコマンド プロンプトかターミナルを開き、ディレクトリを **hdinsight-eventhub-example/SendEvents/nodejs** に変更し、次のコマンドを使用して、アプリケーションで必要な依存関係をインストールします。

        npm install

2. テキスト エディターで、**app.js** ファイルを開き、先に取得した Event Hub の情報を追加します。

        // ServiceBus Namespace
        var namespace = 'servicebusnamespace';
        // Event Hub Name
        var hubname ='eventhubname';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'key';

2. 次のコマンドを使用して、Event Hub に新しいエントリを挿入します。

        node app.js

    Event Hub に送信されるデータを含む出力の複数の行が表示されます。 たとえば、次のように表示されます。

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":0,"Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":1,"Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":2,"Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":3,"Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":4,"Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":5,"Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":6,"Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":7,"Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":8,"Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":9,"Temperature":84}


### トポロジの開始

2. 次のコマンドを使用してトポロジをローカルで開始します。

    mvn compile exec:java -Dstorm.topology=com.microsoft.examples.Temperature

    これでトポロジが開始され、Event Hub からファイルを読み取り、Azure Websites で実行されているダッシュボードにファイルを送信します。 Web ダッシュボードに、次のような行が表示されます。

    ![dashboard with data](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. ダッシュ ボードの実行中を使用して、 `ノード app.js` コマンド、ダッシュ ボードに新しいデータを送信する前の手順を実行します。 温度の値がランダムに生成されるため、グラフを更新して新しい値を表示する必要があります。

3. この動作を確認したら、Ctrl+C キーを押してトポロジを停止します。 SendEvent アプリを停止するには、ウィンドウを選択して、任意のキーを押します。 Ctrl + C を使用して、Web サーバーも停止できます。

## HDInsight へのトポロジのパッケージ化とデプロイ

開発環境で、次の手順に従って HDInsight Storm クラスターで Temperature トポロジを実行します。

### Web サイトのダッシュボードを発行する

1. ダッシュ ボードを Azure の web サイトを展開する」の手順に従います [をビルドおよび Node.js web サイトを Azure に配置](../web-sites-nodejs-develop-deploy-mac.md)します。 Web サイトの URL が、**mywebsite.azurewebsites.net** のようになることに注意してください。

2. Web サイトが作成されたら、Azure クラシック ポータル内のサイトに移動して、**[構成]** タブを選択します。 **[Web ソケット]** を有効にし、ページの下部にある **[保存]** をクリックします。

2. **hdinsight-eventhub-example\TemperatureMonitor\src\main\java\com\microsoft\examples\bolts\DashboardBolt.java** を開き、発行されたダッシュボードの URL をポイントするように次の行を変更します。

        socket = IO.socket("http://mywebsite.azurewebsites.net");

3. **DashboardBolt.java** ファイルを保存します。

### トポロジのパッケージ化とデプロイ

1. 次のコマンドを使用して、プロジェクトから JAR パッケージを作成します。

        mvn package

    これにより、プロジェクトの **target** ディレクトリに **TemperatureMonitor-1.0-SNAPSHOT.jar** という名前のファイルが作成されます。

2. 手順に従います [展開し、Storm トポロジを管理](hdinsight-storm-deploy-monitor-topology.md) をアップロードして HDInsight クラスター上の storm を使用してトポロジを開始、 **Storm ダッシュ ボード**します。

3. トポロジが開始した後は、ブラウザーを開き、Azure に発行した web サイトを使用して、 `ノード app.js` Event Hub にデータを送信するコマンドです。 情報を表示する Web ダッシュボードの更新が表示されます。

    ![dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## 省略可能: HBase の使用

Storm と HBase を一緒に使用するには、Azure Virtual Network を作成し、ネットワーク内で Storm クラスターと HBase クラスターを作成する必要があります。

### Azure Virtual Network の作成 (省略可能)

この例で HBase を使用する場合は、HDInsight クラスター上の Storm と HDInsight クラスター上の HBase を格納する Azure Virtual Network を作成する必要があります。

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。

2. ページ下部で、**[+ 新規]** > **[Network Services]** > **[Virtual Network]** > **[簡易作成]** をクリックします。

3. 次の値を入力または選択します。

    - **名前**: 仮想ネットワークの名前

    - **アドレス空間**: クラスター内のすべてのノードにアドレスを提供するために十分な大きさの、仮想ネットワークのアドレス空間を選択します。 そうでないと、プロビジョニングは失敗します。

    - **最大 VM 数**: 仮想マシンの最大数のいずれかを選択します。

    - **場所**: 場所は作成する HBase クラスターと同じである必要があります。

    - **DNS サーバー**: この記事では、Azure が提供する内部 DNS サーバーを使用するため、**[なし]** を選択します。 カスタム DNS サーバーを使用した、より高度なネットワーク構成もサポートされています。 詳細なガイダンスについては、次を参照してください。 [名前解決 (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)します。

4. **[仮想ネットワークの作成]** をクリックします。 新しい仮想ネットワーク名が一覧に表示されます。 [ステータス] 列に **[作成済み]** が表示されるまで待機します。

5. メイン ウィンドウで、作成した仮想ネットワークをクリックします。

6. ページの上部で、**[ダッシュボード]** をクリックします。

7. **[概要]** の下の **[仮想ネットワーク ID]** を書き留めます。 この ID は、Storm と HBase クラスターのプロビジョニング時に必要です。

8. ページの上部で、**[構成]** をクリックします。

9. ページの下部に示される既定のサブネット名は、**Subnet-1** です。 **[サブネットの追加]** をクリックして「**Subnet-2**」を追加します。 これらのサブネットには、Storm クラスターと HBase クラスターが収容されます。
    > [AZURE.NOTE] この記事では、ノードが 1 つのみのクラスターを使用します。 マルチノード クラスターを作成する場合は、クラスターに使用されるサブネットの **[CIDR (アドレス数)]** を確認する必要があります。 アドレス数は、worker ノード数に 7 (ゲートウェイ: 2、ヘッドノード: 2、Zookeeper: 3) を加えた合計よりも多くする必要があります。 たとえば、10 ノードの HBase クラスターが必要な場合、サブネットのアドレス数は、17 (10+7) を超えている必要があります。 17 以下の場合、デプロイは失敗します。
    >
    > 1 つのクラスターには単一のサブネットを指定することを強くお勧めします。

11. ページの下部にある **[保存]** をクリックします。

### 仮想ネットワークに Storm クラスターと HBase クラスターを作成します。

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。

2. 左側のウィンドウにある **[HDInsight]** をクリックし、ページの左下隅にある **[+ 新規]** をクリックします。

3. 2 番目の列にある [HDInsight] アイコンをクリックし、**[カスタム]** を選択します。

4. **[クラスターの詳細]** ページで、新しいクラスターの名前を入力し、**[クラスターの種類]** に **[Storm]** を選択します。 矢印をクリックして続行します。

5. このクラスターで使用する**データ ノード**の数に 1 を入力します。 **[リージョン/仮想ネットワーク]** に、先ほど作成した Azure Virtual Network を選択します。 **[仮想ネットワーク サブネット]** に **Subnet-1** を選択します。
    > [AZURE.NOTE] この記事で使用するクラスターのコストを最小限に抑えるには、**クラスター サイズ**を 1 に削減し、使用終了後にクラスターを削除します。

6. 管理者の **[ユーザー名]** と **[パスワード]** を入力し、矢印を選択して続行します。

4. **ストレージ アカウント**の場合、**[新しいストレージを作成する]** を選択するか、既存のストレージ アカウントを選択します。 使用する**アカウント名**と**既定のコンテナー**を選択するか入力します。 左下隅にあるチェック マーク アイコンをクリックして、Storm クラスターを作成します。

5. これらの手順を繰り返して、新しい **HBase** クラスターを作成します。 主な違いを次に示します。

    * **[クラスターの種類]**: **[HBase]** を選択します

    * **[仮想ネットワーク サブネット]**: **[Subnet-2]** を選択します

    * **[ストレージ アカウント]**: Storm クラスターに使用されるコンテナーとは別のコンテナーを使用する必要があります。

### HBase DNS サフィックスの探索

Storm クラスターから HBase に書き込むには、HBase クラスターに完全修飾ドメイン名 (FQDN) を使用する必要があります。 次のコマンドを使用して、この情報を探索します。

    curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

返された JSON データで、**"host_name"** エントリを見つけます。 これには、クラスターのノードの FQDN が含まれています。次に例を示します。

    ...
    "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
    ...

クラスター名で始まるドメイン名の部分は、DNS サフィックスです。たとえば、**mycluster.b1.cloudapp.net** となります。

### HBase ボルトを有効にします。

1. 開いている **hdinsight-eventhub-example\TemperatureMonitor\conf\hbase-site.xml** と置換、 `サフィックス` 、HBase クラスターの DNS サフィックスを持つ次の行のエントリが以前に入手できます。 これらの変更を行った後は、ファイルを保存します。

        <value>zookeeper0.suffix,zookeeper1.suffix,zookeeper2.suffix</value>

    これは、HBase クラスターとの通信に HBase ボルトによって使用されます。

1. 開いている **hdinsight eventhub-example\TemperatureMonitor\src\main\java\com\microsoft\examples\bolts\** をテキスト エディターで削除することで、次の行をコメント解除します、 `//` 最初からです。 これらの変更を行った後は、ファイルを保存します。

        topologyBuilder.setBolt("HBase", new HBaseBolt("SensorData", mapper).withConfigKey("hbase.conf"), spoutConfig.getPartitionCount())
          .fieldsGrouping("Parser", "hbasestream", new Fields("deviceid")).setNumTasks(spoutConfig.getPartitionCount());

    これで、HBase ボルトが有効になります。
    > [AZURE.NOTE] HBase ボルトは、ローカルでテストしている場合ではなく、Storm クラスターにデプロイしている場合にのみ有効にする必要があります。

### HBase  データと Storm データ

トポロジを実行する前に、データを受け入れる HBase を準備する必要があります。

1. リモート デスクトップを使用して HBase クラスターに接続します。

2. デスクトップで HDInsight コマンド ラインを開始し、次のコマンドを入力します。

    cd HBASE_HOME %
    bin\hbase シェル

3. HBase シェルから、次のコマンドを入力して、センサー データを格納するテーブルを作成します。

    create 'SensorData', 'cf'

4. 次のコマンドを入力して、テーブルにデータが格納されていないことを確認します。

    scan 'SensorData'

処理後のデータと Storm クラスターでトポロジを開始するときに使用できます、 `'SensorData' をスキャン` データが HBase に挿入されたことを確認するには、もう一度コマンドです。


## 次のステップ

ここでは、Storm を使用して、Event Hub からデータを読み取り、SignalR と D3.js を使用して Storm からの情報を外部ダッシュボードに表示する方法について説明しました。 省略可能な手順を使用した場合は、仮想ネットワークで HDInsight を構成する方法と HBase ボルトを使用して、Storm トポロジと HBase 間の通信を構成する方法について説明しました。

* HDinsight での Storm トポロジの例については、次の項目をご覧ください。

    * [HDInsight での Storm に関するトポロジ例](hdinsight-storm-example-topology.md)

* Apache Storm の概要の詳細については、次を参照してください。、  [Apache Storm](https://storm.incubator.apache.org/) サイトです。

* HDInsight での HBase の詳細については、次を参照してください。、 [HDInsight HBase の概要](hdinsight-hbase-overview.md)します。

* Socket.io の詳細については、次を参照してください。、 [socket.io](http://socket.io/) サイトです。

* D3.js の詳細については、次を参照してください。 [D3.js - データ駆動ドキュメント](http://d3js.org/)します。

* Java でトポロジを作成する方法の詳細については、次を参照してください。 [HDInsight で Apache Storm の開発の Java ベース トポロジ](hdinsight-storm-develop-java-topology.md)します。

* .NET でトポロジを作成する方法の詳細については、次を参照してください。 [Visual Studio を使用した HDInsight での Apache Storm の c# を開発トポロジ](hdinsight-storm-develop-csharp-visual-studio-topology.md)します。


[azure-portal]: https://manage.windowsazure.com/ 


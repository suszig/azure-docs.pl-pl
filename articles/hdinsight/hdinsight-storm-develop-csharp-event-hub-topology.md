<properties
   pageTitle="HDInsight で Storm を使用して Event Hubs のイベントを処理する | Microsoft Azure"
   description="Visual Studio で HDInsight Tools for Visual Studio を使用して作成した C# Storm トポロジによって Event Hubs のデータを処理する方法について説明します。"
   services="hdinsight,notification hubs"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>

# HDInsight で Storm を使用して Azure Event Hubs のイベントを処理する (＃C)

Azure Event Hubs では、Web サイト、アプリ、デバイスで発生する大量のデータを処理できます。 Event Hubs スパウトでは、HDInsight で Apache Storm を使用してこのデータをリアルタイムで簡単に分析できます。 また、Event Hubs のボルトを使用して Storm から Event Hub にデータを書き込むこともできます。

このチュートリアルでは、HDInsight Tools for Visual Studio と Event Hubs スパウトおよびボルトを使用して、次の 2 つのハイブリッド C#/Java トポロジを作成する方法について説明します。

* **EventHubWriter**: ランダムにデータを生成し、Event Hubs に書き込む

* **EventHubReader**: Event Hubs からデータを読み取って Azure テーブル ストレージに格納

[AZURE.NOTE] このドキュメントの手順では、Windows ベースの HDInsight クラスターでのみ機能します。 Linux または Windows ベースのクラスターで動作するが、このプロジェクトの Java バージョンを参照してください。 [(Java) で Storm の使用の Azure Event Hubs からイベントを処理する](hdinsight-storm-develop-java-event-hub-topology.md)です。

## 前提条件

* [HDInsight クラスターでの Apache Storm](hdinsight-storm-getting-started.md)

* [Azure Event Hub](../service-bus/service-bus-event-hubs-csharp-ephcs-getstarted.md)

* [Azure .NET SDK](http://azure.microsoft.com/downloads/)

*  [HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

## 完成したプロジェクト

GitHub からは、このチュートリアルで作成したプロジェクトの完全なバージョンをダウンロードできます: [eventhub storm-ハイブリッド](https://github.com/Blackmist/eventhub-storm-hybrid)します。 ただし、このチュートリアルの手順に従って構成設定を指定する必要があります。

> [AZURE.NOTE] 使用する必要があります、完成したプロジェクトを使用すると、 **NuGet パッケージ マネージャー** このソリューションが必要とするパッケージを復元します。

## Event Hubs スパウトとボルト

Event Hubs スパウトとボルトは Java コンポーネントで、これらのコンポーネントを使用して Apache Storm から Event Hubs を簡単に操作できます。 これらのコンポーネントは Java で記述されていますが、HDInsight Tools for Visual Studio を使用して、C# と Java のコンポーネントが混在するハイブリッド トポロジを作成できます。

スパウトとボルトはという名前の 1 つ Java アーカイブ (.jar) ファイルとして配布 **eventhubs-storm-spout-0.9-jar-with-dependencies.jar**します。

### jar ファイルのダウンロード

最新バージョン、 **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** でファイルが含まれている、 <a href="https://github.com/hdinsight/hdinsight-storm-examples" target="_blank">HDInsight Storm examples</a> プロジェクトの下、 **lib** フォルダーです。 ファイルをダウンロードするには、次のいずれかの方法を実行してください。

> [AZURE.NOTE] Apache Storm プロジェクトに組み込むのため、スパウトとボルトが送信されました。 詳細については、GitHub の「<a href="https://github.com/apache/storm/pull/336/files">STORM-583: Initial check-in for storm-event hubs (STORM-583: Storm-event hub の初期チェックイン)</a>」をご覧ください。

* **ZIP ファイルをダウンロード**: から、 <a href="https://github.com/hdinsight/hdinsight-storm-examples" target="_blank">HDInsight Storm examples</a> のサイトで、 **ZIP のダウンロード** プロジェクトを含む .zip ファイルをダウンロードするには、右側のペインにします。

    ![ZIP のダウンロード ボタン](./media/hdinsight-storm-develop-csharp-event-hub-topology/download.png)

    ファイルのダウンロード、アーカイブを抽出すると、ファイルがあります、 **lib** ディレクトリ。

* **プロジェクトを複製します**: があれば <a href="http://git-scm.com/" target="_blank">Git</a> リポジトリをローカルを複製し、ファイルを見つけます次のコマンドを使用して、インストールされている、 **lib** ディレクトリ。

        git clone https://github.com/hdinsight/hdinsight-storm-examples

## Event Hub の構成

Event Hubs は、この例のデータ ソースです。 新しい Event Hub を作成するには、次の手順に従います。

1.  [Azure Classic Portal](https://manage.windowsazure.com), [ **新規** > **Service Bus** > **Event Hub** > **カスタム作成**します。

2.  **新しいイベント ハブの追加** 画面で、入力、 **イベント ハブ名**, を選択、 **地域** 、ハブを作成し、新しい名前空間を作成するか既存のものを選択します。 クリックして、 **矢印** を続行します。

    ![ウィザード ページ 1](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

    > [AZURE.NOTE] 同じ操作を選択する必要があります **場所** を待機時間とコストを減らすために HDInsight サーバーで使用する Storm とします。

2.  **Event Hub の構成** 画面で、入力、 **カウントをパーティション分割** と **メッセージ保有** 値。 この例では、パーティション カウントに 10 を、メッセージ保持に 1 を使用します。 パーティション数を書き留めておきます。この値は後で必要になります。

    ![ウィザード ページ 2](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

3. Event hub が作成された後、名前空間をクリックし、 **Event Hubs**, 、先ほど作成した event hub を選択します。

4. 選択 **構成**, 、次の情報を使用して 2 つの新しいアクセス ポリシーを作成します。

    <table>
    <tr><th>名前</th><th>アクセス許可</th></tr>
    <tr><td>ライター</td><td>送信</td></tr>
    <tr><td>閲覧者</td><td>リッスン</td></tr>
    </table>

    アクセス許可の作成後、選択、 **保存** ページの下部にあるアイコン。 これにより、この Event Hub に対する送信 (ライター) とリッスン (リーダー) に使用する共有アクセス ポリシーが作成されます。

    ![ポリシー](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

5. 使用してポリシーを保存した後、 **共有アクセス キー生成コンポーネント** のキーを取得するページの下部で、 **ライター** と **リーダー** ポリシーです。 後で使用できるように、これらを保存します。

## テーブル ストレージの構成

テーブル ストレージはからテーブル ストレージを簡単に確認できるので、Event Hub から読み取られる値を保持するために使用する Visual studio から **サーバー エクスプ ローラー**します。 新しいテーブル ストレージを作成するには、次の手順に従います。

1.  [Azure Classic Portal](https://manage.windowsazure.com), [ **新規** > **Data Services** > **ストレージ** > **簡易作成**します。

    ![ストレージの簡易作成](./media/hdinsight-storm-develop-csharp-event-hub-topology/storagecreate.png)

2. 入力、 **名前** ストレージ アカウントの選択、 **場所**, 、順にクリック、 **チェック マーク** ストレージ アカウントを作成します。

    > [AZURE.NOTE] 同じ操作を選択する必要があります **場所** 待機時間とコストを削減するには、Event Hubs と HDInsight サーバーでの Storm とします。

3. 新しいストレージ アカウントをプロビジョニングするときは、アカウントを選択し、使用、 **アクセス キーの管理** を取得するページの下部にあるリンク、 **ストレージ アカウント名** と **プライマリ アクセス キー**します。 後で使用するため、この情報を保存します。

    ![アクセス キー](./media/hdinsight-storm-develop-csharp-event-hub-topology/managekeys.png)

## EventHubWriter の作成

このセクションでは、Event Hubs のボルトを使用して Event Hubs にデータを書き込むトポロジを作成します。

1. 最新バージョンの HDInsight Tools for Visual Studio をまだインストールしていない場合は、 <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">「Get started using HDInsight Tools for Visual Studio (HDInsight Tools for Visual Studio を使用する)」</a>.

2. Visual Studio を開き、選択 **ファイル** > **新規** し **プロジェクト**します。

3.  **新しいプロジェクト** 画面で、展開 **インストールされている** > **テンプレート**, を選択して **HDInsight**します。 テンプレートの一覧から選択 **Storm アプリケーション**します。 画面の下部には、次のように入力します。 **EventHubWriter** として、アプリケーションの名前。

    ![image](./media/hdinsight-storm-develop-csharp-event-hub-topology/newproject.png)

4. プロジェクトの作成後、次のファイルが生成されます。

    * **Program.cs**: プロジェクトのトポロジを定義します。 既定では、スパウトとボルト 1 つずつで構成される既定のトポロジが作成されます。

    * **Spout.cs**: スパウトの例です。

    * **Bolt.cs**: ボルトの例です。 Event Hubs のボルトを使用して Event Hub に書き込むため、これは後で削除します。

### 構成

1.  **ソリューション エクスプ ローラー**, を右クリックして **EventHubWriter**, 、し、[ **プロパティ**します。

2. プロジェクトのプロパティで選択 **設定**, 、し、[ **このプロジェクトに既定の設定ファイルが含まれていません。ファイルを作成するには、ここをクリックしてください。]** を選択します。

3. 次の設定を入力します。 前の手順で作成した Event Hub の情報を使用して、 **値** 列です。

    <table>
    <tr><th style="text-align:left">名前</th><th style="text-align:left">型</th><th style="text-align:left">Scope</th></tr>
    <tr><td style="text-align:left">EventHubPolicyName</td><td style="text-align:left">string</td><td style="text-align:left">アプリケーション</td></tr>
    <tr><td style="text-align:left">EventHubPolicyKey</td><td style="text-align:left">文字列</td><td style="text-align:left">アプリケーション</td></tr>
    <tr><td style="text-align:left">EventHubNamespace</td><td style="text-align:left">string</td><td style="text-align:left">アプリケーション</td></tr>
    <tr><td style="text-align:left">EventHubName</td><td style="text-align:left">string</td><td style="text-align:left">アプリケーション</td></tr>
    <tr><td style="text-align:left">EventHubPartitionCount</td><td style="text-align:left">int</td><td style="text-align:left">アプリケーション</td></tr>
    </table>

4. 保存して閉じる、 **プロパティ** ページです。

### トポロジの定義

1.  **ソリューション エクスプ ローラー**, を右クリックして **Bolt.cs**, を選択して **削除**します。 Java Event Hubs のボルトを使用するので、このファイルは不要です。

2. 開いている、 **Program.cs** ファイルし、次の追加後すぐにコード、 `TopologyBuilder topologyBuilder = new TopologyBuilder("EventHubWriter");` 行です。

        int partitionCount = Properties.Settings.Default.EventHubPartitionCount;
        List<string> javaDeserializerInfo =
            new List<string>() { "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer", "java.lang.String" };

    最初の行は、先ほど定義したプロパティのパーティション数です。 2 行目は、スパウトによって生成される JSON データを Java コンポーネントで使用できるように、`java.lang.String` にシリアル化解除するデシリアライザーを定義します。

4. 次のコードを見つけます。

        topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"count"}}
            },
            1);

    これを次のコードに置き換えます。

        topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"Event"}}
            },
            partitionCount).
            DeclareCustomizedJavaDeserializer(javaDeserializerInfo);

    このコードは、スパウトを作成し、このコンポーネントの並列処理のヒントとして Event Hubs パーティション数を使用します。 このコードは、各パーティションのスパウトのインスタンスを作成します。

    また、これにより、先ほど作成したデシリアライザーはこのコンポーネントからの出力ストリームに関連付けられます。 これにより、ダウンストリームの EventHubSpout コンポーネントで C# スパウトから生成されたデータを使用できます。

5. 前のコードの直後に、次のコードを追加します。

        JavaComponentConstructor constructor =
            JavaComponentConstructor.CreateFromClojureExpr(
            String.Format(@"(com.microsoft.eventhubs.bolt.EventHubBolt. (com.microsoft.eventhubs.bolt.EventHubBoltConfig. " +
            @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
            Properties.Settings.Default.EventHubPolicyName,
            Properties.Settings.Default.EventHubPolicyKey,
            Properties.Settings.Default.EventHubNamespace,
            "servicebus.windows.net", //suffix for servicebus fqdn
            Properties.Settings.Default.EventHubName,
            "true"));

    このコードは、Java ボルトの新しいコンストラクターを作成します。このコンストラクターは、ボルトの新しいインスタンスを構成するために実行時に使用されます。 この場合は、使用している、 <a href="http://storm.apache.org/documentation/Clojure-DSL.html" target="_blank">Apache Storm Clojure DSL</a> Event Hubs 構成情報をスパウトに構成する前に追加しました。 具体的には、このコードは、次のことを行うために HDInsight によって実行時に使用されます。

    * 新しいインスタンスを作成 **com.microsoft.eventhubs.bolt.EventHubBoltConfig** 指定した Event Hubs 情報を使用します。
    * 新しいインスタンスを作成 **com.microsoft.eventhubs.bolt.EventHubBolt**, を渡すことで、 **EventHubBoltConfig** インスタンス。

6. 次のコードを見つけます。

        topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            1).shuffleGrouping("Spout");

    これを次のコードに置き換えます。

        topologyBuilder.SetJavaBolt(
            "EventHubBolt",
            constructor,
            partitionCount).
            shuffleGrouping("Spout");

    これは、トポロジで使用するよう指示、 **JavaComponentConstructor** をボルトとして前の手順です。 このトポロジのコンポーネントは、"EventHubBolt" というわかりやすい名前で参照できます。 並行処理のヒントには Event Hub のパーティション数が設定され、スパウトによって生成されるデータ ("Spout") にサブスクライブします。

この時点では、が完了したら、 **Program.cs**します。 トポロジを定義するが、ここで変更する必要があります **Spout.cs** Event Hubs のボルトが使用できる形式でデータを生成できるようにします。

> [AZURE.NOTE] このトポロジは既定の例として十分な 1 つのワーカー プロセスを作成します。 これを運用環境のクラスターに適用する場合、次を追加して、ワーカーの数を変更する必要があります。

    StormConfig config = new StormConfig();
    config.setNumWorkers(1);
    topologyBuilder.SetTopologyConfig(config);


### スパウトを変更する

Event Hubs のボルトは、Event Hub にルーティングされる単一の文字列値を予期しています。 次の例では、既定値を変更して **Spout.cs** JSON 文字列を生成するファイルです。

1.  **ソリューション エクスプ ローラー**, 、開かれている **Spout.cs** 、ファイルの先頭に次のコードを追加します。

        using Newtonsoft.Json;
        using Newtonsoft.Json.Linq;

    これにより、JSON データをより簡単に処理できます。
    
    > [AZURE.NOTE] JSON.NET パッケージが既にインストールされている、c# Storm トポロジに使用される SCP.NET フレームワークが必要です。

3. 次のコードを見つけます。

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        outputSchema.Add("default", new List<Type>() { typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));

    これを次のコードに置き換えます。

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer());

    これを使用するスパウトによって作成されるデータの定義が変更 **文字列** データ、および **CustomizedInteropJSONSerializer** トポロジ (program.cs) 内で宣言されました。

2. 置き換える、 **NextTuple** を次のメソッド。

        public void NextTuple(Dictionary<string, Object> parms)
        {
            JObject eventData = new JObject();
            eventData.Add("deviceId", r.Next(10));
            eventData.Add("deviceValue", r.Next());
            ctx.Emit(new Values(eventData.ToString(Formatting.None)));
        }

    このコードは、デバイス ID である値をランダムに生成し、Json.NET を使って、これらの値を使用する JSON オブジェクトを出力します。

3. 保存、 **Spout.cs** ファイルです。

ここまでで、ランダムなデータを生成し、生成したデータを、Event Hubs のボルトを使用して Event Hubs に格納する基本的なトポロジを作成しました。 次にリーダーを作成します。

## EventHubReader の作成

このセクションでは、Event Hubs スパウトを使用して Event Hubs からデータを読み取るトポロジを作成します。

2. Visual Studio を開き、選択 **ファイル** > **新規** > **プロジェクト**します。

3.  **新しいプロジェクト** 画面で、展開 **インストールされている** > **テンプレート**, を選択して **HDInsight**します。 テンプレートの一覧から選択 **Storm アプリケーション**します。 画面の下部には、次のように入力します。 **EventHubReader** として、アプリケーションの名前。

### 構成

1.  **ソリューション エクスプ ローラー**, を右クリックして **EventHubReader** し、[ **プロパティ**します。

2. プロジェクトのプロパティで選択 **設定**, 、し、[ **このプロジェクトに既定の設定ファイルが含まれていません。ファイルを作成するには、ここをクリックしてください。]** を選択します。

3. 次の設定を入力します。 作成した Event Hub とストレージ アカウントの情報を使用、 **値** 列です。

    <table>
    <tr><th style="text-align:left">名前</th><th style="text-align:left">型</th><th style="text-align:left">Scope</th></tr>
    <tr><th style="text-align:left">EventHubPolicyName</th><th style="text-align:left">string</th><th style="text-align:left">アプリケーション</th></tr>
    <tr><th style="text-align:left">EventHubPolicyKey</th><th style="text-align:left">文字列</th><th style="text-align:left">アプリケーション</th></tr>
    <tr><th style="text-align:left">EventHubNamespace</th><th style="text-align:left">string</th><th style="text-align:left">アプリケーション</th></tr>
    <tr><th style="text-align:left">EventHubName</th><th style="text-align:left">string</th><th style="text-align:left">アプリケーション</th></tr>
    <tr><th style="text-align:left">EventHubPartitionCount</th><th style="text-align:left">int</th><th style="text-align:left">アプリケーション</th></tr>
    <tr><th style="text-align:left">StorageConnection</th><th style="text-align:left">(接続文字列)</th><th style="text-align:left">アプリケーション</th></tr>
    <tr><th style="text-align:left">TableName</th><th style="text-align:left">string</th><th style="text-align:left">アプリケーション</th></tr>
    </table>

     **TableName**, 、イベントを格納するテーブルの名前を入力します。

     **StorageConnection**, の値を入力 `DefaultEndpointsProtocol=https;AccountName=myAccount;AccountKey=myKey;`します。 置換 **myAccount** と **myKey** ストレージ アカウント名と先に取得したキーを使用します。

    これらの値は、Event Hubs と Table Storage と通信するために、トポロジによって使用されます。

4. 保存して閉じる、 **プロパティ** ページです。

### トポロジの定義

1.  **ソリューション エクスプ ローラー**, を右クリックして **Spout.cs** 選択 **削除**します。 Java Event Hubs スパウトを使用するので、このファイルは不要です。

2. 開いている、 **Program.cs** ファイルし、次の追加後すぐにコード、 `TopologyBuilder topologyBuilder = new TopologyBuilder("EventHubReader");` 行。

        int partitionCount = Properties.Settings.Default.EventHubPartitionCount;
        EventHubSpoutConfig ehConfig = new EventHubSpoutConfig(
                Properties.Settings.Default.EventHubPolicyName,
                Properties.Settings.Default.EventHubPolicyKey,
                Properties.Settings.Default.EventHubNamespace,
                Properties.Settings.Default.EventHubName,
                partitionCount);

    パーティション数が読み込まれ、ローカル変数に割り当てられます。 これは複数回使用されます。

    `EventHubSpoutConfig` は、Event Hub スパウトの構成を定義します。 この場合の Event Hubs の構成は、以前に追加しました。 バック グラウンドで Java Event Hub スパウトを使用するとこの新しいインスタンスを作成 **com.microsoft.eventhubs.spout.EventHubSpoutConfig** Event Hubs 情報を使用します。

5. 次のコードを見つけます。

        topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"count"}}
            },
            1);

    これを次のコードに置き換えます。

        topologyBuilder.SetEventHubSpout(
            "EventHubSpout", 
            ehConfig, 
            partitionCount); 

    これにより、トポロジに新しい Event Hub スパウトを作成し、前の手順から構成として `EventHubSpoutConfig` を使用するよう指示します。 "EventHubSpout" は、スパウトの表示名を設定し、`partitionCount` は、並列処理のヒントを設定するために使用します。 バック グラウンドでの新しいインスタンスを作成 **com.microsoft.eventhubs.spout.EventHubSpout** 指定した構成情報を使用して、Java コンポーネントです。

2. 前のコードの直後に、次のコードを追加します。

         List<string> javaSerializerInfo = new List<string>() { "microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer" };

    これにより、Java コンポーネント (EventHubSpout など) によって生成された情報を、ダウンストリームの C# コンポーネントで使用できる JSON 形式へのシリアル化に使用されるカスタムのシリアライザーを作成します。

3. 次のコードを見つけます。

        topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            1).shuffleGrouping("Spout");

    これを次のコードに置き換えます。

        topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            partitionCount,
            true).
            DeclareCustomizedJavaSerializer(javaSerializerInfo).
            shuffleGrouping("EventHubSpout");

    このコードは、トポロジでのボルト (Bolt.cs で定義) の使用を指示します。 先ほど定義したカスタムのシリアライザーをここで使用し、このボルトがアップストリームの Java コンポーネントで生成されたデータを使用できるようにします。 この場合、EventHubSpout です。

    > [AZURE.IMPORTANT] SetBolt の最後のパラメーター (値は `true`,、) このボルトの ACK 機能を有効にします。 これは、EventHubSpout コンポーネントでは、出力されるデータの ACK を想定するために必要です。 ダウンストリームのコンポーネントで ACK が返されない場合、スパウトは約 1000 件のメッセージを処理した後に受信を停止します。

この時点が完了した **Program.cs**します。 トポロジを定義し、テーブル ストレージにデータを書き込むヘルパー クラスを作成する必要がありますが、変更する必要があります **Bolt.cs** 、スパウトによって生成されたデータを認識できるようにします。

> [AZURE.NOTE] このトポロジは既定の例として十分な 1 つのワーカー プロセスを作成します。 これを運用環境のクラスターに適用する場合、次を追加して、ワーカーの数を変更する必要があります。

    StormConfig config = new StormConfig();
    config.setNumWorkers(1);
    topologyBuilder.SetTopologyConfig(config);


### ヘルパー クラスの作成

テーブル ストレージにデータを書き込む場合、書き込まれるデータを記述したクラスを作成する必要があります。

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **EventHubReader** プロジェクト、および選択 **追加**, 、し **クラスの新しい**します。 新しいクラスの名前を **Devices.cs**します。

2. 開いている **Devices.cs** 既定のコードを次に置き換えます。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.Storage.Table;

        namespace EventHubReader
        {
            class Device : TableEntity
            {
                public int value { get; set; }

                public Device() { }
                public Device(int id)
                {
                    this.PartitionKey = id.ToString();
                    this.RowKey = System.Guid.NewGuid().ToString();
                }
            }
        }

    このコードは、テーブル ストレージに、パーティション キー (Event Hub から読み取られたデバイス ID に設定される)、一意の行キー、Event Hub から読み取られた値で構成されるエンティティを作成します。 また、各エンティティがテーブルに挿入された時点で、エンティティのタイムスタンプが自動的に作成されます。

### ボルトの変更

1.  **ソリューション エクスプ ローラー**, 、展開、 **EventHubReader** 開き、プロジェクト、 **Bolt.cs** ファイルです。 ファイルの先頭に次のコードを追加します。

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;

    これにより、ボルトからの JSON データの操作がより簡単になり、テーブル ストレージにデータを書き込めるようになります。

2. `private int count;` ステートメントを見つけて次の内容に置き換えます。

        private CloudTable table;

    これは、テーブルに接続するときに使用されます。

4. 次のコードを見つけます。

        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        inputSchema.Add("default", new List<Type>() { typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));

    これを次のコードに置き換えます。

        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
        this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());

    受信するボルトこれにより、 **文字列** 値の代わりに、 **int**, を使用してデータを逆シリアル化して、 **CustomizedInteropJSONDeserialzer** トポロジ前 (program.cs ファイル) 内で宣言されています。

3. 前のコードの直後に、次のコードを追加します。

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(Properties.Settings.Default.StorageConnection);
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
        table = tableClient.GetTableReference(Properties.Settings.Default.TableName);
        table.CreateIfNotExists();

    これに接続、 **イベント** テーブルで構成した接続文字列を使用しています。 テーブルが存在しない場合は、作成されます。

2. 検索、 **Execute** メソッドを次に置き換えます。

        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Processing events");
            string eventValue = (string)tuple.GetValue(0);
            if (eventValue != null)
            {
                JObject eventData = JObject.Parse(eventValue);

                Device device = new Device((int)eventData["deviceId"]);
                device.value = (int)eventData["deviceValue"];

                TableOperation insertOperation = TableOperation.Insert(device);

                table.Execute(insertOperation);
                this.ctx.Ack(tuple);
            }
        }

    これは、Json.NET を使用して、スパウトからの JSON データの解析し、し、 **deviceId** と **deviceValue** フィールドです。 新しい **デバイス** を使用してオブジェクトが作成され、 **deviceId** 初期化時に、テーブルのパーティション キーを設定します。 値に設定し、 **deviceValue**, 、し、最後にエンティティをテーブルに挿入します。

    エンティティがテーブルに挿入された後、タプルに `Ack()` が呼び出され、データが正常に処理されたことをスパウトに通知します。

    > [AZURE.IMPORTANT] EventHubSpout コンポーネントは、このボトルなどのダウン ストリーム コンポーネントから各タプルに ACK を想定しています。 ACK を受信しない場合、EventHubSpout はタプルの処理が失敗したものと見なします。

この時点では、Event Hub からデータを読み取り、という名前のテーブルのテーブル ストレージに保存する完全なトポロジがある **イベント**です。

## トポロジのデプロイ

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **EventHubReader** プロジェクトし、選択 **HDInsight での Storm に送信**します。

    ![Storm に送信](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2.  **トポロジの送信** 画面で、[、 **Storm クラスター**します。 展開 **追加の構成**, を選択 **Java ファイル パス**, 、[ **...** を含むディレクトリを選択し、 **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** 先ほどダウンロードしたファイルです。 最後に、クリックして **送信**します。

    ![送信ダイアログの画像](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. トポロジが送信されると、 **Storm トポロジ ビューアー** が表示されます。 選択、 **EventHubReader** 、トポロジの統計を表示するのには、左側のウィンドウ内のトポロジです。 Event Hubs に書き込まれたイベントはまだないため、現時点では、何も発生していません。

    ![ストレージ ビューの例](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4.  **ソリューション エクスプ ローラー**, を右クリックし、 **EventHubWriter** プロジェクトし、選択 **HDInsight での Storm に送信**します。

2.  **トポロジの送信** 画面で、[、 **Storm クラスター**します。 展開 **追加の構成**, を選択 **Java ファイル パス**, [ **...** を含むディレクトリを選択し、 **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** 既にダウンロードしたファイルです。 最後に、クリックして **送信**します。

5. トポロジが送信されると、更新でトポロジ一覧、 **Storm トポロジ ビューアー** 両方のトポロジがクラスターで実行されていることを確認します。

6. 両方のトポロジが実行されている選択 **サーバー エクスプ ローラー**, 、順に展開 **Azure** > **ストレージ**, 、先ほど作成したストレージ アカウントを選択します。 ストレージ アカウントでは、展開 **テーブル**します。 最後をダブルクリックして、 **イベント** テーブルをテーブルを開きます。 テーブルに格納されているデータが表示される、 **EventHubReader** トポロジです。

    * によって、イベントが生成された、 **EventHubWriter** トポロジでは、Event Hub に書き込まれます。

    *  **EventHubReader** し、Event Hubs からイベントを読み取りでテーブル ストレージに格納、 **イベント** テーブルです。

## トポロジの停止

トポロジを停止するで各トポロジを選択、 **Storm トポロジ ビューアー**, 、] をクリックし、 **Kill**します。

![トポロジの強制終了の画像](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## メモ

### チェックポイント機能

EventHubSpout は Zookeeper ノードに対する状態へのチェックポイントを定期的に設定し、キューから読み取ったメッセージの現在のオフセットを保存します。 これにより、コンポーネントは次のシナリオで保存されたオフセットでメッセージの受信を開始できます。

* コンポーネントのインスタンスは失敗し、再起動されます。

* ノードを追加または削除して、クラスターを拡張または圧縮します。

* トポロジが強制終了され、再起動 **同名**します。

また、保存されたチェックポイントを WASB (HDInsight クラスターで使用される Azure のストレージ) にエクスポート、インポートできます。これを行うスクリプトは、HDInsight クラスターの Storm でにある、 **c:\apps\dist\storm-0.9.3.2.2.1.0-2340\zkdatatool-1.0\bin**します。

>[AZURE.NOTE] パスのバージョン番号は、Storm クラスターへのインストールのバージョンが、今後変わる可能性が異なる、可能性があります。

このディレクトリのスクリプトは次のとおりです。

* **stormmeta_import.cmd**: クラスターの既定のストレージ コンテナーから Zookeeper にすべての Storm メタデータをインポートします。

* **stormmeta_export.cmd**: Zookeeper からクラスターの既定のストレージ コンテナーにすべての Storm メタデータをエクスポートします。

* **stormmeta_delete.cmd**: Zookeeper からすべての Storm メタデータを削除します。

インポートのエクスポートにより、クラスターを削除する必要がある一方で、新しいクラスターを再びオンラインにする際にハブの現在のオフセットから処理を再開する場合、チェックポイントのデータを保持できます。

> [AZURE.NOTE] 既定のストレージ コンテナーは、新しいクラスターにデータが保存されるため **必要があります** 以前のクラスターと同じストレージ アカウントとコンテナーを使用します。

## 概要

このドキュメントでは、C# トポロジから Java Event Hubs スパウトおよびボルトを使用して、Azure Event Hub のデータを操作する方法について説明しました。 C# トポロジの作成の詳細については、次の記事を参照してください。

* [Visual Studio を使用して HDInsight で Apache Storm の C# トポロジを開発する](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)
 


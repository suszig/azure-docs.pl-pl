<properties
   pageTitle="Visual Studio および c# での Apache Storm トポロジ |Microsoft Azure"
   description="HDInsight Tools for Visual Studio を使用して、Visual Studio で簡単なワード カウントトポロジを作成することで、C# で Storm トポロジを作成する方法を説明します。"
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
   ms.date="12/04/2015"
   ms.author="larryfr"/>

# Hadoop Tools for Visual Studio を使用した HDInsight での Apache Storm の C# トポロジの開発

HDInsight tools for Visual Studio を使用して C# Storm トポロジを作成する方法を説明します。 このチュートリアルでは、Visual Studio で新しい Storm プロジェクトを作成し、ローカルでテストして、HDInsight クラスターで Apache Storm にデプロイする手順について説明します。

また、C# と Java コンポーネントを使用するハイブリッド トポロジの作成方法についても説明します。

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##前提条件

-   下記いずれかのバージョンの Visual Studio

    -   Visual Studio 2012 ([Update 4](http://www.microsoft.com/download/details.aspx?id=39305))

    -   Visual Studio 2013 ([Update 4](http://www.microsoft.com/download/details.aspx?id=44921)) または [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)

    -   Visual Studio 2015 または [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

-   Azure SDK 2.5.1 以降

-   HDInsight Tools for Visual Studio - HDInsight Tools for Visual Studio のインストールと構成については、「[HDInsight Tools for Visual Studio の使用開始](hdinsight-hadoop-visual-studio-tools-get-started.md)」をご覧ください。

    > [AZURE.NOTE] HDInsight Tools for Visual Studio は Visual Studio Express でサポートされていません

-   HDInsight 上の Apache Storm クラスター - クラスターを作成する手順については、「[HDInsight での Apache Storm の使用](hdinsight-storm-getting-started.md)」をご覧ください。

    > [AZURE.NOTE] 現時点では、HDInsight Tools for Visual Studio では、HDInsight バージョン 3.2 cluster の Storm のみがサポートされます。

##テンプレート

HDInsight Tools for Visual Studio には次のテンプレートがあります。

| プロジェクトの種類 | 対象 |
| ------------ | ------------- |
| Storm アプリケーション | 空の Storm トポロジ プロジェクト |
| Storm Azure SQL ライターのサンプル | Azure SQL データベースに書き込む方法 |
| Storm DocumentDB リーダーのサンプル | Azure DocumentDB から読み取る方法 |
| Storm DocumentDB ライターのサンプル | Azure DocumentDB を記述する方法 |
| Storm EventHub リーダーのサンプル | Azure Event Hubs から読み取る方法 |
| Storm EventHub ライターのサンプル | Azure Event Hubs に記述する方法 |
| Storm HBase リーダーのサンプル | HDInsight クラスターで HBase から読み取る方法 |
| Storm HBase ライターのサンプル | HDInsight クラスターで HBase に記述する方法 |
| Storm ハイブリッドのサンプル | Java コンポーネントを使用する方法 |
| Storm サンプル | 基本的なワード カウント トポロジ |

> [AZURE.NOTE] HBase のリーダーとライターのサンプルでは、HBase REST API を使用して、HBase Java API ではない、HDInsight クラスターの HBase と通信します。

このドキュメントの手順では、基本的な種類の Storm アプリケーション プロジェクトを使用して新しいトポロジを作成します。

##C# トポロジの作成

1.  HDInsight Tools for Visual Studio の最新バージョンをまだインストールしていない場合は、「[HDInsight Tools for Visual Studio の使用開始](hdinsight-hadoop-visual-studio-tools-get-started.md)」をご覧ください。

2.  Visual Studio を開き、選択 **ファイル** > **新規**, 、し **プロジェクト**します。

3.   **新しいプロジェクト** 画面で、展開 **インストールされている** > **テンプレート**, を選択して **HDInsight**します。 テンプレートの一覧から選択 **Storm アプリケーション**します。 画面の下部には、次のように入力します。 **WordCount** として、アプリケーションの名前。

    ![image](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4.  プロジェクトの作成後、次のファイルが生成されます。

    -   **Program.cs**: プロジェクトのトポロジを定義します。 既定では、スパウトとボルト 1 つずつで構成される既定のトポロジが作成されます。

    -   **Spout.cs**: ランダムな数字を出力するスパウトの例です。

    -   **Bolt.cs**: スパウトが出力する数字の数をカウントするサンプル ボルトです。

    プロジェクト作成の過程で最新の [SCP.NET パッケージ](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/)が NuGet からダウンロードされます。

次のセクションでは、このプロジェクトを基本的な WordCount アプリケーションに変更します。

###スパウトを実装する

1.  開いている **Spout.cs**します。 スパウトは、外部ソースからデータをトポロジに読み込む際に使用されます。 スパウトの主なコンポーネントは次のとおりです。

    -   **NextTuple**: 新しいタプルを出力するスパウトが許可されている場合は、Storm によって呼び出されます。

    -   **Ack** (トランザクションのトポロジのみ): このスパウトから送信されたタプルのトポロジの他のコンポーネントによって開始される受信確認を処理します。 タプルの受信確認によって、ダウンストリーム コンポーネントで処理が正常に完了したことがスパウトに通知されます。

    -   **失敗** (トランザクションのトポロジのみ): 処理に失敗トポロジの他のコンポーネントしたタプルを処理します。 これにより、再処理用のタプルを再出力できるようになります。

2.  内容を置き換える、 **スパウト** を次のクラスです。 これにより、トポロジにセンテンスをランダムに出力するスパウトが作成されます。

    ```
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };


    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;


        Context.Logger.Info("Generator constructor called");


        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }


    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }


    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
        string sentence;


        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));


        Context.Logger.Info("NextTuple exit");
    }


    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }


    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

    コメントに目を通して、このコードが行う内容をご確認ください。

###ボルトを実装する

1.  既存の **Bolt.cs** プロジェクトからのファイルです。

2.   **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **追加** > **[新しい項目の**です。 リストから、次のように選択します。 **Storm Bolt**, 、入力と **Splitter.cs** 名として。 2 つ目のボルトを作成するという名前を繰り返し **Counter.cs**します。

    -   **Splitter.cs**: 文章を個々 の単語に分割し、新しい単語のストリームを出力するボルトを実装します。

    -   **Counter.cs**: 新しい各単語をカウントし、単語のストリームを出力して各単語をカウントするボルトを実装します。

    > [AZURE.NOTE] これらのボルトは単に読み取りおよび、ストリームは、書き込みが、ボルトを使用してデータベースまたはサービスなどのソースと通信することもできます。

3.  開いている **Splitter.cs**します。 既定で 1 つのメソッドがあります: **Execute**します。 これは、ボルトが処理のタプルを受信したときに呼び出されます。 ここで、受信したタプルを読み取って処理し、送信タプルを出力できます。

4.  内容を置き換える、 **スプリッター** クラスを次のコードとします。

    ```
    private Context ctx;


    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;


        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }


    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }


    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");


        // Get the sentence from the tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }


        Context.Logger.Info("Execute exit");
    }
    ```

    コメントに目を通して、このコードが行う内容をご確認ください。

5.  開いている **Counter.cs** クラスの内容を次に置き換えます。

    ```
    private Context ctx;


    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();


    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;


        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });


        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }


    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }


    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");


        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;


        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));


        Context.Logger.Info("Execute exit");
    }
    ```

    コメントに目を通して、このコードが行う内容をご確認ください。

###トポロジの定義

スパウトとボルトはコンポーネント間のデータ フローを定義するグラフに配置されます。 このトポロジのグラフは次のようになります。

![コンポーネントの配置方法の画像](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

センテンスはスパウトから出力され、Splitter ボルトのインスタンスに配布されます。 Splitter ボルトがセンテンスを単語に分け、Counter ボルトに配布します。

ワード カウントは Counter インスタンスにローカルに保持されるため、特定の 1 つの単語が同じ Counter ボルト インスタンスに届くようにして、その単語を追跡するインスタンスが複数発生しないようにする必要があります。 ただし、Splitter ボルトの場合はどのボルトがどのセンテンスを受信しても問題ないため、インスタンス全体の負荷の分散のみを考慮します。

開いている **Program.cs**します。 重要なメソッドは **ITopologyBuilder**, 、これは Storm に送信されるトポロジの定義に使用します。 内容を置き換える **ITopologyBuilder** を前述のトポロジを実装する次のコード。

```
    // Create a new topology named 'WordCount'
    TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount");

    // Add the spout to the topology.
    // Name the component 'sentences'
    // Name the field that is emitted as 'sentence'
    topologyBuilder.SetSpout(
        "sentences",
        Spout.Get,
        new Dictionary<string, List<string>>()
        {
            {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
        },
        1);
    // Add the splitter bolt to the topology.
    // Name the component 'splitter'
    // Name the field that is emitted 'word'
    // Use suffleGrouping to distribute incoming tuples
    //   from the 'sentences' spout across instances
    //   of the splitter
    topologyBuilder.SetBolt(
        "splitter",
        Splitter.Get,
        new Dictionary<string, List<string>>()
        {
            {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
        },
        1).shuffleGrouping("sentences");

    // Add the counter bolt to the topology.
    // Name the component 'counter'
    // Name the fields that are emitted 'word' and 'count'
    // Use fieldsGrouping to ensure that tuples are routed
    //   to counter instances based on the contents of field
    //   position 0 (the word). This could also have been
    //   List<string>(){"word"}.
    //   This ensures that the word 'jumped', for example, will always
    //   go to the same instance
    topologyBuilder.SetBolt(
        "counter",
        Counter.Get,
        new Dictionary<string, List<string>>()
        {
            {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
        },
        1).fieldsGrouping("splitter", new List<int>() { 0 });

    // Add topology config
    topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
    {
        {"topology.kryo.register","[\"[B\"]"}
    });

    return topologyBuilder;
```

コメントに目を通して、このコードが行う内容をご確認ください。

##トポロジを送信する

1.   **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **HDInsight での Storm に送信**します。

    > [AZURE.NOTE] メッセージが表示されたら、Azure サブスクリプションのログイン資格情報を入力します。 2 つ以上のサブスクリプションをお持ちの場合は、HDInsight クラスターの Storm があるサブスクリプションにログインします。

2.  HDInsight クラスターの Storm を選択して、 **Storm クラスター** クリックしてドロップダウン リスト **送信**します。 使用して、送信が成功した場合に監視することができます、 **出力** ウィンドウです。

3.  トポロジが正常に送信されたときに、 **Storm トポロジ** クラスターを表示する必要があります。 選択、 **WordCount** 実行中のトポロジに関する情報を表示するには、一覧からトポロジです。

    > [AZURE.NOTE] 表示することも **Storm トポロジ** から **サーバー エクスプ ローラー**: 展開 **Azure** > **HDInsight**, 、HDInsight クラスターの Storm を右クリックし、[ **View Storm Topologies**します。

    スパウトやボルトのリンクを使用してこれらのコンポーネントに関する情報を表示します。 各アイテムを選択すると新しいウィンドウが開きます。

4.   **トポロジの概要** ビューで、[ **Kill** トポロジを停止します。

    > [AZURE.NOTE] Storm トポロジは、これらが非アクティブ化、または、クラスターが削除されるまで実行し続けます。

##トランザクションのトポロジ

前のトポロジは、トランザクションではなく、 トポロジ内のコンポーネントはトポロジのコンポーネントによって処理が失敗した場合のリプレイ メッセージに機能を実装することはありません。 トランザクション トポロジの例、新しいプロジェクトを作成し、選択 **Storm サンプル** プロジェクトの種類として。

トランザクションのトポロジは次の内容を実装してデータのリプレイをサポートします。

-   **メタデータ キャッシュ**: スパウトは、データを取得し、障害が発生した場合、再生成されるように出力されたデータに関するメタデータを保存する必要があります。 サンプルが出力するデータは少量であるため、各タプルの生データはリプレイのディクショナリに保存されます。

-   **Ack**: トポロジ内の各ボルトは呼び出すことができます `this.ctx.Ack(tuple)` 確認することが正常に処理されたタプルです。 すべてのボルトでタプルを確認したら、スパウトの `Ack` メソッドが呼び出されます。 これで、データが完全に処理されたため、スパウトはリプレイのキャッシュされたデータを削除できます。

-   **失敗**: 各ボルトは呼び出すことができます `this.ctx.Fail(tuple)` をタプルの処理が失敗したことを示します。 エラーがスパウトの `Fail` メソッドに伝達され、そこでキャッシュされたメタデータを使用してタプルをリプレイされます。

-   **Sequence ID**: タプルの出力時、シーケンス ID を指定できます。 これは、リプレイ (Ack と Fail) 処理するタプルを示す値である必要があります。 スパウトなど、 **Storm サンプル** プロジェクトは、データを生成するときに、次を使用します。

    ```
    this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);
    ```

    これで、センテンスを含む既定のストリームに含まれるシーケンス ID 値を持つ新しいタプルが、 **lastSeqId**します。 たとえば、 **lastSeqId** は出力されるタプルごとに増加します。

説明するよう、 **Storm サンプル** プロジェクトで、コンポーネントがトランザクションかどうかに設定できますが、実行時の構成に基づいています。

##ハイブリッド トポロジ

また、HDInsight tools for Visual Studio は、C# のコンポーネントと Java のコンポーネントが混在するハイブリッド トポロジの作成に使用できます。

ハイブリッド トポロジの例は、[新しいプロジェクトの作成を選択します **Storm ハイブリッド サンプル**します。 これで、次を示す複数のトポロジを含む、完全にコメントされたサンプルが作成されます。

-   **Java スパウト** と **c# ボルト**: で定義されている **HybridTopology_javaSpout_csharpBolt**

    -   定義されたトランザクション バージョン **HybridTopologyTx_javaSpout_csharpBolt**

-   **C# スパウト** と **Java ボルト**: で定義されている **HybridTopology_csharpSpout_javaBolt**

    -   定義されたトランザクション バージョン **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] このバージョンでは、Clojure コードをテキスト ファイルから Java コンポーネントとして使用する方法も示します。

プロジェクトの送信時に使用されるトポロジを切り替えるには、単純にクラスターの送信前に `[Active(true)]` のステートメントを使用するトポロジに移動します。

> [AZURE.NOTE] 必要とされるすべての Java ファイルは、このプロジェクトの一部として提供される、 **JavaDependency** フォルダーです。

ハイブリッド トポロジの作成と送信時には、次のものが使用されます。

-   **JavaComponentConstructor** やボルト スパウトの Java クラスの新しいインスタンスを作成に使用する必要があります。

-   **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** または Java オブジェクトから JSON への Java コンポーネント内のデータをシリアル化に使用する必要があります。

-   使用する必要があります、サーバーのトポロジを送信するときに、 **追加の構成** を指定するオプション、 **Java ファイルのパス**します。 ここで指定するパスは、Java クラスを含む JAR ファイルのあるディレクトリである必要があります。

###Azure Event Hubs

SCP.Net バージョン 0.9.4.203 には、Event Hub スパウト (Event Hub から読み取る Java スパウト) の操作専用に新しいクラスとメソッドが導入されています。このスパウトを使用するトポロジを作成する場合は、次のメソッドを使用します。

-   **EventHubSpoutConfig** クラス: スパウト コンポーネントの構成を格納するオブジェクトを作成

-   **TopologyBuilder.SetEventHubSpout** メソッド: トポロジに Event Hub スパウト コンポーネントを追加

> [AZURE.NOTE] これらによって、その他の Java コンポーネントよりも Event Hub スパウトを容易に操作が、スパウトによって生成されたデータをシリアル化するのに、CustomizedInteropJSONSerializer を使用する必要があります。

##SCP.NET を更新する方法

SCP.NET の最新リリースでは、NuGet からパッケージをアップグレードできます。 新しい更新プログラムが利用できる場合、アップグレードの通知が表示されます。 手動でアップグレードを確認するには、次の手順に従います。

1.  **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **NuGet パッケージの管理**します。

2. パッケージ マネージャーから選択 **更新**します。 更新プログラムが利用できる場合、それが一覧表示されます。 クリックして、 **更新** パッケージをインストールする] ボタンをクリックします。

> [AZURE.IMPORTANT] SCP.NET パッケージの更新に NuGet を使用していないのは、以前のバージョンのいずれかで、プロジェクトが作成されている場合は、次の手順に従って、新しいバージョンに更新を行う必要があります。
>
> 1.  **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **NuGet パッケージの管理**します。
> 2. 使用して、 **検索** フィールドを検索し、追加し、 **Microsoft.SCP.Net.SDK** をプロジェクトにします。

##トラブルシューティング

###トポロジをローカルでテストする

トポロジのクラスターへのデプロイは簡単ですが、状況によっては、トポロジをローカルでテストする必要が生じる場合があります。 次の手順を使用して、お使いの開発環境のローカルでこのチュートリアルのサンプル トポロジを実行してテストします。

> [AZURE.WARNING] ローカル テストは、basic、c# のみトポロジにのみ機能します。 エラーが発生するため、ハイブリッド トポロジまたは複数のストリームを使用するトポロジには、ローカル テストを使用しないでください。

1.   **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **プロパティ**します。 プロジェクトのプロパティの変更、 **出力の種類** に **コンソール アプリケーション**します。

    ![出力の種類](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] 変更してください、 **出力の種類** に **クラス ライブラリ** トポロジをクラスターにデプロイする前にします。

2.   **ソリューション エクスプ ローラー**, プロジェクトを右クリックし、選択、 **追加** > **[新しい項目の**です。 選択 **クラス** 入力 **LocalTest.cs** クラス名として。 最後に、クリックして **追加**します。

3.  開いている **LocalTest.cs** し、以下の追加 **を使用して** ステートメントを先頭にします。

    ```
    using Microsoft.SCP;
    ```

4.  内容として次を使用して、 **LocalTest** クラス。

    ```
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();


        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);


            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion


        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion


        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);


            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    コード コメントに目を通してください。 このコードを使用して **LocalContext** コンポーネントを実行して、開発環境で、ローカル ドライブ上のテキスト ファイルにコンポーネント間のデータ ストリームが引き続き発生します。

5.  開いている **Program.cs** に以下を追加し、 **Main** メソッド。

    ```
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();


    //If we are not running under the local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

6.  変更を保存し、クリックして **f5 キーを押して** または選択 **デバッグ** > **[デバッグ開始]** プロジェクトを開始します。 コンソール ウィンドウが表示され、テストの進行に合わせてステータスを記録します。 ときに **テストの完了** が表示されたら、キーを押してウィンドウを閉じます。

7.  使用 **Windows エクスプ ローラー** など、プロジェクトが含まれているディレクトリを検索する **C:\Users\ < your_user_name > \Documents\Visual Studio 2013 \projects\wordcount\wordcount**します。 このディレクトリで、開く **Bin**, 、クリックして **デバッグ**します。 テストの実行時に生成された、sentences.txt、counter.txt、splitter.txt というテキスト ファイルが表示されます。 それぞれのテキスト ファイルを開いてデータを確認します。

    > [AZURE.NOTE] これらのファイルでの 10 進数の値の配列として、文字列データを保存します。 たとえば、\[[97,103,111] で、 **splitter.txt** ファイルは、word「および」です。

基本的なワード カウント アプリケーションをローカルでテストすることはとても簡単ですが、外部データ ソースとやりとりしたり、複雑なデータ分析を実行したりする複合的なトポロジを扱う場合には非常に役立ちます。 そのようなプロジェクトを扱う際は、コンポーネントのコードにブレークポイントとステップを設定して問題を分離する必要が生じる場合もあります。

> [AZURE.NOTE] 設定して、 **プロジェクトの種類が** に **クラス ライブラリ** HDInsight クラスターの Storm に展開する前にします。

###ログ情報

トポロジ コンポーネントからの情報は、`Context.Logger` を使用して簡単に記録できます。 たとえば、次のようにすると情報ログ エントリが作成されます。

```
Context.Logger.Info("Component started");
```

記録された情報を表示できます、 **Hadoop Service Log**, は **サーバー エクスプ ローラー**します。 HDInsight クラスターの Storm のエントリを展開し、展開 **Hadoop Service Log**します。 最後に、閲覧するログ ファイルを選択します。

> [AZURE.NOTE] ログは、クラスターで使用される Azure ストレージ アカウントに格納されます。 Visual Studio でログインしたサブスクリプションと異なる場合、この情報を閲覧するにはストレージ アカウントのあるサブスクリプションにログインする必要があります。

###エラー情報の表示

トポロジの実行中に発生したエラーを表示するには、次の手順に従います。

1.   **サーバー エクスプ ローラー**, 、HDInsight クラスターの Storm を右クリックし、選択、 **View Storm topologies**します。

2.   **スパウト** と **ボルト**, 、 **最後のエラー** 列が発生した最後のエラーに関する情報が表示されます。

3.  選択、 **Spout Id** または **Bolt Id** エラーの一覧にあるコンポーネントのためです。 [詳細] ページが表示され、詳細なエラー情報が内に表示される、 **エラー** 、ページの下部のセクションです。

4.  詳細情報を取得するには選択、 **ポート** から、 **エグゼキュータ** 過去数分間の Storm ワーカー ログを表示するページのセクションです。

##次のステップ

開発および Visual Studio の HDInsight ツールから Storm トポロジを展開について説明する方法を習得できた方法 [HDInsight で Storm を使用して Azure Event Hub からイベントを処理する](hdinsight-storm-develop-csharp-event-hub-topology.md)です。

複数のストリームにデータのストリームを分割する c# トポロジの例は、次を参照してください。 [c# Storm の使用例](https://github.com/Blackmist/csharp-storm-example)します。

詳細については、c# トポロジの作成を検出するには、次を参照してください。 [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md)します。

HDInsight を使用するさまざまな方法や、HDInsight での Storm のその他の例については、次の記事をご覧ください。

**HDInsight での Apache Storm**

-   [Deploy and monitor topologies with Apache Storm on HDInsight (HDInsight の Storm を使用したトポロジのデプロイと監視)](hdinsight-storm-deploy-monitor-topology.md)

-   [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)

**HDInsight での Apache Hadoop**

-   [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

-   [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

-   [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)

**HDInsight での Apache HBase**

-   [HDInsight での HBase の使用](hdinsight-hbase-get-started.md)


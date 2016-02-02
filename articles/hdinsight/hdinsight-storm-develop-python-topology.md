<properties
   pageTitle="HDinsight での Storm トポロジの Python コンポーネントの使用 |Microsoft Azure"
   description="Azure HDInsight で Apache Storm から Python コンポーネントを使用する方法について説明します。Java ベースと Clojure ベースの両方の Storm トポロジから Python コンポーネントを使用する方法を学習します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>


# HDInsight での Python を使用した Apache Storm トポロジの開発

Apache Storm では複数の言語がサポートされています。これにより、1 つのトポロジでの複数の言語からのコンポーネントの結合も可能になります。 このドキュメントでは、HDInsight での Java および Clojure ベース Storm トポロジにおける Python コンポーネントの使用方法を学習します。

## 前提条件

* Python 2.7 以上

* Java JDK 1.7 以上

## Storm の複数言語サポート

Storm は、これは、コンポーネントが使用する方法を理解する必要があります。 任意のプログラミング言語を使用して記述されたコンポーネントを使用するように設計されました、 [Thrift の定義の Storm に関する](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)します。 Python では、Storm と簡単に連動できるようにするための Apache Storm プロジェクトの一部として、モジュールが提供されます。 このモジュールを検索する [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py)します。

Apache Storm は Java 仮想マシン (JVM) で実行される Java プロセスであるために、その他の言語で記述されたコンポーネントはサブプロセスとして実行されます。 JVM で実行されている Storm ビットは、stdin/stdout 経由で送信された JSON メッセージを使用してこれらのサブプロセスと通信します。 コンポーネント間の通信の詳細については記載されて、 [マルチ lang プロトコル](https://storm.apache.org/documentation/Multilang-protocol.html) ドキュメントです。

### Storm モジュール

Storm モジュール (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py、) では、Storm を使用する Python のコンポーネントを作成するために必要なビットを提供します。

これによりなど `storm.emit` のタプルを出力して `storm.logInfo` 、ログに書き込む。 このファイルに目を通し、提供内容を理解することをお勧めします。

## 課題

__storm.py__ モジュールを使用して、データを使用する Python スパウトやデータを処理するボルトを作成できますが、コンポーネント間の通信を関連付ける Storm トポロジ定義全体は引き続き Java または Clojure を使用して書き込まれます。 さらに、Java を使用する場合は、Python コンポーネントへのインターフェイスとして機能する Java コンポーネントを作成もする必要もあります。

また、Storm クラスターは分散環境で実行されるため、Python コンポーネントで必要なすべてのモジュールが、クラスター内のすべての worker ノードで使用できることを確認する必要があります。 Storm の場合、多言語リソースではこれを容易に行うことはできません。つまり、トポロジの jar ファイルの一部として依存関係をすべて含めるか、クラスター内の各 worker ノードに依存関係を手動でインストールする必要があります。

など、これらの欠点を克服しようとするいくつかのプロジェクトがある [Pyleus](https://github.com/Yelp/pyleus) と [Streamparse](https://github.com/Parsely/streamparse)します。 これらはいずれも、Linux ベースの HDInsight クラスターで実行できますが、クラスターのセットアップ時にカスタマイズが必要であり、HDInsight クラスターでは完全にテストされていないため、このドキュメントの重要なポイントではありません。 HDInsight でのこれらのフレームワークの使用に関する注意事項は、このドキュメントの最後に示されています。

### Java とClojure トポロジの定義

トポロジを定義する 2 つの方法の 1 つである Clojure は、トポロジ定義で Python コンポーネントを直接参照できるため、非常に簡単でわかりやすい方法です。 Java ベースのトポロジ定義の場合、Python コンポーネントから返されるタプルでのフィールド宣言などを処理する Java コンポーネントを定義する必要もあります。

このドキュメントでは両方の方法について説明し、プロジェクト例も示します。

## Java トポロジを使用する Python コンポーネント

> [AZURE.NOTE] この例については、「で https://github.com/Blackmist/hdinsight-python-storm-wordcount、 __JavaTopology__ ディレクトリ。 これは Maven ベースのプロジェクトです。 Maven に慣れていない場合は、次を参照してください。 [HDInsight での Apache Storm の開発の Java ベース トポロジ](hdinsight-storm-develop-java-topology.md) Storm トポロジの Maven プロジェクトを作成する方法についてです。

Python (または他の JVM 言語コンポーネント) を使用する Java ベース トポロジは最初は Java コンポーネントを使用しているように思えますが、各 Java スパウト/ボルトを見ると、コードは次のようになっています。

    public SplitBolt() {
        super("python", "countbolt.py");
    }

この場合、Java は Python を呼び出し、実際のボルト ロジックを含むスクリプトを実行します。 Java スパウト/ボルト (この例の場合) は、基になる Python コンポーネントによって出力されるタプルでフィールドを宣言するだけです。

実際の Python ファイルに格納されます、 `/複数言語/リソース` この例ではディレクトリです。  `/Multilang` で言及されているディレクトリ、 __pom.xml__:

<resources>
    <resource>
        
        <directory>${basedir}/複数言語</directory>
    </resource>
</resources>

これには、すべてのファイルが含まれます、 `/multilang` このプロジェクトからビルドされる jar 内のフォルダーです。
> [AZURE.IMPORTANT] 注こののみを指定する、 `/multilang` ディレクトリおよび not `/複数言語/リソース`します。 Storm JVM 以外のリソースが必要ですが、 `リソース` ディレクトリに、内部的に既にで検索しています。 このフォルダーにコンポーネントを配置することで、Java コードでは名前での参照のみが可能になります。 たとえば、 `("python"、"countbolt.py"); super`します。 考えてみると、別の方法は、Storm を認識している、 `リソース` ディレクトリをルート (/) マルチ lang リソースにアクセスするとき。
>
> この例のプロジェクトで、 `storm.py` にモジュールが含まれる、 `/複数言語/リソース` ディレクトリ。

### プロジェクトをビルドして実行する

このプロジェクトをローカルで実行する場合は、単に以下の Maven コマンドを使用して、ローカル モードでビルドおよび実行します。

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

プロセスを強制終了するには、Ctrl キーを押しながら C キーを押します。

Apache Storm を実行して HDInsight クラスターにプロジェクトをデプロイするには、次の手順を使用します。

1. 以下を使用して、uber jar をビルドします。

        mvn package

    という名前のファイルが作成されます __WordCount--1.0 SNAPSHOT.jar__ で、 `/target` このプロジェクトのディレクトリ。

2. 次のいずれかの方法を使用して、Hadoop クラスターに jar ファイルをアップロードします。

    * __Linux ベース__ HDInsight クラスター: 使用 `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` クラスターでは、SSH ユーザー名とクラスター名、HDInsight クラスター名を持つユーザー名を置換する jar ファイルをコピーします。

        SSH を使用してクラスターに接続し、トポロジを使用して、起動後、ファイルのアップロードが完了した、 `storm jar WordCount 1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * __Windows ベース__ HDInsight クラスター。 ブラウザーで HTTPS://CLUSTERNAME.azurehdinsight.net/に移動して、Storm ダッシュ ボードに接続します。 CLUSTERNAME を HDInsight クラスター名に置き換え、プロンプトが表示されたら、管理者の名前とパスワードを入力します。

        フォームを使用して、以下のアクションを実行します。

        * __Jar ファイル__: __[参照]__ を選択してから __WordCount-1.0-SNAPSHOT.jar__ ファイルを選択します。
        * __クラス名__: 入力 `com.microsoft.example.WordCount`
        * __追加のパラメーター__: フレンドリ名を入力します `wordcount` トポロジを識別するために。

        最後に、__[送信]__ を選択してトポロジを開始します。

> [AZURE.NOTE] Storm トポロジは、開始すると停止 (強制終了) するまで実行されます。 トポロジを停止するには、いずれかを使用、 `storm kill TOPOLOGYNAME` コマンド、コマンド ライン (への SSH セッション Linux クラスターなど) をまたは Storm UI を使用すると、クリックして、トポロジを選択して、 __Kill__ ] ボタンをクリックします。

## Clojure トポロジを使用する Python コンポーネント

> [AZURE.NOTE] この例については、「で https://github.com/Blackmist/hdinsight-python-storm-wordcount、 __ClojureTopology__ ディレクトリ。

このトポロジを使用して作成された [Leiningen](http://leiningen.org) に [新しい Clojure プロジェクトを作成する](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)します。 その後、スキャフォールディングされたプロジェクトには次の変更が加えられました。

* __project.clj__: Storm の依存関係が追加され、HDInsight サーバーへのデプロイ時に問題が生じる可能性がある項目は除外されました。
* __リソース/リソース__: Leiningen、既定値を作成する `リソース` が、このプロジェクトから作成した jar ファイルのルートへの追加は、ここに格納されているファイルが表示され、Storm という名前のサブディレクトリにファイルが必要ですが、ディレクトリ `リソース`します。 ディレクトリのサブディレクトリが追加されたため、Python ファイルに格納されます `リソース/リソース`します。 実行時に、これは Phthon コンポーネントにアクセスするためのルート (/) として扱われます。
* __src/wordcount/core.clj__: このファイルはトポロジ定義を含み、__project.clj__ ファイルから参照されます。 Clojure を使用して Storm トポロジを定義する方法については、次を参照してください。 [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html)します。

### プロジェクトをビルドして実行する

__プロジェクトをローカルでビルドして実行するには__、次のコマンドを使用します。

    lein do clean, run

トポロジを停止するには、__Ctrl キーを押しながら C キーを押します__。

__uberjar をビルドして、HDInsight にデプロイするには__、次の手順を使用します。

1. 以下のコマンドを使用して、トポロジと必要な依存関係を含む uberjar を作成します。

        lein uberjar

    という名前の新しいファイルが作成されます `wordcount 1.0-SNAPSHOT.jar` で、 `target\uberjar + uberjar` ディレクトリ。

2. 以下のいずれかの方法を使用し、HDInsight クラスターにトポロジをデプロイして実行します。

 * __Linux ベースの HDInsight__

   1. HDInsight クラスター ヘッド ノードを使用して、ファイルをコピー `scp`します。 次に例を示します。

        scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
    
    Replace USERNAME with an SSH user for your cluster, and CLUSTERNAME with your HDInsight cluster name.

   2. ファイルがクラスターにコピーされたら、SSH を使用してクラスターに接続し、ジョブを送信します。 HDInsight での SSH の使用方法については、以下のいずれかの記事を参照してください。

       * [Use SSH with Linux-based HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
       * [Use SSH with Linux-based HDInsight from Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

   3. 接続したら、次のコマンドを使用してトポロジを開始します。

           storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount


 * __Windows ベースの HDInsight__

   1. Storm ダッシュ ボードに接続するには、ブラウザーで HTTPS://CLUSTERNAME.azurehdinsight.net/しようとします。 CLUSTERNAME を HDInsight クラスター名に置き換え、プロンプトが表示されたら、管理者の名前とパスワードを入力します。

   2. フォームを使用して、以下のアクションを実行します。

    * __Jar File__: Select __Browse__, then select the __wordcount-1.0-SNAPSHOT.jar__ file
    * __Class Name__: Enter `wordcount.core`
    * __Additional Paramters__: Enter a friendly name such as `wordcount` to identify the topology
    
    Finally, select __Submit__ to start the topology.


> [AZURE.NOTE] Storm トポロジは、開始すると停止 (強制終了) するまで実行されます。 トポロジを停止するには、いずれかを使用、 `storm kill TOPOLOGYNAME` コマンド ライン (SSH セッションから、Linux クラスターに) コマンドまたはクリックして、トポロジに Storm UI が選択を使用して、 __Kill__ ] ボタンをクリックします。

## Pyleus フレームワーク

[Pyleus](https://github.com/Yelp/pyleus) 機能を次の Storm で Python を使用して容易にできるようにしようとする framework には。

* __YAML ベースのトポロジ定義__: これにより、Java や Clojure の知識を必要としない、より簡単なトポロジの定義方法が提供されます。
* __MessagePack ベースのシリアライザー__: JSON の代わりに、既定のシリアル化として MessagePack が使用されます。 これにより、コンポーネント間でより速くメッセージングを行うことができます。
* __依存関係の管理__: Virtualenv を使用することで、Python の依存関係がすべての worker ノードに確実にデプロイされるようになります。 これには、Virtualenv を worker ノードにインストールする必要があります。

> [AZURE.IMPORTANT] Pyleus では、開発環境に Storm が必要です。 基本的な Apache Storm 0.9.3 ディストリビューションを使用すると、HDInsight で提供されるバージョンの Storm と jar の互換性がなくなるようです。 したがって、次の手順では、開発環境として HDInsight クラスターを使用します。

ビルド環境として HDInsight ヘッド ノードを使用することで、例の Pyleus トポロジを正常にビルドできます。

1. HDInsight クラスター上に新しい Storm をプロビジョニングする場合、Python Virtualenv がクラスター ノード上に存在することを確認する必要があります。 新しい Linux ベースの HDInsight クラスターを作成する場合で次のスクリプト アクションの設定を使用して [クラスターのカスタマイズ](hdinsight-hadoop-customize-cluster.md):

    * __名前__: ここには単にわかりやすい名前を指定します。
    * _ _ スクリプト URI__: 使用 `https://hditutorialdata.blob.core.windows.net/customizecluster/pythonvirtualenv.sh` 値として。 このスクリプトは、ノードに Python Virtualenv をインストールします。
        > [AZURE.NOTE] このドキュメントの後述の Streamparse フレームワークで使用されるいくつかのディレクトリも作成されます。

    * __Nimbus__: この項目をオンにすると、スクリプトが Nimbus (ヘッド) ノードに適用されるようになります。
    * __Supervisor__: この項目をオンにすると、スクリプトが Supervisor (worker) ノードに適用されるようになります。

    その他の項目は空白のままにします。

1. クラスターが作成されたら、以下のように SSH を使用して接続します。

    * [Linux、Unix、OS X から Linux ベースの HDInsight で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Windows から Linux ベースの HDInsight で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-windows.md)

2. SSH 接続の場合は、以下を使用して新しい仮想環境を作成し、Pyleus をインストールします。

        virtualenv pyleus_venv
        source pyleus_venv
        pip install pyleus

3. 次に、以下を使用して、Pyleus の git リポジトリをダウンロードし、WordCount の例をビルドします。

        sudo apt-get install git
        git clone https://github.com/Yelp/pyleus.git
        pyleus build pyleus/examples/word_count/pyleus_topology.yaml

    という名前の新しいファイルがビルドが完了すると、 `word_count.jar` 現在のディレクトリにします。

4. Storm クラスターにトポロジを送信するには、次のコマンドを使用します。

        pyleus submit -n localhost word_count.jar

    `-N` Nimbus ホストを指定します。 使用して、ヘッド ノード上であるため `localhost`します。

    使用することも、 `pyleus` その他の Storm を操作するコマンドです。 実行中のトポロジを一覧表示し、強制終了し、次を使用して、 `word_count` トポロジ。

        pyleus list -n localhost
        pyleus kill -n localhost word_count


## Streamparse フレームワーク

[Streamparse](https://github.com/Parsely/streamparse) 機能を次の Storm で Python を使用して容易にできるようにしようとする framework には。

* __スキャフォールディング__: これを使用すると、簡単にプロジェクトのスキャフォールディングを作成し、ロジックを追加するためにファイルを変更できるようになります。
* __Clojure DSL 関数__: Clojure トポロジ定義での Python コンポーネントの使用詳細度を下げます。
* __依存関係の管理__: Virtualenv を使用することで、Python の依存関係がすべての worker ノードに確実にデプロイされるようになります。 これには、Virtualenv を worker ノードにインストールする必要があります。
* __リモート デプロイメント__: Streamparse は SSH オートメーションを使用して worker ノードにコンポーネントをデプロイすることができ、Nimbus と通信するために SSH トンネルを作成できます。 したがって、開発環境から HDInsight などの Linux ベース クラスターに簡単にデプロイできます。

> [AZURE.IMPORTANT] Streamparse が必要とするコンポーネントに依存して [Unix 信号](https://en.wikipedia.org/wiki/Unix_signal), はできない windows です。 開発環境は Linux、Unix、または OS X である必要があり、HDInsight クラスターは Linux ベースである必要があります。

1. HDInsight クラスター上に新しい Storm をプロビジョニングする場合、Python Virtualenv がクラスター ノード上に存在することを確認する必要があります。 新しい Linux ベースの HDInsight クラスターを作成する場合で次のスクリプト アクションの設定を使用して [クラスターのカスタマイズ](hdinsight-hadoop-customize-cluster.md):

    * __名前__: ここには単にわかりやすい名前を指定します。
    * _ _ スクリプト URI__: 使用 `https://hditutorialdata.blob.core.windows.net/customizecluster/pythonvirtualenv.sh` 値として。 このスクリプトは、ノードに Python Virtualenv をインストールし、Streamparse によって使用されるディレクトリを作成します。
    * __Nimbus__: この項目をオンにすると、スクリプトが Nimbus (ヘッド) ノードに適用されるようになります。
    * __Supervisor__: この項目をオンにすると、スクリプトが Supervisor (worker) ノードに適用されるようになります。

    その他の項目は空白のままにします。
    > [AZURE.WARNING] Streamparse を使用してリモートでデプロイするには、__公開キー__を使用して、HDInsight クラスターの SSH ユーザーをセキュリティで保護する必要もあります。
    >
    > HDInsight での SSH キーの使用について詳しくは、次のいずれかのドキュメントを参照してください。
    >
    > * [Linux、Unix、OS X から Linux ベースの HDInsight で SSH を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Windows から Linux ベースの HDInsight で SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

2. クラスターのプロビジョニング時に、次のコマンドを使用して、開発環境に Streamparse をインストールします。

        pip install streamparse

3. Streamparse がインストールされたら、次のコマンドを使用して例のプロジェクトを作成します。

        sparse quickstart wordcount

    これはという名前の新しいディレクトリを作成 `wordcount`, 、ワード カウント プロジェクトの例で設定します。

4. ディレクトリに変更、 `wordcount` ディレクトリとローカル モードでトポロジを開始します。

        cd wordcount
        sparse run

    トポロジを停止するには、Ctrl キーを押しながら C キーを押します。

### トポロジのデプロイ

Linux ベースの HDInsight クラスターが作成されたら、次の手順を使用して、トポロジをクラスターにデプロイします。

1. 以下のコマンドを使用して、クラスターの worker ノードの完全修飾ドメイン名を見つけます。

        curl -u admin:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts | grep '"host_name" : "worker'

    これで、クラスターのホスト情報が取得され、grep にパイプされてから、worker ノードの項目に返されます。 次のような結果が表示されます。

        "host_name" : "workernode0.1kft5e4nx2tevg5b2pdwxqx1fb.jx.internal.cloudapp.net"

    保存、 `"workernode0.1kft5e4nx2tevg5b2pdwxqx1fb.jx.internal.cloudapp.net"` との情報は、次の手順で使用されます。

2. 開いている、 __config.json__ ファイルで、 `wordcount` ディレクトリ、し、次のエントリを変更します。

    * __user__: これを、HDInsight クラスター用に構成した SSH ユーザー アカウント名に設定します。 これは、プロジェクトのデプロイ時に、クラスターに対する認証に使用されます。
    * __nimbus__: これを設定 `CLUSTERNAME ssh.azurehdinsight.net`します。 CLUSTERNAME はクラスターの名前に置き換えます。 これは、クラスターのヘッド ノードである Nimbus ノードとの通信時に使用されます。
    * __workers__: worker 項目に、curl を使用して取得した worker ノードのホスト名を設定します。 次に例を示します。

        ```
"workers": [
    "workernode0.1kft5e4nx2tevg5b2pdwxqx1fb.jx.internal.cloudapp.net",
    "workernode1.1kft5e4nx2tevg5b2pdwxqx1fb.jx.internal.cloudapp.net"
    ]
        ```

    * __virtualenv\_root__: これを設定"/virtualenv"

    これが、HDInsight クラスター用のプロジェクト構成を含む、 `/virtualenv` スクリプト操作でのプロビジョニング中に作成されたディレクトリです。

4. Streamparse 以降 HDInsight での展開、ヘッド ノードを使用した、認証を作業者に転送する必要がある `で ssh-agent` ワークステーションで開始する必要があります。 ほとんどのオペレーティング システムでは、自動的に開始されます。 実行されていることを確認するには、次のコマンドを使用します。

        echo "$SSH_AUTH_SOCK"

    次のような応答を返すこのは `ssh` が実行されています。

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    > [AZURE.NOTE] 完全なパスは、使用するオペレーティング システムに応じて異なる場合があります。 たとえば、OS X 上のパスようになります `/private/tmp/com.apple.launchd.vq2rfuxaso/Listeners`します。 ただし、エージェントが実行されている場合はいくつかのパスが返されます。

    何も返されない場合は使用して、 `で ssh-agent` エージェントを開始するコマンドです。

5. HDInsight サーバーの認証に使用するキーをエージェントが認識していることを確認します。 エージェントで使用できるキーをリストするには、次のコマンドを使用します。

        ssh-add -L

    これで、エージェントに追加されている秘密キーが返されます。 HDInsight を認証するための SSH キーの作成時に生成した秘密キーの内容と、結果を比較することができます。

    情報が返されない場合、または返される情報が秘密キーと一致しない場合は、以下を使用してエージェントに秘密キーを追加します。

        ssh-add /path/to/key/file

    たとえば、 `ssh アド ~/.ssh/id_rsa`します。

4. HDInsight クラスターで転送を使用する必要があることを認識するように SSH を構成する必要もあります。 次のコードを追加 `~/.ssh/config`します。 このファイルが存在しない場合は、作成し、内容として以下を使用します。

     Host *.azurehdinsight.net
       ForwardAgent yes
    
     Host *.internal.cloudapp.net
       ProxyCommand ssh CLUSTERNAME-ssh.azurehdinsight.net nc %h %p

 CLUSTERNAME を、使用する HDInsight クラスターの名前に置き換えます。

 これで、接続先の任意の *.azurehdinsight.net システムを介して SSH 資格情報の転送を有効にするように、ワークステーション上の SSH エージェントが構成されます。 この場合は、クラスターのヘッド ノードです。 次に、ヘッド ノードから個々の worker ノード (internal.cloudapp.net) へのプロキシ経由の SSH トラフィックに使用するコマンドを構成します。 これにより、Streamparse をヘッド ノードに接続し、SSH アカウントのキー認証を使用して、そこから各 worker ノードに接続できるようになります。

5. 最後に、以下のコマンドを使用して、ローカル開発環境から HDInsight クラスターにトポロジを送信します。

        sparse submit

    これで、HDInsight クラスターに接続され、トポロジと Python 依存関係がデプロイされてから、トポロジが開始されます。

## 次のステップ

このドキュメントでは、Storm トポロジから Python コンポーネントを使用する方法を学習しました。 HDInsight で Phthon を使用する他の方法については、次のドキュメントを参照してください。

* [ストリーミング MapReduce ジョブの Python を使用する方法](hdinsight-hadoop-streaming-python.md)
* [Python ユーザー定義関数 (UDF) で Pig と Hive を使用する方法](hdinsight-python.md)





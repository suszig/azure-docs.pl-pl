<properties
   pageTitle="Java を使用した HDInsight の Storm での Event Hubs のイベントの処理 | Azure"
   description="Maven で作成した Java Storm トポロジを使用して Event Hubs を処理する方法について説明します。"
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

# HDInsight で Storm を使用して Azure Event Hubs のイベントを処理する (Java)

Azure Event Hubs では、Web サイト、アプリ、デバイスで発生する大量のデータを処理できます。 Event Hubs スパウトでは、HDInsight で Apache Storm を使用してこのデータをリアルタイムで簡単に分析できます。 また、Event Hubs のボルトを使用して Storm から Event Hub にデータを書き込むこともできます。

このチュートリアルでは、Event Hubs スパウトとボルトを使用して、Java ベースの Storm トポロジでデータの読み取りと書き込みを行う方法について説明します。

## 前提条件

* HDInsight クラスター上の Apache Storm。 基本について説明した次のいずれかの記事を参照してクラスターを作成します。

    - A [Linux ベースのクラスター](hdinsight-apache-storm-tutorial-get-started-linux.md): SSH を使用して、Linux、Unix、OS X、または Windows クライアントからクラスターを操作する場合、このオプションを選択

    - A [Windows ベースのクラスター](hdinsight-apache-storm-tutorial-get-started.md): PowerShell を使用して、Windows クライアントからクラスターを操作する場合、このオプションを選択

    > [AZURE.NOTE] 2 つのクラスターの種類の唯一の違いは、SSH を使用してクラスターまたは web フォームにトポロジを送信するかどうかです。

* [Azure Event Hub](../event-hubs/service-bus-event-hubs-csharp-ephcs-getstarted.md)

* [Oracle Java Developer Kit (JDK) バージョン 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) など、それと同等または [OpenJDK](http://openjdk.java.net/)

* [Maven](https://maven.apache.org/download.cgi): Maven は Java プロジェクトのプロジェクトのビルド システム

* テキスト エディターまたは Java 統合開発環境 (IDE)

    > [AZURE.NOTE] エディターまたは IDE には、このドキュメントで扱っていない Maven を操作するための特定の機能がある場合があります。 お使いの編集環境の機能に関する詳細は、製品のマニュアルをご覧ください。

 * SSH クライアント HDInsight で SSH を使用する方法の詳細については、次の記事をご覧ください。

    - [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

* SCP クライアント。 Linux、Unix、OS X システムのすべてに付属しています。 Windows クライアントのことをお勧めは PSCP、 [PuTTY のダウンロード ページ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

##例の説明

 [Hdinsight java storm eventhub](https://github.com/Blackmist/hdinsight-java-storm-eventhub) 例には、2 つのトポロジが含まれています。

__com.microsoft.example.EventHubWriter__ Azure Event Hub にランダムなデータを書き込みます。 データはスパウトで生成され、ランダムなデバイス ID とデバイス値です。 この例では、文字列 ID と数値を出力する何らかのハードウェアをシミュレートしています。

__com.microsoft.example.EventHubReader__ Event Hub (EventHubWriter、によって書き込まれたデータ) からデータを読み取り、/devicedata ディレクトリを HDFS (ここで記述され、Azure HDInsight のテストがこれから WASB) に保管します。

データは JSON ドキュメント形式に変更されてから、Event Hub に出力されます。リーダーが読み取るときに、JSON からタプルに解析されます。 JSON 形式は次のとおりです。

    { "deviceId": "unique identifier", "deviceValue": some value }

Event Hub スパウトとボルトの内部書式のしくみを利用せずに、JSON ドキュメントを使用してデータを Event Hub を格納する理由は、形式がわかっているからです。

###プロジェクトの構成

 **POM.xml** ファイルには、この Maven プロジェクトの構成情報が含まれています。 興味深いのは次の点です。

####EventHubs Storm スパウトの依存関係

    <dependency>
      <groupId>com.microsoft.eventhubs</groupId>
      <artifactId>eventhubs-storm-spout</artifactId>
      <version>0.9.3</version>
    </dependency>

eventhubs-storm-spout パッケージの依存関係が加わります。このパッケージには、Event Hubs からの読み取りに使うスパウトと書き込みに使うボルトの両方が含まれます。

> [AZURE.NOTE] このパッケージは、Maven でが利用できないと、後の手順で、ローカルの Maven リポジトリに手動でインストールされます。

####HdfsBolt および WASB コンポーネント

通常、HdfsBolt は、Hadoop Distributed File System HDFS にデータを格納するために使用されます。 一方、HDInsight クラスターは Azure Storage (WASB) を既定のデータ ストアとして使用するので、HdfsBolt が WASB ファイル システムを理解できるようにするコンポーネントをいくつか読み込む必要があります。

      <!--HdfsBolt stuff -->
      <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-hdfs</artifactId>
        <exclusions>
          <exclusion>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
          </exclusion>
          <exclusion>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-hdfs</artifactId>
          </exclusion>
        </exclusions>
        <version>0.9.3</version>
      </dependency>
      <!--
     This is a temporary workaround to make HdfsBolt work with WASB through hadoop-azure project.
     For now, we have to build hadoop-client, hadoop-hdfs and hadoop-azure from Hadoop trunk
     (which defaults to 3.0.0-SNAPSHOT version). And push those jars and dependencies to local
     mvn repo (take a look at push_lib_mvn.ps1).

     Once Hadoop 2.7 is released, we can just switch to that version.
     Note that hadoop-azure is added to Hadoop on Hadoop 2.7.
     -->
     <dependency>
       <groupId>org.apache.hadoop</groupId>
       <artifactId>hadoop-client</artifactId>
       <version>3.0.0-SNAPSHOT</version>
     </dependency>
     <dependency>
       <groupId>org.apache.hadoop</groupId>
       <artifactId>hadoop-hdfs</artifactId>
       <version>3.0.0-SNAPSHOT</version>
     </dependency>
     <dependency>
       <groupId>org.apache.hadoop</groupId>
       <artifactId>hadoop-azure</artifactId>
       <version>3.0.0-SNAPSHOT</version>
     </dependency>
     <dependency>
       <groupId>org.apache.hadoop</groupId>
       <artifactId>hadoop-common</artifactId>
       <version>3.0.0-SNAPSHOT</version>
       <exclusions>
         <exclusion>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-log4j12</artifactId>
         </exclusion>
       </exclusions>
     </dependency>
     <dependency>
       <groupId>com.microsoft.windowsazure.storage</groupId>
       <artifactId>microsoft-windowsazure-storage-sdk</artifactId>
       <version>0.6.0</version>
     </dependency>

> [AZURE.NOTE] WASB を有効にするパッケージは、Maven リポジトリに表示されないし、後の手順で手動でインストールされました。

####maven-compiler-plugin

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>2.3.2</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

プロジェクトに Java 7 との互換性を持たせてコンパイルするように Maven に指示します。Java 7 は、HDInsight クラスターから使用されます。

####maven-shade-plugin

      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>2.3</version>
        <configuration>
          <!-- Keep us from getting a can't overwrite file error -->
          <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
            </transformer>
          </transformers>
          <!-- Keep us from getting a bad signature error -->
          <filters>
            <filter>
                <artifact>*:*</artifact>
                <excludes>
                    <exclude>META-INF/*.SF</exclude>
                    <exclude>META-INF/*.DSA</exclude>
                    <exclude>META-INF/*.RSA</exclude>
                </excludes>
            </filter>
        </filters>
        </configuration>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
          </execution>
        </executions>
      </plugin>

プロジェクト コードと必要な依存関係の両方を含む uber jar にソリューションをパッケージ化するために使用されます。 また、次の用途にも使用されます。

* 依存関係のライセンス ファイル名を変更する: この処理を実行しないと、Windows ベースの HDInsight クラスターで実行時にエラーが発生する可能性があります。

* セキュリティ/署名を除外する: この処理を実行しないと、HDInsight クラスターで実行時にエラーが発生する可能性があります

####exec-maven-plugin

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.2.1</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

次のコマンドを使用して、ローカルの開発環境でトポロジを実行できます。

    mvn compile exec:java -Dstorm.topology=<CLASSNAME>

たとえば、「`mvn compile exec:java -Dstorm.topology=com.microsoft.example.EventHubWriter`」のように入力します。

####resources セクション

    <resources>
      <resource>
        <directory>${basedir}/conf</directory>
        <filtering>false</filtering>
        <includes>
          <include>EventHubs.properties</include>
          <include>core-site.xml</include>
        </includes>
      </resource>
    </resources>

プロジェクトに必要なリソースを定義します。

- **EventHubs.properties**: Azure イベント ハブに接続するための情報が含まれています
- **core-site.xml**: HDInsight クラスターで使用する Azure ストレージに関する情報が含まれています。

これらの両方に、Event Hub と HDInsight クラスターに関する情報を設定する必要があります。

##環境変数を構成する

開発用ワークステーションに Java と JDK をインストールするときに、次のような環境変数が設定される場合があります。 ただし、これらが存在するかどうかや、システムに対して適切な値が含まれているかを確認する必要があります。

* **JAVA_HOME** -Java ランタイム環境 (JRE) がインストールされているディレクトリを指す必要があります。 たとえば、Unix や Linux ディストリビューションの場合は、`/usr/lib/jvm/java-7-oracle` のような値になります。 Windows の場合は、`c:\Program Files (x86)\Java\jre1.7` のような値になります。

* **パス** -次のパスを含める必要があります。

    * **JAVA_HOME** (または同等のパス)

    * **Java_home \bin** (または同等のパス)

    * Maven がインストールされているディレクトリ

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

## プロジェクトのダウンロードとビルド

1. GitHub からプロジェクトのダウンロード: [hdinsight java storm eventhub](https://github.com/Blackmist/hdinsight-java-storm-eventhub)します。 Zip アーカイブとしてパッケージをダウンロードするか、使用して [git](https://git-scm.com/) をローカルでプロジェクトを複製します。

2. 次のコマンドを使用して、プロジェクトに含まれるパッケージをローカルの Maven リポジトリにインストールします。 その結果、Event Hub スパウトとボルトが有効になるだけでなく、HdfsBolt を使用して Azure Storage (WASB) に出力できるようになります。

        mvn -q install:install-file -Dfile=lib/eventhubs/eventhubs-storm-spout-0.9.3-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9.3 -Dpackaging=jar

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-azure-3.0.0-SNAPSHOT.jar

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-client-3.0.0-SNAPSHOT.jar

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-hdfs-3.0.0-SNAPSHOT.jar

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-common-3.0.0-SNAPSHOT.jar -DpomFile=lib/hadoop/hadoop-common-3.0.0-SNAPSHOT.pom

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-project-dist-3.0.0-SNAPSHOT.pom -DpomFile=lib/hadoop/hadoop-project-dist-3.0.0-SNAPSHOT.pom

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-project-3.0.0-SNAPSHOT.pom -DpomFile=lib/hadoop/hadoop-project-3.0.0-SNAPSHOT.pom

        mvn -q org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=lib/hadoop/hadoop-main-3.0.0-SNAPSHOT.pom -DpomFile=lib/hadoop/hadoop-main-3.0.0-SNAPSHOT.pom

    > [AZURE.NOTE] 設定する必要は mau を Powershell を使用している場合、 `-D` 引用符で囲まれたパラメーター。 たとえば、「`"-Dfile=lib/hadoop/hadoop-main-3.0.0-SNAPSHOT.pom"`」のように入力します。

    またこれらのファイルはもともと https://github.com/hdinsight/hdinsight-storm-examples、最新のバージョンを検索できるようにです。

3. 次のコマンドを使用して、プロジェクトをビルドしてパッケージ化します。

        mvn package

    必要な依存関係がダウンロードされ、ビルドされ、プロジェクトがパッケージ化されます。 出力を格納する、 __/target__ ディレクトリを __EventHubExample 1.0-SNAPSHOT.jar__します。

## トポロジのデプロイ

このプロジェクトで作成した jar には、2 つのトポロジが含まれています。 __com.microsoft.example.EventHubWriter__ と __com.microsoft.example.EventHubReader__します。 EventHubWriter トポロジを最初に開始する必要があります。これは、Event Hub にイベントが出力され、その出力が EventHubReader から読み取られるためです。

###Linux ベースのクラスターを使用する場合

1. SCP を使用して、HDInsight クラスターに jar パッケージをコピーします。 USERNAME には、使用するクラスターの SSH ユーザーを指定します。 CLUSTERNAME には、使用する HDInsight クラスター名を指定します。

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    SSH アカウントにパスワードを使用した場合は、そのパスワードの入力を求められます。 アカウントで SSH キーを使用した場合は、`-i` パラメーターを使用してキー ファイルのパスを指定することが必要な場合があります。 たとえば、「`scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`」のように入力します。

    > [AZURE.NOTE] クライアントが Windows ワークステーションの場合は、インストールされている SCP コマンドをしないがあります。 PSCP からダウンロードできることをお勧めします [PuTTY のダウンロード ページ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

    このコマンドで、クラスター上の SSH ユーザーのホーム ディレクトリにファイルをコピーします。

1. ファイルのアップロードが完了したら、SSH を使用して HDInsight クラスターに接続します。 置換 **USERNAME** 、SSH ログインの名前。 置換 **CLUSTERNAME** 、HDInsight クラスター名を使用します。

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] SSH アカウントのパスワードを使用する場合は、パスワードを入力するように求められます。 アカウントで SSH キーを使用した場合は、`-i` パラメーターを使用してキー ファイルのパスを指定することが必要な場合があります。 次の例は、`~/.ssh/id_rsa` から秘密キーを読み込みます。
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

    PuTTY を使用している場合は、入力 `CLUSTERNAME-ssh.azurehdinsight.net` で、 __ホスト名 (または IP アドレス)__ 、フィールドをクリックして __開く__ に接続します。 SSH アカウント名を入力するように求められます。

    > [AZURE.NOTE] SSH アカウントのパスワードを使用する場合は、パスワードを入力するように求められます。 アカウントで SSH キーを使用した場合、必要に応じて次の手順でキーを選択します。
    >
    > 1.  **カテゴリ**, 、展開 **接続**, 、展開 **SSH**, を選択して **Auth**します。
    > 2. クリックして **参照** し、秘密キーが含まれた .ppk ファイルを選択します。
    > 3. をクリックして __開く__ に接続します。

2. 次のコマンドを使用して、トポロジを開始します。

        storm jar EventHubExample-1.0-SNAPSHOT.jar com.microsoft.example.EventHubWriter writer
        storm jar EventHubExample-1.0-SNAPSHOT.jar com.microsoft.example.EventHubReader reader

    トポロジが開始され、"reader" と "writer" というフレンドリ名が付けられます。

3. 1 ～ 2 分待つと、トポロジで Event Hub のイベントの書き込みと読み取りを実行できるようになります。次に、以下のコマンドを使用して、EventHubReader がデータを HDInsight ストレージに格納していることを確認します。

        hadoop fs -ls /devicedata

    次のようなファイルの一覧が返されます。

        -rw-r--r--   1 storm supergroup      10283 2015-08-11 19:35 /devicedata/wasbbolt-14-0-1439321744110.txt
        -rw-r--r--   1 storm supergroup      10277 2015-08-11 19:35 /devicedata/wasbbolt-14-1-1439321748237.txt
        -rw-r--r--   1 storm supergroup      10280 2015-08-11 19:36 /devicedata/wasbbolt-14-10-1439321760398.txt
        -rw-r--r--   1 storm supergroup      10267 2015-08-11 19:36 /devicedata/wasbbolt-14-11-1439321761090.txt
        -rw-r--r--   1 storm supergroup      10259 2015-08-11 19:36 /devicedata/wasbbolt-14-12-1439321762679.txt

    > [AZURE.NOTE] いくつかのファイル、EventHubReader によって作成されたが、データがまだに格納されていないと、0 のサイズが表示されます。

    ファイルの内容を表示するには、次のコマンドを使用します。

        hadoop fs -text /devicedata/*.txt

    次のようなデータが返されます。

        3409e622-c85d-4d64-8622-af45e30bf774,848981614
        c3305f7e-6948-4cce-89b0-d9fbc2330c36,-1638780537
        788b9796-e2ab-49c4-91e3-bc5b6af1f07e,-1662107246
        6403df8a-6495-402f-bca0-3244be67f225,275738503
        d7c7f96c-581a-45b1-b66c-e32de6d47fce,543829859
        9a692795-e6aa-4946-98c1-2de381b37593,1857409996
        3c8d199b-0003-4a79-8d03-24e13bde7086,-1271260574

    最初の列にはデバイスの ID 値、2 列目にはデバイス値が含まれます。

4. 次のコマンドを使用してトポロジを停止します。

        storm kill reader
        storm kill writer

###Windows ベースのクラスターを使用している場合

1. Https://CLUSTERNAME.azurehdinsight.net にブラウザーを開きます。 メッセージが表示されたら、HDInsight クラスター管理者の資格情報を入力します。 Storm ダッシュボードが表示されます。

2. 使用して、 __Jar ファイル__ ボックスの一覧を参照して、ビルド環境から EventHubExample 1.0-SNAPSHOT.jar ファイルを選択します。

3.  __クラス名__, 、入力 `com.mirosoft.example.EventHubWriter`します。

4.  __追加パラメーター__, 、入力 `writer`します。 最後に、クリックして __送信__ を jar ファイルをアップロードして EventHubWriter トポロジを開始します。

5. トポロジが開始されたら、フォームを使用して EventHubReader を開始します。

    * __Jar ファイル__: EventHubExample-1.0-SNAPSHOT.jar がまだアップロードされての選択
    * __クラス名__: を入力してください `com.microsoft.example.EventHubReader`
    * __追加のパラメーター__: を入力してください `reader`

    [送信] をクリックして EventHubReader トポロジを開始します。

6. イベントを生成、保存し、Azure ストレージを選択し、トポロジを許可するように、数分待ってから、 __クエリ コンソール__ ] タブの上部にある、 __Storm ダッシュ ボード__ ページです。

7.  __クエリ コンソール__, [ __Hive エディター__ と既定値を置換 `select * from hivesampletable` に次の。

        create external table devicedata (deviceid string, devicevalue int) row format delimited fields terminated by ',' stored as textfile location 'wasb:///devicedata/';
        select * from devicedata limit 10;

    クリックして __選択__ クエリを実行します。 EventHubReader から Azure Storage (WASB) に出力されたデータから、10 行が返されます。 クエリが完了すると、次のようなデータが表示されます。

        3409e622-c85d-4d64-8622-af45e30bf774,848981614
        c3305f7e-6948-4cce-89b0-d9fbc2330c36,-1638780537
        788b9796-e2ab-49c4-91e3-bc5b6af1f07e,-1662107246
        6403df8a-6495-402f-bca0-3244be67f225,275738503
        d7c7f96c-581a-45b1-b66c-e32de6d47fce,543829859
        9a692795-e6aa-4946-98c1-2de381b37593,1857409996
        3c8d199b-0003-4a79-8d03-24e13bde7086,-1271260574

8. 選択、 __Storm ダッシュ ボード__ ページの上部にあるを選択し、 __Storm UI__します。  __Storm UI__, へのリンクを選択、 __リーダー__ トポロジと、使用して、 __Kill__ 、トポロジの停止] ボタンをクリックします。 プロセスを繰り返して、 __ライター__ トポロジです。



### チェックポイント機能

EventHubSpout は Zookeeper ノードに対する状態へのチェックポイントを定期的に設定し、キューから読み取ったメッセージの現在のオフセットを保存します。 これにより、コンポーネントは次のシナリオで保存されたオフセットでメッセージの受信を開始できます。

* コンポーネントのインスタンスは失敗し、再起動されます。

* ノードを追加または削除して、クラスターを拡張または圧縮します。

* トポロジが強制終了され、再起動 **同名**します。

####Windows ベースの HDInsight クラスターの場合

保存されたチェックポイントを WASB (HDInsight クラスターで使用される Azure Storage) にエクスポート、インポートできます。これを行うスクリプトは、HDInsight クラスターの Storm でにある、 **c:\apps\dist\storm-0.9.3.2.2.1.0-2340\zkdatatool-1.0\bin**します。

>[AZURE.NOTE] パスのバージョン番号は、Storm クラスターへのインストールのバージョンが、今後変わる可能性が異なる、可能性があります。

このディレクトリのスクリプトは次のとおりです。

* **stormmeta_import.cmd**: クラスターの既定のストレージ コンテナーから Zookeeper にすべての Storm メタデータをインポートします。

* **stormmeta_export.cmd**: Zookeeper からクラスターの既定のストレージ コンテナーにすべての Storm メタデータをエクスポートします。

* **stormmeta_delete.cmd**: Zookeeper からすべての Storm メタデータを削除します。

インポートのエクスポートにより、クラスターを削除する必要がある一方で、新しいクラスターを再びオンラインにする際にハブの現在のオフセットから処理を再開する場合、チェックポイントのデータを保持できます。

> [AZURE.NOTE] 既定のストレージ コンテナーは、新しいクラスターにデータが保存されるため **必要があります** 以前のクラスターと同じストレージ アカウントとコンテナーを使用します。

##トラブルシューティング

(`hadoop fs -ls /devicedata` コマンドまたはクエリ コンソールの Hive コマンドを使用して) /devicedata ディレクトリに格納されているファイルが表示されない場合は、Storm UI を使用して、トポロジから返されるエラーを確認します。

Storm UI の詳細な使用方法については、次のトピックを参照してください。

* 使用している場合、 __Linux ベース__ Storm HDInsight クラスターで、次を参照してください [展開し、Linux ベースの HDInsight での Apache Storm トポロジの管理。](hdinsight-storm-deploy-monitor-topology-linux.md)

* 使用している場合、 __Windows ベース__ Storm HDInsight クラスターで、次を参照してください [展開し、Windows ベースの HDInsight での Apache Storm トポロジの管理。](hdinsight-storm-deploy-monitor-topology-linux.md)

##次のステップ

* [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)


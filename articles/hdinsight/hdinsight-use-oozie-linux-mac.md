<properties
    pageTitle="Linux ベースの HDInsight での Hadoop Oozie ワークフローの使用 | Microsoft Azure"
    description="Linux ベースの HDInsight で Hadoop Oozie を使用します。 Oozie ワークフローを定義し、Oozie ジョブを送信する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="larryfr"/>


# Hadoop で Oozie を使用して Linux ベースの HDInsight でワークフローを定義して実行する (プレビュー)

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Apache Oozie を使用して、Hive と Sqoop を使用するワークフローを定義し、Linux ベースの HDInsight クラスターでワークフローを実行する方法について説明します。

Apache Oozie は Hadoop ジョブを管理するワークフローおよび調整システムです。 Hadoop スタックと統合されていて、Apache MapReduce、Apache Pig、Apache Hive、Apache Sqoop の Hadoop ジョブをサポートしています。 Java プログラムやシェル スクリプトなど、システムに固有のジョブのスケジュールを設定する際にも使用できます。

> [AZURE.NOTE] HDInsight でワークフローを定義するための別のオプションは、Azure Data Factory です。 Azure Data Factory の詳細については、次を参照してください。 [Pig の使用と Data Factory を使って Hive][azure のデータの工場出荷時の pig-hive]します。

##前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**: を参照してください [取得 Azure 無料試用版](get-azure-free-trial-for-testing-hadoop-in-hdinsight.md)します。

- **Azure CLI**: を参照してください [のインストールし、Azure CLI の構成](xplat-cli-install.md)

- **HDInsight クラスター**: を参照してください [Linux で HDInsight の概要](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Azure SQL database**: このドキュメントで手順を使用して作成されます

##ワークフローの例

このドキュメントの手順に従って実装するワークフローには 2 つのアクションが含まれます。 アクションはタスク (Hive、Sqoop、MapReduce、または他のプロセスの実行など) の定義です。

![ワークフロー図][img-workflow-diagram]

1. レコードを抽出する HiveQL スクリプトを実行して Hive アクション、 **hivesampletable** HDInsight に含まれています。 各データ行は、特定のモバイル デバイスからのアクセスを表します。 レコードの形式は次のようになります。

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    このドキュメントで使用する Hive スクリプトは、プラットフォームごと (Android や iPhone など) の合計アクセス数をカウントし、カウントしたアクセス数を新しい Hive テーブルに保存します。

    Hive の詳細については、次を参照してください。 [Hive を HDInsight と共に使用して][hdinsight を使用して hive]します。

2.  Sqoop アクションでは、新しい Hive テーブルの内容を Azure SQL Database のテーブルにエクスポートします。 Sqoop の詳細については、次を参照してください。 [HDInsight での Hadoop Sqoop の使用][hdinsight を使用して sqoop]します。

> [AZURE.NOTE] HDInsight クラスターでサポートされている Oozie のバージョンを参照してください [HDInsight で提供される Hadoop クラスター バージョンの新機能ですか?][。hdinsight バージョン]です。

##作業ディレクトリの作成

Oozie では、ジョブに必要なリソースを同じディレクトリに保存する必要があります。 この例では **wasb:////チュートリアル/$storageuri/tutorials/useoozie**します。 次のコマンドを使用して、このディレクトリと、このワークフローで作成される新しい Hive テーブルを保持する data ディレクトリを作成します。

    hadoop fs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE]  `-p` パラメーターの原因となったすべてのディレクトリのパスがまだ存在しない場合に作成されます。  **データ** によって使用されるデータを保持するためにディレクトリが使用される、 **useooziewf.hql** スクリプトです。

次のコマンドを実行して、Hive ジョブと Sqoop ジョブの実行時に Oozie がユーザー アカウントを偽装できるようにします。 置換 **USERNAME** をログイン名。

    sudo adduser USERNAME users

ユーザーが既に users のメンバーであることを示すエラーが返された場合は無視してかまいません。

##データベース ドライバーの追加

このワークフローでは、Sqoop を使用してデータを SQL Database にエクスポートするため、SQL Database との対話に使用する JDBC ドライバーのコピーを提供する必要があります。 次のコマンドを使用して、ドライバーを作業ディレクトリにコピーします。

    hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc4.jar /tutorials/useoozie/sqljdbc4.jar

ワークフローで他のリソース (MapReduce アプリケーションを含む jar など) を使用する場合は、これらのリソースも追加する必要があります。

##Hive クエリの定義

次の手順に従って、クエリを定義する HiveQL スクリプトを作成します。これは、このドキュメントで後述する Oozie ワークフローで使用します。

1. SSH を使用して、Linux ベースの HDInsight クラスターに接続します。

    * **Linux、Unix または OS X クライアント**: を参照してください [SSH Linux、OS X または Unix から HDInsight 上の Linux ベースの Hadoop の使用](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows クライアント**: を参照してください [SSH Windows から Linux ベースの Hadoop の使用](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 次のコマンドを使用して、新しいファイルを作成します。

        nano useooziewf.hql

1. nano エディターが開いたら、ファイルの内容として次のスクリプトを使用します。

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    このスクリプトでは、次の 2 つの変数を使用しています。

    - **${hivetablename}**: 作成するテーブルの名前を含む
    - **${hivedatafolder}**: テーブルのデータ ファイルを保存する場所が含まれます

    ワークフロー定義ファイル (このチュートリアルでは workflow.xml) は、実行時にこの HiveQL スクリプトにこれらの値を渡します。

2. Ctrl + X キーを押してエディターを終了します。 メッセージが表示されたら、[ **Y** ファイルを保存するを使用し、 **Enter** を使用する、 **useooziewf.hql** ファイル名。

3. 次のコマンドを使用して、コピー **useooziewf.hql** に **wasb:///tutorials/useoozie/useooziewf.hql**:

        hadoop fs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    これらのコマンド、 **useooziewf.hql** クラスターが削除された場合でも、ファイルを保持するが、このクラスターに関連付けられている Azure ストレージ アカウントのファイルにします。 これにより、ジョブとワークフローを維持しながら、使用されていないクラスターを削除することでコストを削減できます。

##ワークフローの定義

Oozie ワークフロー定義は hPDL (XML プロセス定義言語) で書かれています。 次の手順に従ってワークフローを定義します。

1. 次のステートメントを使用して、新しいファイルを作成し、編集します。

        nano workflow.xml

1. nano エディターが開いたら、ファイルの内容として次のコードを入力します。

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
          <start to = "RunHiveScript"/>
          <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
              <job-tracker>${jobTracker}</job-tracker>
              <name-node>${nameNode}</name-node>
              <configuration>
                <property>
                  <name>mapred.job.queue.name</name>
                  <value>${queueName}</value>
                </property>
              </configuration>
              <script>${hiveScript}</script>
              <param>hiveTableName=${hiveTableName}</param>
              <param>hiveDataFolder=${hiveDataFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
          </action>
          <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
              <job-tracker>${jobTracker}</job-tracker>
              <name-node>${nameNode}</name-node>
              <configuration>
                <property>
                  <name>mapred.compress.map.output</name>
                  <value>true</value>
                </property>
              </configuration>
              <arg>export</arg>
              <arg>--connect</arg>
              <arg>${sqlDatabaseConnectionString}</arg>
              <arg>--table</arg>
              <arg>${sqlDatabaseTableName}</arg>
              <arg>--export-dir</arg>
              <arg>${hiveDataFolder}</arg>
              <arg>-m</arg>
              <arg>1</arg>
              <arg>--input-fields-terminated-by</arg>
              <arg>"\t"</arg>
              <archive>sqljdbc4.jar</archive>
              </sqoop>
            <ok to="end"/>
            <error to="fail"/>
          </action>
          <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
          </kill>
          <end name="end"/>
        </workflow-app>

    このワークフローでは、2 つのアクションが定義されています。

    - **RunHiveScript**: 開始アクションは、このを実行、 **useooziewf.hql** Hive スクリプト

    - **RunSqoopExport**: Sqoop を使用して SQL データベースに Hive スクリプトから作成されたデータをエクスポートします。 このは場合にのみ実行、 **RunHiveScript** アクションが正常に完了します。

        > [AZURE.NOTE] Oozie ワークフローとワークフロー アクションの使用に関する詳細については、次を参照してください。 [Apache Oozie 4.0 のドキュメント][apache oozie-400] (の hdinsight クラスター バージョン 3.0) または [Apache Oozie 3.3.2 のドキュメント][apache oozie-332] (HDInsight バージョン 2.1) 用です。

    このワークフローには、このドキュメントで後述するジョブ定義で使用する値に置き換えられる複数のエントリ (`${jobTracker}` など) が含まれています。

    また、Sqoop セクションの `<archive>sqljdbc4.jar</arcive>` エントリにも注意してください。 このエントリは、このアクションの実行時にこのアーカイブを Sqoop で使用できるようにすることを Oozie に指示します。

2. CTRL + X を使用し、 **Y** と **Enter** ファイルを保存します。

3. 次のコマンドを使用して、コピー、 **workflow.xml** ファイルを **wasb:///tutorials/useoozie/workflow.xml**:

        hadoop fs -copyFromLocal workflow.xml wasb:///tutorials/useoozie/workflow.xml

##データベースの作成

次の手順では、データのエクスポート先となる Azure SQL Database を作成します。

> [AZURE.IMPORTANT] 次の手順を実行する前に行う必要があります [のインストールし、Azure CLI の構成](xplat-cli-install.md)します。 CLI のインストールとデータベースを作成する手順は、HDInsight クラスターまたはローカル ワークステーションから実行できます。

1. 次のコマンドを使用して、新しい Azure SQL Database サーバーを作成します。

        azure sql server create <adminLogin> <adminPassword> <region>

    たとえば、`azure sql server create admin password "West US"` です。

    コマンドが完了したら、次のような応答が返されます。

        info:    Executing command sql server create
        + Creating SQL Server
        data:    Server Name i1qwc540ts
        info:    sql server create command OK

    > [AZURE.IMPORTANT] このコマンドによって返されるサーバー名に注意してください (**i1qwc540ts** 上の例です)。これは、作成された SQL Database サーバーの短い名前です。 完全修飾ドメイン名 (FQDN) は **& lt; shortname & gt;。.database.windows.net を付けて**します。 上記の例では、FQDN がなります **i1qwc540ts.database.windows.net**します。

2. という名前のデータベースを作成する次のコマンドを使用して **oozietest** SQL データベース サーバー上。

        azure sql db create [options] <serverName> oozietest <adminLogin> <adminPassword>

    コマンドが完了すると、"OK" というメッセージが返されます。

    > [AZURE.NOTE] アクセスがないことを示すエラーが発生した場合は、次のコマンドを使用して SQL Database ファイアウォールに、システムの IP アドレスを追加する必要があります。
    >
    > `sql firewallrule create [options] <serverName> <ruleName> <startIPAddress> <endIPAddress>`

###テーブルを作成する

> [AZURE.NOTE] テーブルを作成する SQL データベースに接続する方法はたくさんあります。 次の手順を使用して [FreeTDS](http://www.freetds.org/) 、HDInsight クラスターからです。

3. 次のコマンドを使用して、FreeTDS を HDInsight クラスターにインストールします。

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. FreeTDS がインストールされると、次のコマンドを使用して、以前に作成した SQL Database サーバーに接続できます。

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    次のような出力が返されます。

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. `1>` プロンプトで、以下の行を入力します。

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    `GO` ステートメントを入力すると、前のステートメントが評価されます。 これはという名前の新しいテーブルを作成 **mobiledata** Sqoop によるに書き込みなります。

    次を使用して、テーブルが作成されたことを確認します。

        SELECT * FROM information_schema.tables
        GO

    次のような出力が表示されます。

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. `1>` プロンプトで「`exit`」と入力して、tsql ユーティリティを終了します。

##ジョブ定義の作成

ジョブ定義では、workflow.xml とワークフローで使用される他のファイル (useooziewf.hql など) の検索場所を記述します。また、ワークフローおよび関連するファイル内で使用されるプロパティの値も定義します。

1. 次のコマンドを使用して、既定のストレージの完全な WASB アドレスを取得します。 これは構成ファイルですぐに使用されます。

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    次のような情報が返されます。

        <name>fs.defaultFS</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>

    保存、 **wasb://mycontainer@mystorageaccount.blob.core.windows.net** 値に設定する次の手順で使用するためです。

2. 次のコマンドを使用して、クラスターのヘッド ノードの FQDN を取得します。 これは、クラスターの JobTracker アドレスに使用されます。 これは構成ファイルですぐに使用されます。

        hostname -f

    次のような情報が返されます。

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    JobTracker に使用する完全なアドレスになるよう、JobTracker に使用するポートは 8050、 **hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**します。

1. 次のコマンドを使用して、Oozie ジョブ定義構成を作成します。

        nano job.xml

2. nano エディターが開いたら、ファイルの内容として次のスクリプトを使用します。

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
          </property>

          <property>
            <name>queueName</name>
            <value>default</value>
          </property>

          <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
          </property>

          <property>
            <name>hiveScript</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * すべてのインスタンスを置き換える **wasb://mycontainer@mystorageaccount.blob.core.windows.net** 前に返された値を使用します。

    > [AZURE.WARNING] パスの一部としてコンテナーとストレージ アカウントを使用して完全な WASB パスを使用する必要があります。 短い形式を使用して (wasb:///) をジョブを開始するときに RunHiveScript アクションが発生します。

    * 置換 **JOBTRACKERADDRESS** 前に返された JobTracker/ResourceManager アドレスを使用します。

    * 置換 **YourName** を HDInsight クラスターのログイン名。

    * 置換 **serverName**, 、**adminLogin**, 、および **adminPassword** Azure SQL Database の情報を使用します。

    このファイル内のほとんどの情報は、workflow.xml ファイルまたは ooziewf.hql ファイルで使用される値 (${nameNode} など) を設定するために使用されます。

    > [AZURE.NOTE]  **Oozie.wf.application.path** エントリは、workflow.xml ファイルの場所を定義、ワークフローが含まれた、このジョブで実行します。

2. CTRL + X を使用し、 **Y** と **Enter** ファイルを保存します。

##ジョブの送信と管理

次の手順では、Oozie コマンドを使用して、クラスターで Oozie ワークフローを送信および管理します。 Oozie コマンドでは、使いやすいインターフェイスはに対する、 [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)します。

> [AZURE.IMPORTANT] Oozie コマンドを使用する場合は、HDInsight ヘッドノードの FQDN を使用する必要があります。 この FQDN にはクラスターからのみアクセスできます。クラスターが Azure Virtual Network 上にある場合は、同じネットワーク上の他のマシンからアクセスできます。

1. 次のコマンドを使用して、Oozie サービスの URL を取得します。

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    次のような値が返されます。

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

     **Http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie** 部分が Oozie コマンドで使用する URL。

2. 次のコマンドを使用して、URL の環境変数を作成します。すべてのコマンドでこれを入力する必要があるわけではありません。

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    URL を前に返された URL に置き換えます。

3. 次のコマンドを使用してジョブを送信します。

        oozie job -config job.xml -submit

    これによりからジョブ情報が読み込まれます。 **job.xml** Oozie に送信しますが、は実行しません。

    コマンドが完了すると、ジョブの ID が返されます。 たとえば、「`0000005-150622124850154-oozie-oozi-W`」のように入力します。 これはジョブの管理に使用されます。

4. 次のコマンドを使用して、ジョブの状態を表示します。 前のコマンドによって返されたジョブ ID を入力します。

        oozie job -info <JOBID>

    次のような情報が返されます。

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasb:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    このジョブの状態は `PREP` です。これは、ジョブは送信済みですが、まだ開始されていないことを示しています。

4. 次のコマンドを使用してジョブを開始します。

        oozie job -start JOBID

    このコマンドの実行後に状態を確認すると、ジョブが実行中状態になり、ジョブのアクションに関する情報が返されます。

5. タスクが正常に完了したら、次のコマンドを使用して、データが生成され、SQL Database テーブルにエクスポートされたことを確認できます。

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    `1>` プロンプトで次のコマンドを入力します。

        SELECT * FROM mobiledata
        GO

    次のような情報が返されます。

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Oozie コマンドの詳細については、次を参照してください。 [Oozie コマンド ライン ツール](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html)します。

##Oozie REST API

Oozie REST API では、Oozie を使用する独自のツールを構築できます。 Oozie REST API の使用に関する HDInsight 固有の情報は次のとおりです。

* **URI**: でクラスターの外部から REST API にアクセスすることができます `https://CLUSTERNAME.azurehdinsight.net/oozie`

* **認証**: クラスターの HTTP アカウント (admin) とパスワードを使用して、API に対して認証が必要です。 次に例を示します。

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Oozie REST API の使用に関する詳細については、次を参照してください。 [Oozie Web サービス API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)します。

##Oozie Web UI

Oozie Web UI は、クラスターでの Oozie ジョブの状態を表示する Web ベースのビューを提供します。 Oozie Web UI を使用して、ジョブの状態、ジョブ定義、構成、ジョブのアクションのグラフ、ジョブのログを表示できます。 また、ジョブのアクションの詳細を表示することもできます。

Oozie Web UI にアクセスするには、次の手順に従います。

1. HDInsight クラスターへの SSH トンネルを作成します。 これを行う方法については、次を参照してください。 [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)です。

2. トンネルが作成されたら、Web ブラウザーで Ambari Web UI を開きます。 Ambari サイトの URI は **https://CLUSTERNAME.azurehdinsight.net**します。 置換 **CLUSTERNAME** Linux ベースの HDInsight クラスターの名前に置き換えます。

3. ページの左側にある選択 **Oozie**, 、し **クイック リンク**, 、し、最後に **Oozie Web UI**します。

    ![メニューの画像](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Oozie Web UI では、実行中のワークフロー ジョブが既定で表示されます。 すべてのワークフロー ジョブを選択して **すべてのジョブ**します。

    ![すべてのジョブの表示](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. ジョブを選択すると、そのジョブの詳細が表示されます。

    ![ジョブ情報](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. [Job Info] タブでは、基本的なジョブ情報とジョブの個々のアクションを確認できます。 上部にあるタブを使用して、ジョブ定義の表示、ジョブ構成の表示、ジョブ ログへのアクセス、ジョブの有向非巡回グラフ (DAG) の表示を行うことができます。

    * **ジョブのログ**: を選択、 **GetLogs** ] ボタンをクリックすると、ジョブのすべてのログを取得または使用して、 **Enter Search Filter** ログをフィルター処理するフィールド

        ![ジョブ ログ](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: DAG は、ワークフローで取得されるデータ パスの概要をグラフィック

        ![ジョブの DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. アクションのいずれかを選択すると、 **Job Info** ] タブは、そのアクションの情報が表示されます。 たとえば、選択、 **RunHiveScript** アクション。

    ![アクション情報](./media/hdinsight-use-oozie-linux-mac/action.png)

8. リンクなど、アクションの詳細を表示、 **コンソールの URL**, 、これは、ジョブの JobTracker 情報を表示を使用できます。

##ジョブのスケジュール設定

コーディネーターを使用すると、ジョブの開始時刻、終了時刻、発生頻度を指定して、ジョブを特定の時間にスケジュールできます。

ワークフローのスケジュールを定義するには、次の手順に従います。

1. 次の使用という名前の新しいファイルを作成して **coordinator.xml**:

        nano coordinator.xml

    このファイルの内容として、次のコードを使用します。

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    このコードでは、ジョブ定義の値に置き換えられる `${...}` 変数を使用しています。 変数は次のとおりです。

    * **${coordfrequency}**: ジョブのインスタンスを実行するまでの時間
    * **${coordstart}**: ジョブの開始時刻
    * **${coordend}**: ジョブの終了時刻
    * **${coordtimezone}**: コーディネーター ジョブは固定タイム ゾーンと夏時間なし (通常は UTC を使用して表されます)。 このタイム ゾーンを、「Oozie 処理のタイムゾーン」と呼びます。
    * **${wfpath}**: workflow.xml のパス

2. CTRL + X を使用し、 **Y** と **Enter** ファイルを保存します。

3. 次のコマンドを使用して、コーディネーター ファイルをこのジョブの作業ディレクトリにコピーします。

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. 次を使用して、変更、 **job.xml** ファイル。

        nano job.xml

    次の変更を行います。

    * `<name>oozie.wf.application.path</name>` を `<name>oozie.coord.application.path</name>` に変更します。 これにより、ワークフロー ファイルではなく、コーディネーター ファイルを実行するよう Oozie に指示します。

    * 次のコードを追加して、coordinator.xml で使用される変数を、workflow.xml の場所を参照するように設定します。

            <property>
              <name>workflowPath</name>
              <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        値に置き換えて **mycontainer** と **mystorageaccount** 、job.xml ファイルの他のエントリで使用される値。

    * 次のコードを追加して、coordinator.xml ファイルで使用する開始時刻、終了時刻、頻度を定義します。

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        これらのコードでは、開始時刻を 2015 年 6 月 25 日午後 12 時に、終了時刻を 2015 年 6 月 27 日に、このジョブの実行間隔を毎日に設定しています (頻度は分単位であるため、24 時間 x 60 分 = 1440 分になります)。最後に、タイムゾーンを UTC に設定しています。

5. CTRL + X を使用し、 **Y** と **Enter** ファイルを保存します。

6. ジョブを実行するには、次のコマンドを使用します。

        oozie job -config job.xml -run

    これにより、ジョブが送信され、開始されます。

7. Oozie Web UI にアクセスを選択すると、 **コーディネーター ジョブ** ] タブで、次のような情報を作成する必要があります。

    ![[Coordinator Jobs] タブ](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    注、 **Next Materialization** エントリ。 これは、ジョブが次回実行時にします。

8. 以前のワークフロー ジョブと同様に、Web UI でジョブ エントリを選択すると、そのジョブの情報が表示されます。

    ![コーディネーター ジョブ情報](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    これは、スケジュールされたワークフロー内の個々のアクションではなく、ジョブの正常な実行のみを示しています。 参照してくださいのいずれかの選択、 **アクション** エントリです。 これにより、以前のワークフロー ジョブで取得された情報と同様の情報が表示されます。

    ![アクション情報](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##トラブルシューティング

Oozie UI では、Oozie ログと、Hive クエリなどの MapReduce タスクの JobTracker ログへのリンクの両方を簡単に表示できるので、Oozie ジョブに関する問題のトラブルシューティングを行うときに非常に役立ちます。 通常、トラブルシューティングのパターンは次のようになります。

1. Oozie Web UI でジョブを表示します。

2. エラーまたは特定のアクションのエラーがある場合は、かどうかをアクションを選択、 **エラー メッセージ** フィールドがエラーに関する詳細を提供します。

3. 使用できる場合は、アクションから URL を使用して、アクションの詳細 (JobTracker ログなど) を表示します。

発生する可能性のあるエラーとその解決方法を以下に示します。

###JA009: Cannot initialize cluster (クラスターを初期化できません)

**現象**: ジョブの状態が変わります **保留**します。 詳細に、RunHiveScript の状態としては、ジョブの表示を表示 **START_MANUAL**します。 アクションを選択すると、次のエラー メッセージが表示されます。

    JA009: Cannot initialize Cluster. Please check your configuration for map

**原因**: で使用される WASB アドレス、 **job.xml** ストレージ コンテナーまたはストレージ アカウント名を含まないファイルです。 WASB アドレスの形式は、`wasb://containername@storageaccountname.blob.core.windows.net` である必要があります。

**解像度**: ジョブで使用される WASB アドレスを変更します。

###JA002: Oozie is not allowed to impersonate &lt;USER> (Oozie は &lt;USER> の偽装を許可されていません)

**現象**: ジョブの状態が変わります **保留**します。 詳細に、RunHiveScript の状態としては、ジョブの表示を表示 **START_MANUAL**します。 アクションを選択すると、次のエラー メッセージが表示されます。

    JA002: User: oozie is not allowed to impersonate <USER>

**原因**: 現在の権限設定を指定したユーザー アカウントを借用する Oozie のことを許可できません。

**解像度**: Oozie がユーザーを偽装できる、 **ユーザー** グループです。 `groups USERNAME` を使用して、ユーザー アカウントがメンバーとして属するグループを確認します。 ユーザーがのメンバーでない場合、 **ユーザー** グループで、次のコマンドを使用して、グループに、ユーザーを追加します。

    sudo adduser USERNAME users

> [AZURE.NOTE] HDInsight は、ユーザーがグループに追加されたことを認識するのに数分かかることがあります。

###Launcher ERROR (ランチャー エラー) (Sqoop)

**現象**: ジョブの状態に変更 **KILLED**します。 詳細にジョブは、RunSqoopExport の状態が表示されます **エラー**します。 アクションを選択すると、次のエラー メッセージが表示されます。

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**原因**: Sqoop が、データベースにアクセスするために必要なデータベース ドライバーを読み込むことができます。

**解像度**: Sqoop、Oozie ジョブからを使用する場合は、ジョブの使用 (workflow.xml) など、他のリソースとデータベース ドライバーを含める必要があります。

また、workflow.xml の `<sqoop>...</sqoop>` セクションから、データベース ドライバーが格納されたアーカイブを参照する必要があります。

たとえば、このドキュメントのジョブの場合、次の手順に従います。

1. sqljdbc4.jar ファイルを /tutorials/useoozie ディレクトリにコピーします。

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc4.jar /tutorials/useoozie/sqljdbc4.jar

2. workflow.xml を変更して、`</sqoop>` の上の新しい行に次の内容を追加します。

        <archive>sqljdbc4.jar</archive>

##次のステップ
このチュートリアルでは、Oozie ワークフローを定義する方法と Oozie ジョブを実行する方法について説明しました。 HDInsight の使用方法の詳細については、次の記事をご覧ください。

- [HDInsight での時間ベースの Oozie コーディネーターの使用][hdinsight oozie コーディネーター時]
- [HDInsight での Hadoop ジョブ用データのアップロード][hdinsight でのデータのアップロード]
- [HDInsight での Hadoop Sqoop の使用][hdinsight での sqoop の使用]
- [HDInsight での Hadoop の Hive の使用][hdinsight での hive の使用]
- [HDInsight での Hadoop での Pig の使用][hdinsight での pig の使用]
- [HDInsight 用 Java MapReduce プログラムの開発][hdinsight 開発 mapreduce]


[hdinsight コマンドレットのダウンロード]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure のデータの工場出荷時の pig-hive]: data-factory-pig-hive-activities.md
[hdinsight oozie コーディネーター時]: hdinsight-use-oozie-coordinator-time.md
[。hdinsight バージョン]:  hdinsight-component-versioning.md
[hdinsight ストレージ]: hdinsight-use-blob-storage.md
[hdinsight の入門]: hdinsight-get-started.md


[hdinsight での sqoop の使用]: hdinsight-use-sqoop-mac-linux.md
[hdinsight のプロビジョニング]: hdinsight-provision-clusters.md
[hdinsight でのデータのアップロード]: hdinsight-upload-data.md
[hdinsight での mapreduce の使用]: hdinsight-use-mapreduce.md
[hdinsight での hive の使用]: hdinsight-use-hive.md
[hdinsight での pig の使用]: hdinsight-use-pig.md
[hdinsight ストレージ]: hdinsight-use-blob-storage.md
[hdinsight get-emulator の概要]: hdinsight-get-started-emulator.md

[hdinsight-開発-ストリーミングのジョブ]: hdinsight-hadoop-develop-deploy-streaming-jobs.md
[hdinsight 開発 mapreduce]: hdinsight-develop-deploy-java-mapreduce.md

[sql database を作成構成]: sql-database-create-configure.md
[sql database 取得開始]: sql-database-get-started.md

[azure 作成 storageaccount]: storage-create-storage-account.md

[apache hadoop]: http://hadoop.apache.org/
[apache oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell のダウンロード]: http://azure.microsoft.com/downloads/
[powershell のプロファイルについて]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell のインストール構成]: powershell-install-configure.md
[powershell 開始]: http://technet.microsoft.com/library/hh847889.aspx
[powershell スクリプト]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross hive テーブル]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img ワークフロー図]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img 準備出力]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img runworkflow 出力]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki hive エラー]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

<!--HONumber=Apr16_HO2-->

<properties
   pageTitle="Linux ベースの HDInsight で Hadoop を使用するためのヒント | Microsoft Azure"
   description="ここには、Azure クラウドで稼動する使い慣れた Linux 環境で、Linux ベースの HDInsight (Hadoop) クラスターを使用するための実装上のヒントを記載します。"
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
   ms.date="12/04/2015"
   ms.author="larryfr"/>

# Linux での HDInsight の使用方法

Linux ベースの Azure HDInsight クラスターは、Azure クラウドで実行される使い慣れた Linux 環境での Hadoop を提供します。 使い方は、ほとんどの点で、Linux 環境の他の Hadoop とまったく同じです。 ここでは、知っておく必要がある特定の違いについて説明します。

## ドメイン名

インターネットからクラスターに接続するときに使用する完全修飾ドメイン名 (FQDN) は **& lt; clustername >. >.azurehdinsight.net** または (SSH のみ) **(& a) lt; clustername-ssh >. >.azurehdinsight.net**します。

内部的には、クラスターの各ノードに、クラスターの構成時に割り当てられた名前が与えられます。 クラスター名を検索するには、お客様がアクセスできる、 __ホスト__ 、Ambari Web UI に] ページまたは次を使用して、Ambari REST API を使用して、ホストの一覧を返す [cURL](http://curl.haxx.se/) と [jq](https://stedolan.github.io/jq/):

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

置換 __パスワード__ 管理者アカウントのパスワードを使用して、 __CLUSTERNAME__ 、クラスターの名前に置き換えます。 これで返される JSON ドキュメントにクラスター内のホストの一覧が含まれます。jq により、クラスター内の各ホストの `host_name` 要素値が取り出されます。

特定のサービスのノード名を見つける必要がある場合、そのコンポーネントについて Ambari に問い合わせることができます。 たとえば、HDFS 名のノードのホストを見つけるには、次の方法を利用します。

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

これでサービスの説明が記載された JSON ドキュメントが返されます。jq により、ホストの `host_name` 値のみが引き出されます。

## サービスへのリモート アクセス

* **Ambari (web)** -https://&lt;clustername>.azurehdinsight.net

    クラスター管理者のユーザー名とパスワードを使用して認証した後、Ambari にログインします。 その際も、クラスター管理者のユーザー名とパスワードを使用します。

    認証はプレーンテキストです。接続をセキュリティで確実に保護するために、常に HTTPS を使用してください。

    > [AZURE.IMPORTANT] クラスター用の Ambari は、インターネット経由で直接アクセスできますが、一部の機能は、クラスターで使用する内部ドメイン名によってノードにアクセスするのには。 これは内部ドメイン名で、パブリックではないため、インターネット経由で機能にアクセスしようとすると、サーバーが見つからないことを示すエラーが発生します。
    >
    > Ambari Web UI の全機能を使用するには、SSH トンネルを使用して、クラスター ヘッド ノードに対する Web トラフィックがプロキシ経由になるようにします。 参照してください [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスするには](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;clustername>.azurehdinsight.net/ambari

    > [AZURE.NOTE] クラスター管理者のユーザーとパスワードを使用して認証します。
    >
    > 認証はプレーンテキストです。接続をセキュリティで確実に保護するために、常に HTTPS を使用してください。

* **WebHCat (Templeton)** -https://&lt;clustername>.azurehdinsight.net/templeton

    > [AZURE.NOTE] クラスター管理者のユーザーとパスワードを使用して認証します。
    >
    > 認証はプレーンテキストです。接続をセキュリティで確実に保護するために、常に HTTPS を使用してください。

* **SSH** -& lt; clustername > のポート 22 または 23 で ssh.azurehdinsight.net します。 ポート 22 はヘッド ノード 0 への接続に、23 はヘッド ノード 1 への接続に使用されます。 ヘッド ノードの詳細については、次を参照してください。 [HDInsight クラスターの Hadoop の可用性と信頼性](hdinsight-high-availability-linux.md)します。

    > [AZURE.NOTE] クラスターのヘッド ノードは、クライアント コンピューターから SSH を通じてのみアクセスできます。 接続されたら、ヘッド ノードから SSH を使用して worker ノードにアクセスできます。

## ファイルの場所

Hadoop 関連ファイルは、`/usr/hdp` のクラスター ノードにあります。 このディレクトリには、次のサブディレクトリが含まれます。

* __2.2.4.9-1__: このディレクトリの名前は、クラスターの数は、ここに表示されているものと異なる場合がありますので、HDInsight で使用される Hortonworks Data Platform のバージョンに対応します。
* __現在__: このディレクトリは、下にあるディレクトリへのリンクを含む、 __2.2.4.9-1__ ディレクトリ、ファイルにアクセスするたびに (つまり、変更があります) のバージョン番号を入力する必要があるないようにが存在するとします。

サンプル データ ファイルと JAR ファイルは、Hadoop 分散ファイル システム (HDFS) または Azure BLOB ストレージの '/example' または 'wasb:///example' にあります。

## HDFS、Azure BLOB ストレージ、ストレージのベスト プラクティス

ほとんどの Hadoop ディストリビューションでは、HDFS はクラスター内のコンピューターのローカル ストレージによって支えられています。 これは効率的である一方、コンピューティング リソースが時間単位で課金されるクラウド ベースのソリューションでは、コストが高くなります。

HDInsight は Azure BLOB ストレージを既定の保管場所として使用します。これには次の利点があります。

* 低コストの長期ストレージ

* Websites、ファイル アップロード/ダウンロード ユーティリティ、さまざまな言語の SDK、Web ブラウザーなどの外部サービスからアクセスできます。

HDInsight の既定の保管場所であるため、通常は何もしなくても使用できます。 次のコマンドがのファイルを一覧表示など、 **/例/データ** Azure Blob ストレージに格納されているフォルダー。

    hadoop fs -ls /example/data

一部のコマンドでは、BLOB ストレージを使用することを指定する必要があります。 指定してコマンドをプレフィックスをこれらの **WASB://**します。

HDInsight では、クラスターに複数の BLOB ストレージ アカウントを関連付けることもできます。 既定以外の Blob ストレージ アカウント上のデータにアクセスするには、形式を使用することができます **WASB://& lt; コンテナー名 > @& lt; アカウント名 >.blob.core.windows.net/**します。 たとえば、次がの内容を一覧、 **/例/データ** ディレクトリに指定したコンテナーと Blob ストレージ アカウント。

    hadoop fs -ls wasb://mycontainer@mystorage.blob.core.windows.net/example/data

### クラスターで使用している BLOB ストレージ

クラスター作成時、既存の Azure ストレージ アカウントとコンテナーを使用するか、新しく作成するかを選択します。 後になって、何を選択したかを忘れることもあります。 Ambari REST API を使用して、既定のストレージ アカウントとコンテナーを調べることができます。

1. 次のコマンドを使用して、HDFS の構成情報を取得します。

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1"

2. 返された JSON データで、`fs.defaultFS` エントリを見つけます。 これには、既定のコンテナーとストレージ アカウント名が次のような形式で含まれています。

        wasb://CONTAINTERNAME@STORAGEACCOUNTNAME.blob.core.windows.net

    > [AZURE.TIP] インストールした場合 [jq](http://stedolan.github.io/jq/), 、次を使用するを返すだけ `fs.defaultFS` エントリ。
    >
    > `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'`

3. ストレージ アカウントの認証に使用するキー、または、クラスターに関連付けられているセカンダリ ストレージ アカウントを見つけるために、次のコマンドを使用します。

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1"

4. 返された JSON データで、`fs.azure.account.key` で始まるエントリを見つけます。 エントリ名の残りの部分は、ストレージ アカウント名です。 たとえば、「`fs.azure.account.key.mystorage.blob.core.windows.net`」のように入力します。 このエントリに格納されている値が、ストレージ アカウントの認証に使用するキーです。

    > [AZURE.TIP] インストールした場合 [jq](http://stedolan.github.io/jq/), 、キーと値の一覧を返す、次を使用することができます。
    >
    > `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties as $in | $in | keys[] | select(. | contains("fs.azure.account.key.")) as $item | $item | ltrimstr("fs.azure.account.key.") | { storage_account: ., storage_account_key: $in[$item] }'`

次のように Azure ポータルを使用して、ストレージ情報を検索することもできます。

1.  [Azure ポータル](https://portal.azure.com/), 、HDInsight クラスターを選択します。

2.  __Essentials__ ] セクションの [ __設定をすべて__です。

3.  __設定__, [ __Azure ストレージ キー__します。

4.  __Azure ストレージ キー__, 、表示されているストレージ アカウントのいずれかを選択します。 これにより、ストレージ アカウントに関する情報が表示されます。

5. キーのアイコンを選択します。 これにより、このストレージ アカウントのキーが表示されます。

### BLOB ストレージにアクセスする方法

クラスターから Hadoop コマンドを使用する以外にも、BLOB にアクセスするさまざまな方法があります。

* [Mac、Linux および Windows 用の azure CLI](../xplat-cli-install.md): Azure を使用するためのコマンド ライン インターフェイス コマンドします。 インストール後、ストレージの使用方法については `azure storage`、BLOB 特有のコマンドについては `azure blob` をご覧ください。

* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): Azure ストレージに blob を使用するための python スクリプトします。

* さまざまな SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [ストレージ REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>クラスターのスケーリング

クラスターのスケール設定機能を使用すると、Azure HDInsight で実行しているクラスターによって使用されるデータ ノードの数を、クラスターを削除して再作成することなく、変更できます。

クラスターで他のジョブまたはプロセスを実行している間にスケーリング操作を実行できます。

別のクラスターの種類は、次のようにスケーリングすることによって影響を受けます。

* __Hadoop__: クラスター内のノードの数を減らすと、一部のクラスター内のサービスが再起動します。 これにより、スケーリング操作の完了時に、実行中および保留中のジョブが失敗する可能性があります。 ただし、操作が完了した時点で、ジョブを再送信できます。

* __HBase__: 地域サーバーは、スケール設定処理が完了した後、数分以内に自動的に分散します。 リージョンのサーバーを手動で調整するには、次の手順を使用します。

    1. SSH を使用して HDInsight クラスターに接続します。 HDInsight での SSH の使用方法の詳細については、次のいずれかのドキュメントをご覧ください。

        * [Linux、Unix、Mac OS X から HDInsight で SSH を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Windows から HDInsight で SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. HBase シェルを開始するには、次のコマンドを使用します。

            hbase shell

    2. HBase シェルが読み込まれたら、次のコマンドを使用してリージョンのサーバーを手動で調整します。

            balancer

* __Storm__: コントロールがスケーリング操作が実行された後に、実行中の Storm トポロジを再調整する必要があります。 これにより、トポロジはクラスター内のノードの新しい数に基づいて並列処理の設定を再調整できます。 実行中のトポロジを再調整するには、次のオプションのいずれかを使用します。

    * __SSH__: サーバーに接続し、次のコマンドを使用してトポロジを再調整します。

            storm rebalance TOPOLOGYNAME

        また、トポロジによって最初に提供された並列処理のヒントを上書きするパラメーターも指定できます。 たとえば、`storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` は、トポロジを再構成して、5 個の worker プロセス、blue-spout コンポーネント用の 3 個の実行プログラム、yellow-bolt コンポーネント用の 10 個の実行プログラムにします。

    * __Storm UI__: Storm UI を使用してトポロジを再調整し、次の手順を使用します。

        1. [クラスターへの SSH トンネルを作成し、Ambari web UI を開きます](hdinsight-linux-ambari-ssh-tunnel.md)します。

        2. ページの左上のサービスのリストから選択 __Storm__します。 選択し、 __Storm UI__ から __クイック リンク__します。


            ![Storm UI entry in quick links](./media/hdinsight-hadoop-linux-information/ambari-storm.png)

            This will display the Storm UI:

            ![the storm ui](./media/hdinsight-hadoop-linux-information/storm-ui.png)

        3. Select the topology you wish to rebalance, then select the __Rebalance__ button. Enter the delay before the rebalance operation is performed.

HDInsight クラスターのスケーリングに関する具体的な情報については、以下を参照してください。

* [Azure ポータルを使用した HDInsight での Hadoop クラスターの管理](hdinsight-administer-use-portal-linux.md#scaling)

* [Azure PowerShell を使用した HDInsight での Hadoop クラスターの管理](hdinsight-administer-use-command-line.md#scaling)

## Hue (またはその他の Hadoop コンポーネント) のインストール方法

HDInsight は、管理されたサービスです。つまり、問題が検出された場合、Azure によってクラスター内のノードは自動的に破棄されて、再プロビジョニングされる可能性があります。 そのため、クラスター ノードにコンポーネントを直接、手動でインストールすることは推奨されません。 代わりに、 [HDInsight Script Action](hdinsight-hadoop-customize-cluster.md) 、以下をインストールする必要がある場合。

* Spark や Hue など、サービスや Web サイト。
* クラスターの複数のノードで構成変更を必要とするコンポーネント。 たとえば、必要な環境変数、ログ ディレクトリの作成、構成ファイルの作成。

スクリプト アクションとは、クラスターのプロビジョニング中に実行される Bash スクリプトで、クラスター上に追加のコンポーネントをインストールし、構成するために使用できます。 次のコンポーネントをインストールするスクリプトの例が用意されています。

* [Hue](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [R](hdinsight-hadoop-r-scripts-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)
* [Spark](hdinsight-hadoop-spark-install-linux.md)

独自の Script Action の開発方法の詳細については、次を参照してください。 [HDInsight での Script Action 開発](hdinsight-hadoop-script-actions-linux.md)します。

###Jar ファイル

一部の Hadoop 技術は自己完結型の jar ファイルで提供されます。このファイルには MapReduce ジョブの一環として、あるいは Pig または Hive 内から使用される関数が含まれています。 ファイルはスクリプト アクションでインストールできますが、多くの場合、セットアップを必要とせず、プロビジョニング後にクラスターにアップロードし、直接使用できます。 クラスターの再イメージ化の後もコンポーネントが残っていることを確認するには、WASB に jar ファイルを保存します。

たとえばの最新バージョンを使用する [DataFu](http://datafu.incubator.apache.org/), 、プロジェクトを含む jar をダウンロードして、HDInsight クラスターにアップロードします。 その後、Pig や Hive からそれを使用する方法については、DataFu のドキュメントに従います。

> [AZURE.IMPORTANT] Jar ファイルをスタンドアロンである一部のコンポーネントは、HDInsight に付属しているが、パスではありません。 特定のコンポーネントを探している場合、次を利用し、クラスターでそれを検索できます。
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> 一致する jar ファイルがあれば、そのパスが返されます。

クラスターがスタンドアロン jar ファイルとしてあるバージョンのコンポーネントを提供するが、別のバージョンを希望する場合、クラスターに新しいバージョンのコンポーネントをアップロードし、ジョブでそれを試すことができます。

> [AZURE.WARNING] HDInsight クラスターと共に提供されるコンポーネントは完全にサポートしを特定し、これらのコンポーネントに関連する問題を解決するには Microsoft サポートが支援します。
>
> カスタム コンポーネントについては、問題のトラブルシューティングを進めるための支援として、商業的に妥当な範囲のサポートを受けることができます。 これにより問題が解決する場合もあれば、オープン ソース テクノロジに関して、深い専門知識が入手できる場所への参加をお願いすることになる場合もあります。 たとえば、使用できますが、このような数多くのコミュニティ サイトがある: [HDInsight についての MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), 、[http://stackoverflow.com](http://stackoverflow.com)します。 Apache プロジェクトがプロジェクトのサイトを持つも [http://apache.org](http://apache.org), 、たとえば: [Hadoop](http://hadoop.apache.org/), 、[Spark](http://spark.apache.org/)します。

## 次のステップ

* [HDInsight での Hive の使用](hdinsight-use-hive.md)
* [HDInsight での Pig の使用](hdinsight-use-pig.md)
* [HDInsight での MapReduce ジョブの使用](hdinsight-use-mapreduce.md)



<properties
   pageTitle="Apache Ambari REST API を使用して HDInsight クラスターを監視および管理する |Microsoft Azure"
   description="Ambari を使用して Linux ベースの HDInsight クラスターを監視および管理する方法を説明します。 このドキュメントでは、HDInsight クラスターに含まれている Ambari REST API を使用する方法について説明します。"
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

#Ambari REST API を使用した HDInsight クラスターの管理

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari には使いやすい Web UI と REST API が用意されているため、Hadoop クラスターを簡単に管理および監視できます。 Linux ベースの HDInsight クラスターに含まれている Ambari は、クラスターの監視と構成の変更を行うために使用します。 このドキュメントでは、クラスター ノードの完全修飾ドメイン名の検索、クラスターによって使用されている既定のストレージ アカウントの検索などの一般的なタスクを実行することによって、Ambari REST API の操作の基本を学習します。

> [AZURE.NOTE] この記事の情報は、Linux ベースの HDInsight クラスターにのみ適用されます。 Windows ベースの HDInsight クラスターでは、Ambari REST API で利用できる監視機能の一部のみを利用できます。 参照してください [Ambari API を使用した HDInsight でモニターの Windows ベースの Hadoop](hdinsight-monitor-use-ambari-api.md)します。

##前提条件

* [cURL](http://curl.haxx.se/): cURL は、コマンドラインから REST Api を使用するために使用するクロスプラット フォーム ユーティリティです。 このドキュメントでは、Ambari REST API との通信にこれを使用しています。
* [jq](https://stedolan.github.io/jq/): jq は JSON ドキュメントを操作するためのクロスプラット フォーム コマンド ライン ユーティリティです。 このドキュメントでは、Ambari REST API から返される JSON ドキュメントの解析にこれを使用しています。

##<a id="whatis"></a>Ambari とは

<a href="http://ambari.apache.org" target="_blank">Apache Ambari</a> は、Hadoop クラスターのプロビジョニング、管理、監視に使用する Web UI を簡単に使用できる方法を提供することで Hadoop の管理を簡略化します。 開発者は、<a href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md" target="_blank">Ambari REST API</a> を使用して、これらの機能をアプリケーションに統合することができます。

Ambari は既定で Linux ベースの HDInsight クラスターに付属しています。

##REST API

HDInsight での Ambari REST API のベース URI は https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME、ここで __CLUSTERNAME__ 、クラスターの名前を指定します。 

> [AZURE.IMPORTANT] HDInsight での Ambari に接続するには、HTTPS が必要です。 管理者のアカウント名を使用して ambari も認証する必要があります (既定値は __admin__,、) と、クラスターが作成されたときに指定したパスワードです。

cURL を使用して REST API に対する GET 要求を実行する例を次に示します。

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
これを実行する場合は、置換 __パスワード__ 、クラスターの管理者パスワードを使用しておよび __CLUSTERNAME__ クラスター名と、次のような情報で始まる JSON ドキュメントを受け取ります。

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

これは JSON であるため、一般に、データを取得するには、JSON パーサーを使用するのが簡単です。 たとえば、アラートの数を取得する (に含まれている、 __「ホスト/host_status/アラート」__ 要素) 以下を使用するには、値に直接アクセスします。

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report."Host/host_status/ALERT"'
    
これは、JSON ドキュメントを取得し、jq への出力のパイプを使用します。 `'.Clusters.health_report."Host/host_status/ALERT"'` 取得する JSON ドキュメント内の要素を示します。

> [AZURE.NOTE]  __ホスト/host_status/アラート__ 要素を示すために引用符で囲みます '/' は要素名の一部です。 Jq の使用に関する詳細については、次を参照してください。、 [jq web サイト](https://stedolan.github.io/jq/)します。

##例: クラスター ノードの FQDN を取得する

HDInsight を使用する際は、クラスター ノードの完全修飾ドメイン名 (FQDN) を知ることが必要になる場合があります。 次のコードを使用して、クラスター内のさまざまなノードの FQDN を簡単に取得できます。

* __ヘッド ノード__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __ワーカー ノード__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper ノード__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

いずれのコードも同じパターンに従っています。つまり、これらのノードで実行されていることがわかっているコンポーネントを照会し、これらのノードの FQDN が含まれている `host_name` 要素を取得しています。

返されるドキュメントの `host_components` 要素には、複数の項目が含まれています。 `.host_components[]` を使用して、要素内のパスを指定すると、各項目に対してループ処理が実行され、特定のパスから値が抽出されます。 最初の FQDN エントリなど、1 つの値のみが必要な場合は、項目をコレクションとして返し、特定のエントリを選択することができます。

    jq '[.host_components[].HostRoles.host_name][0]'

このコードは、最初の FQDN をコレクションから返します。

##例: 既定のストレージ アカウントとコンテナーを取得する

HDInsight クラスターを作成する場合は、Azure ストレージ アカウントと、クラスターの既定のストレージとして BLOB コンテナーを使用する必要があります。 Ambari を使用すると、クラスターが作成された後にこの情報を取得できます。 たとえば、プログラムでコンテナーに直接データを書き込むとします。

次のコードは、クラスターの既定のストレージの WASB URI を取得します。

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] サーバーに適用される最初の構成が返されます (`service_config_version=1`,、) がこの情報が含まれます。 クラスターの作成後に変更された値を取得する場合は、構成のバージョンを一覧表示した後で最新の構成を取得することが必要になる場合があります。

次のような値を返すこれは、 __コンテナー__ 既定のコンテナーですと __ACCOUNTNAME__ Azure ストレージ アカウントの名前を指定します。

    wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net

この情報を使用することができますし、 [Azure CLI](../xplat-cli-install.md) をアップロードまたはコンテナーからデータをダウンロードします。 次に例を示します。

1. ストレージ アカウントのリソース グループを取得します。 置換 __ACCOUNTNAME__ Ambari から取得したストレージ アカウント名。

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    アカウントのリソース グループ名が返されます。
    
2. ストレージ アカウントのキーを取得します。 置換 __GROUPNAME__ 前の手順からリソース グループにします。 置換 __ACCOUNTNAME__ をストレージ アカウント名。

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    アカウントのプライマリ キーが返されます。
    
3. upload コマンドを使用してファイルをコンテナーに保存します。

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    置換 __ACCOUNTNAME__ をストレージ アカウント名。 置換 __ACCOUNTKEY__ 以前取得したキーを使用しています。 __FILEPATH__ 、アップロードするファイルへのパスは、中に __BLOBPATH__ コンテナー内のパスです。

    たとえば、wasb://example/data/filename.txt にし、HDInsight で表示するファイル __BLOBPATH__ は `example/data/filename.txt`です。

##次のステップ

REST API の完全なリファレンスを参照してください。 [Ambari API リファレンス V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)します。

> [AZURE.NOTE] 一部の Ambari 機能が無効になって、HDInsight クラウド サービスが管理しています。追加するか、クラスターからホストを削除するなど、新しいサービスを追加します。


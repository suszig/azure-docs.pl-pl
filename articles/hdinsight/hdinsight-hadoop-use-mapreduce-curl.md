<properties
   pageTitle="HDInsight での MapReduce と Hadoop の Curl の使用 | Microsoft Azure"
   description="Curl を使用して HDInsight の Hadoop で MapReduce ジョブをリモートで実行する方法を説明します。"
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


# Curl を使用して HDInsight の Hadoop で MapReduce ジョブを実行

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

このドキュメントでは、Curl を使用して HDInsight クラスターの Hadoop で MapReduce ジョブを実行する方法を説明します。

Curl は、未加工の HTTP 要求を使用して HDInsight とやり取りし、MapReduce ジョブを実行する方法を示すために使用します。 これは、HDInsight クラスターで提供される WebHCat REST API (旧称: Templeton) を使用することで機能します。
> [AZURE.NOTE] Linux ベースの Hadoop サーバーの使用に慣れて既に HDInsight に慣れていない場合、 [HDInsight での Linux ベースの Hadoop について知っておく必要がある](hdinsight-hadoop-linux-information.md)します。

## <a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* HDInsight クラスター (Linux または Windows ベース) の Hadoop

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Curl を使用して MapReduce ジョブを実行します。

> [AZURE.NOTE] Curl、または WebHCat を使用したその他の REST 通信が使用できる場合は、HDInsight クラスター管理者のユーザー名とパスワードを指定して要求を認証する必要があります。 また、サーバーへの要求の送信に使用する URI にクラスター名を含める必要があります。
>
> このセクションのコマンドでは、**USERNAME** をクラスターを認証するユーザーの名前に、**PASSWORD** をユーザー アカウントのパスワードに置き換えてください。 **CLUSTERNAME** をクラスターの名前に置き換えます。
>
> 使用して REST API がセキュリティで保護された [基本アクセス認証](http://en.wikipedia.org/wiki/Basic_access_authentication)します。 資格情報を安全にサーバーに送信するには、必ず HTTPS を使用して要求を行う必要があります。

1. コマンド ラインで次のコマンドを使用して、HDInsight クラスターに接続できることを確認します。

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    次のような応答を受け取ります。

        {"status":"ok","version":"v1"}

    このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-u**: 要求の認証に使用するユーザー名とパスワード
    * **-G**: GET 要求であることを示します。

    URI の先頭 **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, 、すべての要求に対して同じです。

2. MapReduce ジョブを送信するには、次のコマンドを使用します。

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasb:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasb:///example/data/gutenberg/davinci.txt -d arg=wasb:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    URI の末尾 (/mapreduce/jar) により、この要求では jar ファイルのクラスから MapReduce ジョブが起動されることが WebHCat に通知されます。 このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-d**: `-g` は使用しないように、POST メソッドに、要求は既定です。 `-d` 要求で送信されるデータ値を指定します。

        * **user.name**: コマンドを実行するユーザー
        * **jar**: 実行されるクラスを含む jar ファイルの場所
        * **class**: MapReduce ロジックを含むクラス
        * **arg**: MapReduce ジョブに渡される引数で、この場合は、入力テキスト ファイルと出力に使用するディレクトリになります。

    このコマンドは、ジョブのステータスの確認に使用できる ジョブ ID を返します。

        {"id":"job_1415651640909_0026"}

3. ジョブのステータスを確認するには、次のコマンドを使用します。 **JOBID** を前の手順で返された値に置き換えます。 たとえば、戻り値が `{"id":"job_1415651640909_0026"}`, 、JOBID になりますし、 `job_1415651640909_0026`します。

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    ジョブが完了している場合、ステータスは "SUCCEEDED" になります。
    > [AZURE.NOTE] この Curl 要求では、ジョブに関する情報が記載された JSON ドキュメントが返されます。状態値のみを取得するには jq を使用します。

4. ジョブのステータスが **SUCCEEDED** に変わったら、Azure BLOB ストレージからジョブの結果を取得できます。  `Statusdir` クエリで渡されるパラメーターには、ここでは、出力ファイルの場所が含まれています。 **wasb:////例/curl**します。 このアドレスではジョブの出力は、HDInsight クラスターが使用する既定のストレージ コンテナーの **example/curl** ディレクトリに保存されます。

一覧表示しを使用してこれらのファイルをダウンロードすることができます、 [Mac、Linux および Windows 用 Azure CLI](../xplat-cli-install.md)します。 たとえば、**example/curl** 内のファイルを一覧表示するには、次のコマンドを使用します。

    azure storage blob list <container-name> example/curl

ファイルをダウンロードするには、次のコマンドを使用します。

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] 使用して blob を格納するストレージ アカウント名を指定する必要があります、 `-a` と `-k` パラメーター、またはセット、 **\_storage\_account** と **azure \_storage\_access\_key** 環境変数です。 参照してください [データを HDInsight にアップロードする方法](hdinsight-upload-data.md) の詳細。

## <a id="summary"></a>概要

このドキュメントを参照して、未加工の HTTP 要求を使用して、HDInsight クラスターで Hive ジョブを実行、監視し、その結果を表示できます。

この記事で使用されている REST インターフェイスの詳細については、次を参照してください。、 [WebHCat リファレンス](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)します。

## <a id="nextsteps"></a>次のステップ

HDInsight での MapReduce ジョブに関する全般的な情報:

* [HDInsight での Hadoop の MapReduce を使用します。](hdinsight-use-mapreduce.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hadoop の Hive を使用します。](hdinsight-use-hive.md)

* [HDInsight での Hadoop での Pig の使用](hdinsight-use-pig.md)






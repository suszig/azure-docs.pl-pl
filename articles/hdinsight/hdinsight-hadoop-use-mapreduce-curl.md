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

#Curl を使用して HDInsight の Hadoop で MapReduce ジョブを実行

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

このドキュメントでは、Curl を使用して HDInsight クラスターの Hadoop で MapReduce ジョブを実行する方法を説明します。

Curl は、未加工の HTTP 要求を使用して HDInsight とやり取りし、MapReduce ジョブを実行する方法を示すために使用します。 これは、HDInsight クラスターで提供される WebHCat REST API (旧称: Templeton) を使用することで機能します。

> [AZURE.NOTE] Linux ベースの Hadoop サーバーの使用に慣れて既に HDInsight に慣れていない場合、 [HDInsight での Linux ベースの Hadoop について知っておく必要がある](hdinsight-hadoop-linux-information.md)です。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* HDInsight クラスター (Linux または Windows ベース) の Hadoop

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Curl を使用した MapReduce ジョブの実行

> [AZURE.NOTE] WebHCat で Curl、またはその他の REST 通信を使用する場合は、HDInsight クラスター管理者のユーザー名とパスワードを提供することで要求を認証する必要があります。 また、サーバーへの要求の送信に使用する URI にクラスター名を含める必要があります。
>
> このセクションでのコマンド **ユーザー名** をクラスターを認証するユーザーと **パスワード** ユーザー アカウントのパスワードを使用しています。 置換 **CLUSTERNAME** 、クラスターの名前に置き換えます。
>
> REST API のセキュリティは、[基本アクセス認証](http://en.wikipedia.org/wiki/Basic_access_authentication)の使用によって保護されています。 資格情報を安全にサーバーに送信するには、必ず HTTPS を使用して要求を行う必要があります。

1. コマンド ラインで次のコマンドを使用して、HDInsight クラスターに接続できることを確認します。

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    次のような応答を受け取ります。

        {"status":"ok","version":"v1"}

    このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-u**: ユーザー名とパスワードが要求の認証に使用されることを示します
    * **-G**: GET 要求であることを示します

    URI の先頭 **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, 、すべての要求に対して同じです。

2. MapReduce ジョブを送信するには、次のコマンドを使用します。

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasb:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasb:///example/data/gutenberg/davinci.txt -d arg=wasb:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    URI の末尾 (/mapreduce/jar) により、この要求では jar ファイルのクラスから MapReduce ジョブが起動されることが WebHCat に通知されます。 このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-d**: `-G` は使用しないように、POST メソッドに、要求は既定です。 `-d` 要求と共に送信されるデータ値を指定します。

        * **user.name**: コマンドを実行しているユーザー
        * **jar**: 実行されるクラスを含む jar ファイルの場所
        * **クラス**: MapReduce ロジックを含むクラス
        * **arg**: 出力に使用される MapReduce ジョブにはここでは、入力テキスト ファイルおよびディレクトリ内に渡される引数

    このコマンドは、ジョブのステータスの確認に使用できる ジョブ ID を返します。

        {"id":"job_1415651640909_0026"}

3. ジョブのステータスを確認するには、次のコマンドを使用します。 置き換える、 **JOBID** 前の手順で返される値を使用します。 たとえば、戻り値が `{"id":"job_1415651640909_0026"}` の場合、JOBID は `job_1415651640909_0026` になります。

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    ジョブが完了している場合、ステータスは "SUCCEEDED" になります。

    > [AZURE.NOTE] この Curl 要求には、ジョブに関する情報を含む JSON ドキュメントが返されますjq を使用して、状態値のみを取得します。

4. ジョブの状態が変更された時期 **SUCCEEDED**, 、Azure Blob ストレージからジョブの結果を取得できます。  `statusdir` クエリで渡されるパラメーターには、ここでは、出力ファイルの場所が含まれています。 **wasb:////例/curl**します。 このアドレスではジョブの出力を格納する、 **例/curl** ディレクトリに、HDInsight クラスターで使用される既定のストレージ コンテナー。

一覧表示しを使用してこれらのファイルをダウンロードすることができます、 [Mac、Linux および Windows 用 Azure CLI](../xplat-cli-install.md)します。 たとえば、ファイルを一覧表示、 **/curl の使用例**, 、次のコマンドを使用します。

    azure storage blob list <container-name> example/curl

ファイルをダウンロードするには、次のコマンドを使用します。

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] 使用して blob を格納するストレージ アカウント名を指定する必要があります、 `-a` と `-k` パラメーター、またはセット、 **azure \_storage\_account** と **azure \_storage\_access\_key** 環境変数です。 参照してください [データを HDInsight にアップロードする方法](hdinsight-upload-data.md) の詳細。

##<a id="summary"></a>概要

このドキュメントを参照して、未加工の HTTP 要求を使用して、HDInsight クラスターで Hive ジョブを実行、監視し、その結果を表示できます。

この記事で使用されている REST インターフェイスの詳細については、次を参照してください。、 [WebHCat リファレンス](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)します。

##<a id="nextsteps"></a>次のステップ

HDInsight での MapReduce ジョブに関する全般的な情報:

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)


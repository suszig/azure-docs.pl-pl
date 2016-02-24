<properties
   pageTitle="HDInsight での Hadoop Pig と Curl の使用 | Microsoft Azure"
   description="Curl を使用して Azure HDInsight の Hadoop クラスターで Pig Latin ジョブを実行する方法を説明します。"
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
   ms.date="11/06/2015"
   ms.author="larryfr"/>

#Curl を使用した HDInsight の Hadoop での Pig ジョブの実行

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

このドキュメントでは、Curl を使用して Azure HDInsight クラスターで Pig Latin ジョブを実行する方法を説明します。 Pig Latin プログラミング言語では、入力データに適用される変換を記述し、目的の出力を生成することができます。

Curl は、未加工の HTTP 要求を使用して HDInsight とやり取りし、Pig ジョブを実行、監視して、その結果を取得する方法を示すために使用します。 これは、HDInsight クラスターで提供される WebHCat REST API (旧称: Templeton) を使用することで機能します。

> [AZURE.NOTE] Linux ベースの Hadoop サーバーの使用に慣れているが HDInsight は、という場合は、次を参照してください。 [Linux ベース HDInsight の使用方法](hdinsight-hadoop-linux-information.md)します。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Azure HDInsight (HDInsight での Hadoop) クラスター (Linux または Windows ベース)

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Curl を使用した Pig ジョブの実行

> [AZURE.NOTE] Curl、またはその他の REST 通信で WebHCat を使用する場合は、HDInsight クラスターの管理者のユーザー名およびパスワードを指定して要求を認証する必要があります。 また、サーバーへの要求の送信に使用する Uniform Resource Identifier (URI) にクラスター名を含める必要があります。
>
> ここでのコマンド **ユーザー名** をクラスターへの認証し、置換するユーザー **パスワード** ユーザー アカウントのパスワードを使用します。 置換 **CLUSTERNAME** 、クラスターの名前に置き換えます。
>
> REST API のセキュリティは、[基本アクセス認証](http://en.wikipedia.org/wiki/Basic_access_authentication)を通じて保護されています。 資格情報をサーバーに安全に送信するには、必ずセキュア HTTP (HTTPS) を使用して要求を行う必要があります。

1. コマンド ラインで次のコマンドを使用して、HDInsight クラスターに接続できることを確認します。

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    次のような応答を受け取ります。

        {"status":"ok","version":"v1"}

    このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-u**: ユーザー名と要求の認証に使用されるパスワード
    * **-G**: GET 要求であることを示します

    URL の先頭 **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, 、すべての要求に対して同じになります。 パス、 **/status**, 、サーバーの要求は WebHCat (別名: Templeton) の状態を返すことを示します。

2. 次のコードを使用して、Pig Latin ジョブをクラスターに送信します。

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasb:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasb:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    このコマンドで使用されるパラメーターの意味は次のとおりです。

    * **-d**: ため `-G` は使用されず、要求は既定で POST メソッドです。 `-d` 要求と共に送信されるデータ値を指定します。

        * **user.name**: コマンドを実行しているユーザー
        * **実行**: 実行する Pig Latin ステートメント
        * **statusdir**: にジョブのステータスが書き込まれるディレクトリ

    > [AZURE.NOTE] Pig Latin ステートメントのスペースで置き換えられる通知、 `+` Curl を使用したとき。

    このコマンドは、ジョブのステータスの確認に使用できる ジョブ ID を返します。たとえば、

        {"id":"job_1415651640909_0026"}

3. ジョブのステータスを確認するには、次のコマンドを使用します。 置換 **JOBID** 前の手順で返される値を使用します。 たとえば、戻り値が `{"id":"job_1415651640909_0026"}`, 、し **JOBID** は `job_1415651640909_0026`です。

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    状態になる場合は、ジョブが完了したら、 **SUCCEEDED**します。

    > [AZURE.NOTE] この Curl 要求は、ジョブに関する情報を持つ JavaScript Object Notation (JSON) ドキュメントを返し、jq を使用して、状態値のみを取得します。

##<a id="results"></a>結果の表示

ジョブの状態が変更された時期 **SUCCEEDED**, 、Azure Blob ストレージからジョブの結果を取得できます。  `statusdir` クエリで渡されるパラメーターには、ここでは、出力ファイルの場所が含まれています。 **wasb:////例/pigcurl**します。 このアドレスではジョブの出力を格納する、 **例/pigcurl** ディレクトリに、HDInsight クラスターで使用される既定のストレージ コンテナー。

一覧表示しを使用してこれらのファイルをダウンロードすることができます、 [Mac、Linux および Windows 用 Azure CLI](../xplat-cli-install.md)します。 たとえば、ファイルを一覧表示 **例/pigcurl**, 、次のコマンドを使用します。

    azure storage blob list <container-name> example/pigcurl

ファイルをダウンロードするには、次のコマンドを使用します。

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] 使用して blob を格納するストレージ アカウント名を指定する必要があります、 `-a` と `-k` パラメーター、またはセット、 **azure \_storage\_account** と **azure \_storage\_access\_key** 環境変数です。

##<a id="summary"></a>概要

このドキュメントを参照すると、未加工の HTTP 要求を使用して HDInsight クラスターで Pig ジョブを実行および監視し、その結果を表示できます。

この記事で使用される REST インターフェイスの詳細については、次を参照してください。、 [WebHCat リファレンス](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)します。

##<a id="nextsteps"></a>次のステップ

HDInsight での Pig に関する全般的な情報:

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)


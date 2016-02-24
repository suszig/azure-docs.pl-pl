<properties
    pageTitle="HDInsight における Python と Hive および Pig の使用 | Microsoft Azure"
    description="HDInsight (Azure の Hadoop テクノロジ スタック) での Hive および Pig の Python ユーザー定義関数 (UDF) の使用方法について説明します。"
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
    ms.devlang="python"
    ms.topic="article"
    ms.date="12/04/2015" 
    ms.author="larryfr"/>

#HDInsight における Python と Hive および Pig の使用

Hive と Pig は HDInsight でデータを処理する場合にきわめて有益ですが、より汎用的な言語が必要になる場合もあります。 Hive と Pig では、さまざまなプログラミング言語を使用してユーザー定義関数 (UDF) を作成できます。 この記事では、Hive と Pig で Python UDF を使用する方法について説明します。

> [AZURE.NOTE] この記事で説明する手順は、HDInsight クラスター version 2.1、3.0、3.1 および 3.2 に適用します。


##<a name="python"></a>HDInsight の Python

Python2.7 は HDInsight 3.0 以降のクラスターに既定でインストール済みです。 このバージョンの Python と共に Hive を使用することで、ストリームを処理できます (Hive と Python の間のデータの受け渡しには STDOUT/STDIN を使用します)。

HDInsight には、Java で記述された Python 実装である Jython も付属しています。 Pig ではストリーミングを用いずに Jython と対話できるため、Pig を使用している場合に有効です。

###<a name="hivepython"></a>Hive と Python

Python は、HiveQL を通じて Hive から UDF として使用できる **変換** ステートメントです。 たとえば、次の HiveQL に格納されている Python スクリプトを呼び出す、 **streaming.py** ファイルです。

**Linux ベースの HDInsight**

    add file wasb:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**Windows ベースの HDInsight**

    add file wasb:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [AZURE.NOTE] Windows ベースの HDInsight クラスターで、 **USING** 句で python.exe への完全パスを指定する必要があります。 これは常に `D:\Python27\python.exe` です。

この例では以下のように処理されます。

1.  **ファイルを追加** ファイルの先頭にあるステートメントを追加、 **streaming.py** ファイル分散キャッシュに、クラスター内のすべてのノードからアクセスできるようにします。

2.   **SELECT TRANSFORM.使用して** ステートメントからのデータの選択、 **hivesampletable**, 、clientid、devicemake、および devicemodel をし、 **streaming.py** スクリプトです。

3.  **AS** 句から返されるフィールドの説明を **streaming.py**

<a name="streamingpy"></a>
ここでは、 **streaming.py** HiveQL の使用例で使用するファイルです。

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

ストリーミングを使用しているため、このスクリプトは以下の処理が必要です。

1. STDIN のデータの読み取り。 この例では、`sys.stdin.readline()` を使用してデータを読み取ります。

2. ここではテキスト データのみが必要であり、行末のインジケーターは不要のため、`string.strip(line, "\n ")` で末尾の改行文字が削除されます。

2. ストリームの処理中は、すべての値が 1 つの行に含まれ、値と値の間はタブ文字で区切られます。 それにより、`string.split(line, "\t")` を使用してタブごとに入力を分割し、フィールドのみを返すことができます。

3. 処理の完了時には、フィールド間がタブで区切られた単一の行として、STDOUT に出力が書き出される必要があります。 この処理は `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])` を使用して実現します。

4. これはすべて `while` ループ内で実行され、`line` が読み込まれなくなるまで繰り返されます。読み込まれなくなった時点で `break` がループを終了し、スクリプトは終了します。

これ以外にも、スクリプトは `devicemake` と `devicemodel` の入力値を連結し、連結後の値のハッシュを計算します。 Hive から呼び出された Python スクリプトは、ループ、入力がなくなるまでの読み込み、タブでの各入力行の区切り、処理、タブで区切った単一行への出力の書き出しを行います。単純ですがこれが基本的な動作の説明です。

参照してください [例を実行する](#running) の HDInsight クラスターでこのサンプルを実行する方法です。

###<a name="pigpython"></a>Pig と Python

Python スクリプトはを通じて Pig から UDF として使用できる、 **生成** ステートメントです。 たとえば、次のコード例に格納されている Python スクリプト、 **jython.py** ファイルです。

    Register 'wasb:///jython.py' using jython as myfuncs;
    LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

以下に、このスクリプトの動作の例を示します。

1. Python スクリプトを含むファイルを登録 (**jython.py**,、) を使用して **Jython**, 、として Pig に公開および **myfuncs**します。 Jython は、Java による Python 実装であり、Pig と同じ Java 仮想マシンで動作します。 この方法によって、Hive で使用するストリーミング手法ではなく、従来の関数呼び出しのように Python スクリプトを扱うことができます。

2. 次の行は、サンプル データ ファイルを読み込む **sample.log** に **ログ**します。 各レコードもこのログ ファイルには、一貫したスキーマが割り当てられていない、ので定義 (**行** この場合、) として、 **chararray**します。 Chararray は本質的には文字列です。

3. 3 番目の行に操作の結果を格納するすべての null 値を除去し **ログ**します。

4. 次に、反復処理することでレコードを **ログ** を使用して **生成** を呼び出す、 **create_structure** メソッドに含まれている、 **jython.py** としてスクリプトが読み込まれて **myfuncs**します。  **行** 現在のレコードを関数に渡すために使用します。

5. 出力が STDOUT にダンプする最後を使用して、 **ダンプ** コマンドです。 これは、操作の完了後に結果をすぐに示すためのもの実際のスクリプトで通常どおり **ストア** 新しいファイルにデータをします。

<a name="jythonpy"></a>
ここでは、 **jython.py** Pig の例で使用されるファイル。

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
      if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
      date, time, classname, level, detail = input.split(' ', 4)
      return date, time, classname, level, detail

覚えてだけ定義した、 **行** 入力を chararray として、入力の一貫したスキーマがないためでしょうか。 どのような **jython.py** は出力の一貫したスキーマにデータを変換します。 次のような処理です。

1.  **@OutputSchema** ステートメント Pig に返されるデータの形式を定義します。 この場合、これは、 **データ バッグ**, 、Pig のデータ型であります。 この bag には以下のフィールドが含まれ、すべて chararray (文字列) です。

    * date - ログ エントリが作成された日付
    * time - ログ エントリが作成された時刻
    * classname - エントリが作成されたクラスの名前
    * level - ログ レベル
    * detail - ログ エントリの詳細説明

2. 次に、 **def create_structure (input)** Pig が行項目を渡す関数を定義します。

3. サンプル データ **sample.log**, 、ほとんどの場合、日付、時刻、クラス名、レベル、および詳細説明を取得するスキーマです。 文字列で始まるいくつかの行も含まれていますが、'*java.lang.Exception*' がスキーマと一致するように変更する必要があります。  **場合** ステートメントは、それらを確認し、一致するように、入力データを移動する、'*java.lang.Exception*' 文字列を末尾に、データの行の予想される出力スキーマとします。

4. 次に、 **分割** コマンドを使用して、最初の 4 つの空白文字でデータを分割します。 これは、結果 5 つの値に割り当てられている **日付**, 、**時間**, 、**classname**, 、**レベル**, 、および **詳細**します。

5. 最後に、値が Pig に返されます。

データが Pig に返されると、それが一貫したスキーマで定義されている、 **@outputSchema** ステートメントです。

##<a name="running"></a>例を実行する

Linux ベースの HDInsight クラスターを使用している場合は使用して、 **SSH** 次の手順です。 Windows ベースの HDInsight クラスターと、Windows クライアントを使用している場合は使用して、 **PowerShell** 手順を実行します。

###SSH

SSH の使用に関する詳細については、次を参照してください。 <a href="../hdinsight-hadoop-linux-use-ssh-unix/" target="_blank">Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する</a> または <a href="../hdinsight-hadoop-linux-use-ssh-windows/" target="_blank">HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する</a>.

1. Python の例を使用して [streaming.py](#streamingpy) と [jython.py](#jythonpy), 、開発用コンピューター上のファイルのローカル コピーを作成します。

2. `scp` を使用して HDInsight クラスターにファイルをコピーします。 次がという名前のクラスターにファイルをコピーするなど、 **mycluster**します。

        scp streaming.py jython.py myuser@mycluster-ssh.azurehdinsight.net:

3. SSH を使用してクラスターに接続します。 次の例、という名前のクラスターに接続 **mycluster** ユーザーとして **myuser**します。

        ssh myuser@mycluster-ssh.azurehdinsight.net

4. 以前アップロードした python のファイルを SSH セッションからクラスターの WASB ストレージに追加します。

        hadoop fs -copyFromLocal streaming.py /streaming.py
        hadoop fs -copyFromLocal jython.py /jython.py

ファイルをアップロードした後、次の手順に従って、Hive ジョブと Pig ジョブを実行します。

####Hive

1. `hive` コマンドを使用して hive シェルを起動します。 シェルが読み込まれると、`hive>` プロンプトが表示されます。

2. `hive>` プロンプトで、以下を入力します。

        add file wasb:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

3. 最後の行を入力すると、ジョブが開始されます。 最終的に、次のような出力が返されます。

        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####Pig

1. `pig` コマンドを使用してシェルを起動します。 シェルが読み込まれると、`grunt>` プロンプトが表示されます。

2. `grunt>` プロンプトで、以下のステートメントを入力します。

        Register wasb:///jython.py using jython as myfuncs;
        LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

3. 次の行を入力すると、ジョブが開始されます。 最終的に、次のような出力が返されます。

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

###PowerShell

次の手順では、Azure PowerShell を使用します。 インストールされていないと、開発用コンピューターで構成されているが場合、は、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../install-configure-powershell.md) 、次の手順を使用する前にします。

1. Python の例を使用して [streaming.py](#streamingpy) と [jython.py](#jythonpy), 、開発用コンピューター上のファイルのローカル コピーを作成します。

2. 次の PowerShell スクリプトを使用して、アップロードする、 **streaming.py** と **jython.py** サーバーへのファイルです。 Azure HDInsight クラスター、およびへのパスの名前に置き換えます、 **streaming.py** と **jython.py** ファイル、スクリプトの最初の 3 つの行にします。

        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\jython.py"

        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
        
        Set-AzureStorageBlobContent `
            -File $pathToStreamingFile `
            -Blob "streaming.py" `
            -Container $container `
            -Context $context
        
        Set-AzureStorageBlobContent `
            -File $pathToJythonFile `
            -Blob "jython.py" `
            -Container $container `
            -Context $context

    このスクリプトは、HDInsight クラスターの情報を取得してから、既定のストレージ アカウント用にアカウントとキーを抽出します。その後、ファイルをコンテナーのルートにアップロードします。

    > [AZURE.NOTE] その他のスクリプトをアップロードする方法は記載されて、 [HDInsight での Hadoop ジョブ用データのアップロード](hdinsight-upload-data.md) ドキュメントです。

ファイルのアップロード後、次の PowerShell スクリプトを使用してジョブを開始します。 ジョブが完了すると、PowerShell コンソールに出力が書き込まれます。

####Hive

次のスクリプトの実行、 __streaming.py__ スクリプトです。 実行前に、HDInsight クラスターの HTTPs/Admin アカウント情報の入力が求められます。

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName
    $creds=Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        | %{ $_.Key1 }
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    $HiveQuery = "add file wasb:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'D:\Python27\python.exe streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
        -Query $HiveQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

出力を **Hive** ジョブは、次のように表示されます。

    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####Pig

次を使用して、 __jython.py__ スクリプトです。 実行前に、HDInsight クラスターの HTTPs/Admin 情報の入力が求められます。

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        | %{ $_.Key1 }
    
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    $PigQuery = "Register wasb:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
        
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -Job $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

出力を **Pig** ジョブは、次のように表示されます。

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

##<a name="troubleshooting"></a>トラブルシューティング

例の実行に使用した両方の PowerShell スクリプトには、ジョブのエラー出力を表示するコメント行が含まれています。 ジョブについて想定された出力が確認できない場合は、次の行をコメント解除し、エラー情報で問題が示されるかどうかを確認してください。

    # Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

エラー情報 (STDERR) およびジョブの結果 (STDOUT) は、クラスターの既定の BLOB コンテナーの以下の場所にも記録されます。

ジョブ|BLOB コンテナーで確認するファイル
---|---
Hive|/HivePython/stderr<p>/HivePython/stdout
Pig|/PigPython/stderr<p>/PigPython/stdout

##<a name="next"></a>次のステップ

既定では提供されない Python モジュールをロードする必要がある場合は、次を参照してください。 [モジュールを Azure HDInsight にデプロイする方法](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx) これを行う方法の例です。

Pig と Hive を使用する他の方法と、MapReduce の使用方法については、以下をご覧ください。

* [HDInsight での Hive の使用](hdinsight-use-hive.md)

* [HDInsight での Pig の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce の使用](hdinsight-use-mapreduce.md)


<properties
    pageTitle="Mahout と Windows ベースの HDInsight を使用したリコメンデーションの生成 | Microsoft Azure"
    description="Apache Mahout 機械学習ライブラリを使用して Windows ベースの HDInsight (Hadoop) で映画のリコメンデーションを生成する方法について説明します。"
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
    ms.date="11/03/2015"
    ms.author="larryfr"/>

#HDInsight で Apache Mahout と Hadoop を使用して映画のリコメンデーションを生成する

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

使用する方法について、 [Apache Mahout](http://mahout.apache.org) 映画のリコメンデーションを生成する Azure HDInsight での機械学習ライブラリです。

> [AZURE.NOTE] このドキュメントの手順では、Windows クライアントと Windows ベースの HDInsight クラスターが必要です。 Linux ベースの HDInsight クラスターで Mahout Linux、OS X または Unix クライアントからの使用方法の詳細については、次を参照してください [Linux ベースの hadoop を HDInsight で Apache Mahout を使用して映画のリコメンデーションを生成。](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>学習内容

Mahout は、 [機械学習][ml] ライブラリの 1 つです。 Mahout には、フィルター処理、分類、クラスタリングなどデータを処理するためのアルゴリズムが含まれています。 この記事では、リコメンデーション エンジンを使用し、友人たちが鑑賞した映画に基づいて映画のリコメンデーションを生成します。 また、デシジョン フォレストで分類を実行する方法についても説明します。 学習内容は以下のとおりです。

* Windows PowerShell を使用して Mahout ジョブを実行する方法

* Mahout ジョブを Hadoop コマンド ラインから実行する方法

* Mahout を HDInsight 3.0 と HDInsight 2.0 クラスターにインストールする方法

    > [AZURE.NOTE] Mahout は HDInsight 3.1 バージョンのクラスターで提供されます。 以前のバージョンの HDInsight を使用している場合は、次を参照してください。 [Mahout のインストール](#install) 続行する前にします。

##前提条件

- **HDInsight での Windows ベースの Hadoop クラスター**します。 1 つを作成する方法の詳細については、次を参照してください [HDInsight で Hadoop を使用します。。][getstarted]
- **Azure PowerShell を実行できるワークステーション**します。 参照してください [Azure PowerShell 1.0 をインストールし、大きい](hdinsight-administer-use-powershell.md#install-azure-powershell-10-and-greater)します。


##<a name="recommendations"></a>Windows PowerShell を使用してリコメンデーションを生成します。

> [AZURE.NOTE] 使用される、ジョブがこのセクションでは、Windows PowerShell を使用して、Mahout で提供されるクラスの多くは現在取り扱っていません Windows PowerShell、および Hadoop コマンドラインを使用して実行する必要があります。 Windows PowerShell で動作しないクラスの一覧は、次を参照してください。、 [トラブルシューティング](#troubleshooting) セクションです。
>
> Hadoop コマンドラインを使用して Mahout ジョブを実行するの例は、次を参照してください。 [Hadoop コマンドラインを使用してデータを分類する](#classify)です。

Mahout で提供される機能の 1 つが、リコメンデーション エンジンです。 データは、`userID`、`itemId`、`prefValue` (項目に対するユーザーの嗜好) の形式で受け付けられます。 Mahout に共起分析を実行できます: _項目の優先順位を持つユーザーがその他の項目もこれらの基本設定に必要が_です。 次に Mahout は、項目の嗜好が似ているユーザーを特定します。これはリコメンデーションの作成に使用できます。

映画を使用した非常にシンプルな例を次に示します。

* __共起__: Joe、Alice と Bob すべて好きな映画として _スター ウォーズ_, 、_The Empire Strikes Back_, 、および _、Jedi の返品_します。 Mahout では、これらの映画のいずれかを好きなユーザーが他の 2 作品も好きであると判断します。

* __共起__: Bob と Alice も好きな映画 _The Phantom Menace_, 、_クローンの攻撃_, 、および _、Sith の復讐_します。 Mahout では、この例の最初の 3 作品を好きなユーザーがこれらの 3 作品も好きであると判断します。

* __類似性のリコメンデーション__: Joe は、最初の 3 作品を好き、Mahout では映画嗜好が似て、その他、Joe がまだ観ていない (好み/順位)。 この場合、Mahout で _The Phantom Menace_, 、_クローンの攻撃_, 、および _、Sith の復讐_します。

###データを読み込む

便利なことに、 [GroupLens Research][movielens] Mahout と互換性のある形式で映画の評価データを提供します。

1. ダウンロード、 [MovieLens 100 k][100k] アーカイブで、1,700 本の映画の 1,000 人のユーザーによる評価 100,000 件が含まれています。

2. アーカイブを展開します。 含める必要があります、 __100 k__ ディレクトリが付いた多数のデータ ファイルが含まれて __u です。__します。 Mahout により分析されるファイルは __u.data__します。 このファイルのデータ構造は、`userID`、`movieID`、`userRating`、`timestamp` です。 次にデータの例を示します。


        196 242 3   881250949
        186 302 3   891717742
        22  377 1   878887116
        244 51  2   880606923
        166 346 1   886397596


3. アップロード、 __u.data__ ファイルを __example/data/u.data__ 、HDInsight クラスター。 次のコマンドでは、PowerShell を使用してデータをアップロードします。 ファイルをアップロードするには、他の方法を参照してください。 [HDInsight での Hadoop ジョブ用データをアップロード][upload]します。

        # Put your cluster name below
        $clusterName="Your HDInsight cluster name"
        # Put the path to the u.data file below
        $fileToUpload="The path to the u.data file"
        
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/data/u.data" `
            -Container $container `
            -Context $context
    
    これによりアップロード、 __u.data__ ファイルを __example/data/u.data__ クラスターの既定のストレージです。 使用して、このデータにアクセスすることができます、 __wasb:///example/data/u.data__ HDInsight ジョブの URI。

###ジョブを実行する

Mahout リコメンデーション エンジンを使用してジョブを実行する次の Windows PowerShell スクリプトを使用して、 __u.data__ 前にアップロードしたファイル。

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
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
            
    # NOTE: The version number portion of the file path
    # may change in future versions of HDInsight.
    $jarFile = "file:///C:/apps/dist/mahout-0.9.0.2.2.7.1-33/examples/target/mahout-examples-0.9.0.2.2.7.1-33-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasb:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasb:///example/data/u.data",
                    "--output", "wasb:///example/out",
                    "--tempDir", "wasb:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout ジョブでは、ジョブを処理中に作成される一時的なデータは削除しません。 このサンプル ジョブでは `--tempDir` パラメーターを指定し、一時ファイルを特定のディレクトリに分離します。

Mahout ジョブは出力を STDOUT に返しません。 代わりと指定された出力ディレクトリに格納 __- part-r-00000__します。 スクリプトをダウンロードするには、このファイル __output.txt__ 、ワークステーション上の現在のディレクトリにします。

ファイルの内容の例を次に示します。

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

最初の列は `userID` です。 "[" と "]" に含まれる値は `movieId`:`recommendationScore` です。

###出力を表示する

生成された出力はアプリケーションで使用できるものですが、人間が判読するのは困難です。 抽出されたその他のファイルの一部、 __100 k__ フォルダー前の手順は使用できます解決するのには `movieId` 次の PowerShell スクリプトでこれを映画名にします。

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "u.data"
            -movieFile "u.item"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

このスクリプトを使用する必要がありますが以前に展開した、 __100 k__ フォルダーです。 スクリプトを実行する例を次に示します。

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\ml-100k\u.data -movieFile .\ml-100k\u.item -recommendationFile .\output.txt

出力は次のようになります。

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Hadoop コマンドラインを使用してデータを分類します。

Mahout で利用可能は分類方法の 1 つは作成する、 [ランダム フォレスト][forest]します。 これは複数の手順から成るプロセスです。データの分類に使用されるデシジョン ツリーをトレーニング データで生成する手順も含まれます。 これを使用して、 __org.apache.mahout.classifier.df.tools.Describe__ Mahout により提供されるクラスです。 現在は Hadoop コマンド ラインを使用して実行する必要があります。

###データを読み込む

1. 次のファイルをダウンロード [The NSL-KDD Data Set](http://nsl.cs.unb.ca/NSL-KDD/)します。

  * [KDDTrain + です。ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): トレーニング ファイル

  * [KDDTest + です。ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): テスト データ

2. 各ファイルを開き、'@' で始まる先頭の数行を削除して、ファイルを保存します。 これらが削除されていない場合、このデータを Mahout で使用するとエラー メッセージが返されます。

2. ファイルをアップロード __例/データ__します。 この処理には、次のスクリプトを使用できます。 置換 __CLUSTERNAME__ HDInsight クラスターの名前に置き換えます。 FILENAME をアップロードするファイルの名前に置き換えます。

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
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
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###ジョブを実行する

1. このジョブでは、Hadoop コマンドラインが必要です。 「[RDP を使用した HDInsight クラスターへの接続](hdinsight-administer-use-management-portal.md#rdp)」の手順に従って、HDInsight クラスターのリモート デスクトップを有効にしてからデスクトップに接続します。

3. 接続した後を使用して、 __Hadoop コマンド ライン__ Hadoop コマンドラインを開くにはアイコン。

    ![hadoop cli][hadoopcli]

3. 次のコマンドを使用してファイル記述子を生成 (__KDDTrain + し__)、Mahout を使用します。

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasb:///example/data/KDDTrain+.arff" -f "wasb:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` は、ファイル内のデータの属性を示します。 たとえば、L はラベルを示します。

4. 次のコマンドを使用してデシジョン ツリーのフォレストをビルドします。

        hadoop jar c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasb:///example/data/KDDTrain+.arff -ds wasb:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    この操作の出力は、 __nsl フォレスト__ _ _ wasb://user/& lt、HDInsight クラスターの記憶域のあるディレクトリ; ユーザー名 >/nsl フォレスト nsl forest.seq します。 &lt;username> は、リモート デスクトップ セッションに使用されるユーザー名です。 これは、人間が判読できないファイルです。

5. 分類することにより、フォレストをテスト、 __KDDTest + .arff__ データセット。 次のコマンドを使用します。

        hadoop jar c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasb:///example/data/KDDTest+.arff -ds wasb:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasb:///example/data/predictions

    このコマンドは、次のような分類プロセスの概要情報を返します。

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  このジョブにあるファイルも生成されますが __wasb:///example/data/predictions/KDDTest+.arff.out__します。 これは、人間が判読できないファイルです。

> [AZURE.NOTE] Mahout ジョブでは、ファイルは上書きされません。 これらのジョブをもう一度実行する場合は、前のジョブで作成したファイルを削除する必要があります。

##<a name="troubleshooting"></a>トラブルシューティング

###<a name="install"></a>Mahout のインストール

Mahout は HDInsight 3.1 クラスターにインストールされますが、次の手順を使用して手動で HDInsight 3.0 または HDInsight 2.1 クラスターにインストールできます。

1. 使用する Mahout のバージョンは、使用するクラスターの HDInsight バージョンによって異なります。 Azure ポータルでクラスターのプロパティを表示することで、クラスターのバージョンを見つけることができます。

  * __HDInsight 2.1__, を含む Java アーカイブ (JAR) ファイルをダウンロードする [Mahout 0.9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)します。

  * __HDInsight 3.0__, 、する必要があります [Mahout をソースからビルド][build] HDInsight で提供される Hadoop バージョンを指定します。 ビルド ページに示された前提条件をインストールし、ソースをダウンロードして、次のコマンドを使用して Mahout jar ファイルを作成します。

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] When Mahout 1.0 is released, you should be able to use the prebuilt packages with HDInsight 3.0.

2. Jar ファイルをアップロード __/jar ファイルの例__ 、クラスターの既定のストレージです。 CLUSTERNAME を次のスクリプトで、HDInsight クラスターの名前に置き換えますへのパスとファイル名を置換、 __mahout-コース-0.9 のヘッド ノードにある__ ファイル.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
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
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###ファイルを上書きできない

Mahout ジョブは、処理中に作成された一時ファイルをクリーンアップしません。 さらに、このジョブは既存の出力ファイルを上書きしません。

Mahout ジョブの実行中のエラーを回避するには、実行と実行の間で一時ファイルと出力ファイルを削除するか、一時ディレクトリと出力ディレクトリに一意の名前を使用します。

###JAR ファイルが見つからない

HDInsight 3.1 クラスターには Mahout が含まれていますが、 パスとファイル名にはクラスターにインストールされている Mahout のバージョン番号が含まれています。 このチュートリアルの Windows PowerShell サンプル スクリプトでは、2015 年 11 月時点で有効なパスを使用していますが、バージョン番号は将来の HDInsight の更新で変わります。 使用しているクラスターの Mahout JAR ファイルへの現在のパスを決定するには、次の Windows PowerShell コマンドを使用して、返されるファイル パスを参照するようにスクリプトを変更します。

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasb:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Windows PowerShell で動作しないクラス

次のクラスを使用する Mahout ジョブを Windows PowerShell から使用すると、さまざまなエラー メッセージが返されます。

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

これらのクラスを使用するジョブを実行するには、HDInsight クラスターに接続し、Hadoop コマンド ラインを使用してジョブを実行します。 参照してください [Hadoop コマンドラインを使用してデータを分類する](#classify) 例については、です。

##次のステップ

ここまで、Mahout の使用方法を学習し、HDInsight でデータを操作するその他の方法を確認してきました。

* [HDInsight での Hive の使用](../hadoop-use-hive.md)
* [HDInsight での Pig の使用](../hadoop-use-pig.md)
* [HDInsight での MapReduce の使用](../hadoop-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: ../hdinsight-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 

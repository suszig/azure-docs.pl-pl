<properties
    pageTitle="HDInsight の Hadoop を使用した Twitter データの分析 | Microsoft Azure"
    description="HDInsight の Hadoop に格納されている Twitter データを Hive で分析し、特定の単語の使用頻度を調べる方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="jgao"/>

# HDInsight での Hive を使用した Twitter データの分析

ビッグ データの多くはソーシャル Website からもたらされます。 用意されているパブリック Api 
Twitter などのサイトで、データを分析して一般的な傾向を把握するのに便利です源です。 
このチュートリアルでは、Twitter streaming API を使用してツイートを取得し、Apache Hive を使用して 
[Azure HDInsight を使用して、特定の単語を含むツイートを多く送信した Twitter ユーザーの一覧を取得します。

> [AZURE.NOTE] このドキュメントの手順では、Windows ベースの HDInsight クラスターが必要です。 固有の手順について 
Linux ベースのクラスターに、次を参照してください。 [Twitter データの分析 HDInsight (Linux) での Hive を使用して](hdinsight-analyze-twitter-data-linux.md)します。



> [AZURE.TIP] 同様のサンプルでは、HDInsight のサンプル ギャラリーにします。 <a href="http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Analyze-Twitter-trend-using-Apache-Hive-in-HDInsight" target="_blank">HDInsight の Apache Hive を使用した Twitter の傾向の分析</a>に関する Channel 9 のビデオをご覧ください。

###前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **ワークステーション** Azure powershell をインストールして構成します。 参照してください [のインストールおよび使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)します。 Windows PowerShell スクリプトを実行するには、Azure PowerShell を管理者として実行し、実行ポリシーを設定する必要があります *RemoteSigned*します。 参照してください [Windows PowerShell スクリプトの実行][powershell-script]します。

    Windows PowerShell スクリプトを実行する前に、次のコマンドレットを使用して Azure サブスクリプションに接続されていることを確認します。

        Login-AzureRmAccount

    Azure サブスクリプションが複数ある場合は、次のコマンドレットを使用して、現在のサブスクリプションを設定します。

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>


- **Azure HDInsight クラスター**します。 クラスターのプロビジョニング方法の詳細については、次を参照してください。 [HDInsight の概要][hdinsight-get-started] または [HDInsight クラスターのプロビジョニング] [hdinsight-provision]します。 このチュートリアルでは、クラスター名は後で必要になります。

このチュートリアルで使用するファイルを次の表に示します。

ファイル|説明
---|---
/tutorials/twitter/data/tweets.txt|Hive ジョブのソース データです。
/tutorials/twitter/output|Hive ジョブの出力フォルダーです。 既定の Hive ジョブ出力ファイル名は **000000 _ 0**です。
tutorials/twitter/twitter.hql|HiveQL スクリプト ファイルです。
/tutorials/twitter/jobstatus|Hadoop ジョブの状態です。


##Twitter Feed の取得

このチュートリアルでは使用して、 [ストリーミング Api を Twitter][twitter-streaming-api]します。 特定の Twitter streaming API を使用するは [ステータス/フィルター][twitter-statuses-filter]します。

>[AZURE.NOTE] 10,000 のツイートを格納しているファイルと Hive スクリプト ファイルは (次のセクションで説明) は、パブリック Blob コンテナーにアップロードされています。 このセクションは、アップロードしたファイルを使用する場合は省略できます。

[ツイート データ](https://dev.twitter.com/docs/platform-objects/tweets) は複雑な入れ子になった構造体を含む JavaScript Object Notation (JSON) 形式で格納します。 従来のプログラミング言語を使用して多数のコード行を記述する代わりに、このネスト構造を Hive テーブルに変換し、構造化照会言語 (SQL) によく似た HiveQL という言語で照会するようにできます。

Twitter は OAuth を使用して、API への承認されたアクセスを提供します。 OAuth は、パスワードを共有せずに代理でアプリケーションが動作することをユーザーが承認できるようにする認証プロトコルです。 詳細については掲載されて [oauth.net](http://oauth.net/) または [OAuth 初心者向けガイド](http://hueniverse.com/oauth/) 確認できます。

OAuth を使用するための最初の手順は、Twitter 開発者サイトで新しいアプリケーションを作成することです。

**Twitter アプリケーションを作成するには**

1. サインイン [https://apps.twitter.com/](https://apps.twitter.com/)します。 クリックして、 **今すぐサインアップ** Twitter アカウントを持っていない場合は、リンクします。
2. クリックして **新しいアプリを作成する**です。
3. 入力 **名前**, 、**説明**, 、**web サイト**します。 URL を行うことができます、 **web サイト** フィールドです。 次のテーブルは使用する値のサンプルを示しています。

フィールド|値
---|---
名前|MyHDInsightApp
説明|MyHDInsightApp
Web サイト|http://www.myhdinsightapp.com

4. 確認 **Yes, I agree**, 、] をクリックし、 **Twitter アプリケーションを作成する**です。
5. クリックして、 **権限** ] タブをクリックします。 既定のアクセス許可は **読み取り専用**します。 このチュートリアルにはこれで十分です。
6. クリックして、 **Keys and Access Tokens** ] タブをクリックします。
7. クリックして **、アクセス トークンを作成**します。
8. クリックして **Test OAuth** ページの右上隅にします。
9. 書き留めて **コンシューマー キー**, 、**コンシューマー シークレット**, 、**アクセス トークン**, 、および **アクセス トークン シークレット**します。 これらの値は後で必要になります。

このチュートリアルでは、Windows PowerShell を使用して Web サービスを呼び出します。 .NET の c# サンプルでは、次を参照してください。 [HDInsight での HBase で Twitter センチメントのリアルタイム分析][hdinsight-hbase-twitter-sentiment]します。 Web サービス呼び出しを行うその他の一般的なツールは [*Curl*][curl]します。 Curl はからダウンロードできます [ここ][curl-download]します。

>[AZURE.NOTE] Windows で curl コマンドを使用する場合は、オプションの値の単一引用符の代わりに二重引用符を使用します。

**ツイートを取得するには**

1. Windows PowerShell Integrated Scripting Environment (ISE) を開きます  (Windows 8 のスタート画面で「 **PowerShell_ISE** ] をクリックし、 **Windows PowerShell ISE**します。 参照してください [Windows 8 での Windows PowerShell と Windows 起動][powershell-start].)

2. 次のスクリプトをスクリプト ウィンドウにコピーします。

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Add-AzureAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName |  %{ $_.Key1 }
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. スクリプトに、最初の 5 ～ 8 個の変数を設定します。


変数|説明
---|---
$clusterName|アプリケーションを実行する HDInsight クラスターの名前です。
$oauth_consumer_key|これは、Twitter アプリケーション **コンシューマー キー** 前に書き留めた Twitter アプリケーションを作成したとします。
$oauth_consumer_secret|これは、Twitter アプリケーション **コンシューマー シークレット** 前に書き留めたします。
$oauth_token|これは、Twitter アプリケーション **アクセス トークン** 前に書き留めたします。
$oauth_token_secret|これは、Twitter アプリケーション **アクセス トークン シークレット** 前に書き留めたします。
$destBlobName|出力 BLOB 名です。 既定値は **tutorials/twitter/data/tweets.txt**します。 既定値を変更する場合は、適宜 Windows PowerShell スクリプトを更新する必要があります。
$trackString|Web サービスはこれらのキーワードに関連するツイートを返します。 既定値は **Azure, Cloud, HDInsight**します。 既定値を変更する場合は、適宜 Windows PowerShell スクリプトを更新します。
$lineMax|この値によってスクリプトが読み取るツイートの数が決まります。 100 個のツイートを読み取るに約 3 分かかります。 大きな数値を設定してもかまいませんが、ダウンロードに時間がかかります。

5. キーを押して **f5 キーを押して** 、スクリプトを実行します。 この問題を回避するには、問題が発生する場合、すべての行を選択し、キーを押します **F8**します。
6. "Complete!"が表示されます。 出力の最後です。 エラー メッセージが赤色で表示されます。

出力ファイルをチェックする検証手順として **/tutorials/twitter/data/tweets.txt**, 、Azure ストレージ エクスプ ローラーまたは Azure PowerShell を使用して Azure Blob ストレージ上です。 一覧表示するファイルのサンプル Windows PowerShell スクリプトは、次を参照してください。 [Blob ストレージの使用 HDInsight で][hdinsight-storage-powershell]します。



##HiveQL スクリプトの作成

Azure PowerShell を使用して、複数の HiveQL ステートメントを一度に実行することも、HiveQL ステートメントをスクリプト ファイルにまとめることもできます。 このチュートリアルでは、HiveQL スクリプトを作成します。 スクリプト ファイルは、Azure Blob ストレージにアップロードする必要があります。 次のセクションでは、Azure PowerShell を使用してスクリプト ファイルを実行します。

>[AZURE.NOTE] Hive スクリプト ファイルと 10,000 のツイートを含むファイルをパブリック Blob コンテナーにアップロードするとします。 このセクションは、アップロードしたファイルを使用する場合は省略できます。

HiveQL スクリプトは、次の作業を実行します。

1. **Tweets_raw テーブルを削除** テーブルが既に存在する場合。
2. **Tweets_raw Hive テーブルを作成**します。 この一時的な Hive 構造テーブルには、さらに抽出、変換、および読み込み (ETL) 処理のデータが保持されます。 詳細については、パーティションは、次を参照してください。 [Hive のチュートリアルに関する][apache-hive-tutorial]します。  
3. **データを読み込む** /tutorials/twitter/data のソース フォルダからです。 JSON ネスト形式の大規模なツイート データセットが、一時的な Hive テーブル構造に変換されました。
3. **ツイート テーブルを削除** テーブルが既に存在する場合。
4. **ツイート テーブルを作成する**です。 Hive を使用してツイート データセットを照会するには、別の ETL 処理を実行する必要があります。 この ETL 処理は、"twitter_raw" テーブルに保存したデータのための詳細なテーブル スキーマを定義します。  
5. **上書きテーブルを挿入**します。 この複雑な Hive スクリプトは、Hadoop クラスターによる一連の長い MapReduce ジョブを開始します。 使用するデータセットおよびクラスターのサイズによっては、10 分ほどかかる場合があります。
6. **上書きディレクトリを挿入**します。 クエリを実行し、データセットをファイルに出力します。 このクエリは、"Azure" という単語が含まれるツイートを送信した Twitter ユーザーの一覧を返します。

**Hive スクリプトを作成して Azure にアップロードするには**

1. Windows PowerShell ISE を開きます。
2. 次のスクリプトをスクリプト ウィンドウにコピーします。

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName | %{$_.key1}
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. スクリプトの最初の 2 個の変数を設定します。

変数|説明
---|---
$clusterName|アプリケーションを実行する HDInsight クラスター名を入力します。
$subscriptionID|Azure サブスクリプション ID を入力します。
$sourceDataPath|Hive クエリがデータを読み取る Azure Blob ストレージの場所です。 この変数を変更する必要はありません。
$outputPath|Hive クエリが結果を出力する Azure Blob ストレージの場所です。 この変数を変更する必要はありません。
$hqlScriptFile|HiveQL スクリプト ファイルの場所とファイル名です。 この変数を変更する必要はありません。

5. キーを押して **f5 キーを押して** 、スクリプトを実行します。 この問題を回避するには、問題が発生する場合、すべての行を選択し、キーを押します **F8**します。
6. "Complete!"が表示されます。 出力の最後です。 エラー メッセージが赤色で表示されます。

出力ファイルをチェックする検証手順として **/tutorials/twitter/twitter.hql**, 、Azure ストレージ エクスプ ローラーまたは Azure PowerShell を使用して Azure Blob ストレージ上です。 一覧表示するファイルのサンプル Windows PowerShell スクリプトは、次を参照してください。 [Blob ストレージの使用 HDInsight で][hdinsight-storage-powershell]します。  


##Hive を使用して Twitter データを処理する

すべての準備作業が完了しました。 Hive スクリプトを呼び出して、結果を確認できます。

### Hive ジョブの送信

次の Windows PowerShell スクリプトを使用して Hive スクリプトを実行します。 最初の変数を設定する必要があります。

>[AZURE.NOTE] ツイートし、アップロードした HiveQL スクリプトを使用して、最後の 2 つのセクションに $hqlScriptFile を設定します。"/tutorials/twitter/twitter.hql"です。 パブリック BLOB にアップロードしたツイートと HiveQL スクリプトを使用するには、$hqlScriptFile を "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql" に設定します。

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName |  %{ $_.Key1 }
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -StandardError
    #endregion

### 結果を確認する

次の Windows PowerShell スクリプトを使用して Hive ジョブ出力を確認します。 最初の 2 つの変数を設定する必要があります。

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName |  %{ $_.Key1 }
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $storageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Hive テーブルでは \001 をフィールド区切り記号として使用します。 区切り記号は出力には表示されません。

分析結果が Azure BLOB ストレージに配置されると、Azure SQL Database/SQL Server へのデータのエクスポート、Power Query を使用してのデータの Excel へのエクスポート、または Hive ODBC ドライバーを使用してのアプリケーションのデータへの接続ができます。 詳細については、次を参照してください。 [HDInsight での Sqoop の使用][hdinsight-use-sqoop], 、[HDInsight を使用したフライト遅延データの分析][hdinsight-analyze-flight-delay-data], 、[Excel から Power Query を使用した HDInsight への接続][hdinsight-power-query], 、および [Excel から Microsoft Hive ODBC ドライバーを使用した HDInsight への接続][hdinsight-hive-odbc]します。

##次のステップ

このチュートリアルでは、Azure 上で HDInsight を使用し、Twitter から収集したデータを照会、探索、分析するため、構造化されていない JSON データセットを構造化された Hive テーブルへ変換する方法を学習しました。 詳細については、次を参照してください。

- [HDInsight の使用][hdinsight-get-started]
- [HDInsight 環境の HBase で Twitter のセンチメントをリアルタイム分析する][hdinsight-hbase-twitter-sentiment]
- [HDInsight を使用したフライトの遅延データの分析][hdinsight-analyze-flight-delay-data]
- [Power Query を使用した Excel から HDInsight への接続][hdinsight-power-query]
- [Microsoft Hive ODBC ドライバーを使用した Excel から HDInsight への接続][hdinsight-hive-odbc]
- [HDInsight での Sqoop の使用][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: ../install-configure-powershell.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md


<properties
    pageTitle="BLOB ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントに接続した後、Visual Studio の ASP.NET プロジェクトで Azure BLOB ストレージの使用を開始する方法について説明します。"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="tarcher"/>

# BLOB ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET)

## 概要

Visual Studio を使用して ASP.NET app で Azure ストレージ アカウントを参照または作成した後、Azure Blob ストレージを使用して開始する方法を説明 **[接続済みサービス** ] ダイアログ ボックス。 この記事では、BLOB コンテナーの作成のほか、BLOB のアップロード、一覧表示、ダウンロード、および削除などの一般的なタスクの実施方法について述べます。 サンプルは C\# で記述され、
使用して、 [.NET 用 Azure ストレージ クライアント ライブラリ](https://msdn.microsoft.com/library/azure/dn261237.aspx)します。

 - Azure Blob ストレージの使用に関する一般的な情報を参照してください。 [.NET から Blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md)します。
 - ASP.NET プロジェクトの詳細については、次を参照してください。 [ASP.NET](http://www.asp.net)します。


Azure BLOB ストレージは、大量の非構造化データを格納して HTTP または HTTPS を介して世界中のどこからでもアクセスできるようにするサービスです。 1 つの BLOB は任意のサイズにできます。 BLOB として扱えるのは、画像、オーディオ ファイル、ビデオ ファイル、生データ、およびドキュメント ファイルのようなデータです。

ファイルがフォルダーに格納されるのと同様に、ストレージ BLOB はコンテナーに格納されます。 ストレージ アカウントを作成した後、その内部に 1 つまたは複数のコンテナーを作成します。 たとえば、"Scrapbook" という名前のストレージに、写真を格納するための "images" という名前の BLOB コンテナーと、音声ファイルを格納するための "audio" という名前の BLOB コンテナーを作成できます。 コンテナーを作成すると、個々の BLOB ファイルをコンテナーにアップロードできるようになります。




## コードで BLOB コンテナーにアクセスする

ASP.NET プロジェクトでプログラムを使用して BLOB にアクセスするには、次の項目を追加する必要があります (存在していない場合)。

1. プログラムを使用して Azure Storage にアクセスするすべての C# ファイルの冒頭部分に、次の名前空間宣言コードを追加します。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;


2. 取得、 **CloudStorageAccount** をストレージ アカウント情報を表すオブジェクト。 次のコードを使用して、Azure サービス構成からストレージ接続文字列とストレージ アカウント情報を取得できます。

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    > [AZURE.NOTE] 次のセクションでは、すべてのコードの前に、前のコードを使用します。

3. 取得、 **CloudBlobClient** 、ストレージ アカウント内の既存のコンテナーを参照するオブジェクト。

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] ASP.NET 5 で Azure Storage への呼び出しを実行する Api の一部は非同期です。 参照してください [Async および Await を使用した非同期プログラミング](http://msdn.microsoft.com/library/hh191443.aspx) の詳細。


## コードで BLOB コンテナーを作成する

使用することも、 **CloudBlobClient** 、ストレージ アカウント内のコンテナーを作成するオブジェクト。 呼び出しを追加を行うには必要なは **CreateIfNotExistsAsync** 上記のコードの次の例に示すようにします。

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();

## コンテナーに BLOB をアップロードする

Azure BLOB ストレージでは、ブロック BLOB とページ BLOB がサポートされています。  ほとんどの場合は、ブロック BLOB を使用することをお勧めします。

ファイルをブロック blob にアップロードするには、コンテナーの参照を取得し、
それを使用してブロック blob の参照を取得します。 BLOB の参照を取得したら、
呼び出すことによってデータのストリーム、 **UploadFromStream** メソッドです。 この操作により、BLOB がまだ存在しない場合は作成され、
存在する場合は上書きされます。 次の例は、BLOB をコンテナーにアップロードする方法を示しています。この例では、既にコンテナーが作成されていることを前提としています。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code."

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## コンテナー内の BLOB を一覧表示する

コンテナー内の blob の一覧を表示する、 **ListBlobs** の blob やディレクトリを取得するメソッド
を取得します。 返される
返される **IListBlobItem**, にキャストする必要があります、 **CloudBlockBlob**,、
**CloudPageBlob**, 、または **CloudBlobDirectory** オブジェクトです。  種類がわからない場合は、
種類の確認を使うとどれにキャストすればよいかがわかります。  次のコードは、
コードは、
 **写真** コンテナーです。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code."

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

前の例に示すように、blob サービスには、コンテナー内のディレクトリの概念として
あります。 これは、BLOB をよりフォルダーに近い構造で整理できるように
するためです。 たとえば、'photos' というコンテナーに次のブロック blob が
名前付き **写真**します。

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

呼び出すと **ListBlobs** (前の例を参照) として、"photos"コンテナーに対して、コレクションが返されます
 **CloudBlobDirectory** と **CloudBlockBlob** オブジェクト
含まれています。 次の例は、結果として得られる出力を示します。

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


必要に応じて設定、 **UseFlatBlobListing** のパラメーター、 **ListBlobs** メソッド
**true**します。 これは、結果として返されるすべての blob としてが、 **CloudBlockBlob**, ディレクトリに関係なく、します。  次の例では、呼び出しを **ListBlobs**します。

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

次の例では結果を示しています。

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg



## BLOB をダウンロードする

Blob をダウンロードするには、使用、 **DownloadToStream** メソッドです。 次の
例では、 **DownloadToStream** blob を転送する方法
ローカル ファイルに保存できるストリーム オブジェクトに BLOB の内容を転送します。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code"

    // Retrieve a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

使用することも、 **DownloadToStream** メソッドをテキスト文字列としての blob の内容をダウンロードします。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code"

    // Retrieve a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## BLOB を削除する

Blob を削除するには、 **削除** メソッドです。

    // Get a CloudBlobContainer named 'container' as described in "Access blob containers in code"

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## BLOB をページで非同期に一覧表示する

多数の BLOB を一覧表示する場合や、1 回の一覧表示操作で返される結果の数を制御する場合には、BLOB の一覧を結果のページで表示できます。 下記の例は、大きな結果のセットを返すために待機している間に実行がブロックされないように、結果をページで非同期に返す方法を示しています。

この例は、blob をフラット、階層的な一覧についてを設定して実行することも、 **useFlatBlobListing** のパラメーター、 **ListBlobsSegmentedAsync** メソッドを **false**します。

始まる必要がありますので、サンプル メソッドは、非同期メソッドを呼び出し、 **async** キーワード、およびそれを返す必要があります、 **タスク** オブジェクトです。 に対して指定された await キーワード、 **ListBlobsSegmentedAsync** メソッドが一覧表示タスクが完了するまで、サンプル メソッドの実行を中断します。

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## 次のステップ

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


<properties
    pageTitle=".NET から Blob Storage を使用する方法 | Microsoft Azure"
    description="Azure Blob Storage の概要、コンテナーの作成方法、BLOB コンテンツのアップロード、ダウンロード、一覧表示、および削除を行う方法について説明します。"
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article" 
    ms.date="12/01/2015"
    ms.author="tamram"/>


# .NET から Blob Storage を使用する方法

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概要

このガイドでは、Azure BLOB ストレージ サービスを使用して一般的なシナリオを実行する方法の
デモンストレーションを行います。 サンプルは C\# で記述され、
Azure .NET 用ストレージ クライアント ライブラリを利用しています。 ストレージ クライアント ライブラリは、Blob Storage REST API の操作を簡略化する SDK です。 このガイドで紹介するシナリオ **アップロード**, 、**を一覧表示する**, 、**ダウンロード**, 、および **を削除する** blob および完了するまで 1 時間はかかります。 作業開始のビデオを参照してくださいを視聴したい場合 [5 分以内に Azure ストレージへの概要](https://azure.microsoft.com/documentation/videos/azure-storage-5-minute-overview/) 読み取ることができます [5 分以内に Azure ストレージの概要](storage-getting-started-guide.md)します。

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-configure-connection-string-include](../../includes/storage-configure-connection-string-include.md)]

## プログラムで Blob Storage にアクセスする

[AZURE.INCLUDE [storage-dotnet-obtain-assembly](../../includes/storage-dotnet-obtain-assembly.md)]

### 名前空間宣言
プログラムを使用して Azure Storage にアクセスするすべての C\# ファイルの
冒頭部分に、次の名前空間宣言を追加します。

    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;

必ず `Microsoft.WindowsAzure.Storage.dll` アセンブリを参照してください。

[AZURE.INCLUDE [storage-dotnet-retrieve-conn-string](../../includes/storage-dotnet-retrieve-conn-string.md)]

A **CloudBlobClient** 型でオブジェクトを取得することを表します
BLOB を表すオブジェクトを取得できます。 この
次のコードを作成、 **CloudBlobClient** オブジェクトのストレージを使用します。
作成します。

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

## コンテナーを作成する

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

この例は、コンテナーがない場合に、コンテナーを作成する方法を示しています。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

既定では、新しいコンテナーはプライベートであるため、このコンテナーから
BLOB をダウンロードするにはストレージ アクセス キーを指定する
できます。 コンテナー内のファイルをだれでも利用できるようにする場合は、
次のコードを使ってコンテナーをパブリックに
設定できます。

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess =
        BlobContainerPublicAccessType.Blob });

パブリック コンテナー内の BLOB は、インターネットに接続しているすべてのユーザーが
表示できますが、変更または削除できるのは、適切なアクセス キーを持っているユーザーだけです。

## コンテナーに BLOB をアップロードする

Azure Blob Storage では、ブロック BLOB とページ BLOB がサポートされています。  ほとんどの場合は、ブロック BLOB を使用することをお勧めします。

ファイルをブロック blob にアップロードするには、コンテナーの参照を取得し、
それを使用してブロック blob の参照を取得します。 BLOB の参照を取得したら、
呼び出すことによってデータのストリーム、 **UploadFromStream** メソッドです。 この操作により、BLOB がまだ存在しない場合は作成され、
存在する場合は上書きされます。 

次の例は、BLOB をコンテナーにアップロードする方法を示しています。この例では、既にコンテナーが作成されていることを前提としています。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## コンテナー内の BLOB を一覧表示する

コンテナー内の BLOB を一覧表示するには、まず、コンテナーの参照を取得します。 クラスターの
コンテナーを使用してできます **ListBlobs** の blob やディレクトリを取得するメソッド
を取得します。 返される
返される **IListBlobItem**, にキャストする必要があります、 **CloudBlockBlob**,、
**CloudPageBlob**, 、または **CloudBlobDirectory** オブジェクトです。  種類がわからない場合は、
種類の確認を使うとどれにキャストすればよいかがわかります。  次のコードは、
コードは、
 `photos` コンテナー。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

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

上記のように、BLOB の名前はパス情報を含めて指定することができます。 これで、従来のファイル システムと同じように、整理およびスキャン可能な仮想ディレクトリ構造が作成されます。 ディレクトリ構造は仮想のみであり、Blob Storage で使用できるリソースはコンテナーと BLOB のみであることに注意してください。 ただし、ストレージ クライアント ライブラリが用意されています、 **CloudBlobDirectory** 仮想ディレクトリを参照し、この方法で整理される blob の操作プロセスを簡略化するオブジェクト。

たとえば、`photos` という名前のコンテナーに次の一連のブロック BLOB があったとします。

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

呼び出すと **ListBlobs** 、'photos' コンテナー (例を参照)、階層化された一覧が返されます。 両方が含まれる **CloudBlobDirectory** と **CloudBlockBlob** ディレクトリと、コンテナー内の blob をそれぞれを表すオブジェクト。 結果の出力は次のようになります。

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


必要に応じて設定、 **UseFlatBlobListing** のパラメーター、 **ListBlobs** メソッド
**true**します。 この場合、コンテナー内のすべての blob として返されます、 **CloudBlockBlob** オブジェクトです。 呼び出し **ListBlobs** を次のようなフラットな一覧を返します。

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

結果は次のようになります。

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## BLOB をダウンロードする

Blob をダウンロードするには、まず blob の参照を取得し、物書き、 **DownloadToStream** メソッドです。 次の
例では、 **DownloadToStream** blob を転送する方法
ローカル ファイルに保存できるストリーム オブジェクトに BLOB の内容を転送します。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

使用することも、 **DownloadToStream** メソッドをテキスト文字列としての blob の内容をダウンロードします。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## BLOB を削除する

BLOB を削除するには、まず BLOB の参照を取得し、次にその
**削除** メソッドをします。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## BLOB をページで非同期に一覧表示する

多数の BLOB を一覧表示する場合や、1 回の一覧表示操作で返される結果の数を制御する場合には、BLOB の一覧を結果のページで表示できます。 この例は、大きな結果のセットを返すために待機している間に実行がブロックされないように、結果をページで非同期に返す方法を示しています。

この例は、blob をフラット、階層的な一覧についてを設定して実行することも、 `useFlatBlobListing` のパラメーター、 **ListBlobsSegmentedAsync** メソッドを `false`します。

始まる必要がありますので、サンプル メソッドは、非同期メソッドを呼び出し、 `async` キーワード、およびそれを返す必要があります、 **タスク** オブジェクトです。 に対して指定された await キーワード、 **ListBlobsSegmentedAsync** メソッドが一覧表示タスクが完了するまで、サンプル メソッドの実行を中断します。

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

## 追加 BLOB への書き込み

追加 BLOB は .NET 向け Azure ストレージ クライアント ライブラリのバージョン 5 x で発表された新しいタイプの BLOB です。 追加 BLOB は、ログ記録などの追加操作のために最適化されています。 ブロック BLOB のように、追加 BLOB はブロックで構成されますが、追加 BLOB に新しいブロックを追加する場合は常に BLOB の最後に追加されます。 追加 BLOB の既存のブロックは更新したり、削除することはできません。 追加 BLOB のブロック ID はブロック BLOB 用のため、公開されることはありません。 
 
追加 BLOB 内の各ブロックは、最大 4 MB のサイズにすることができます。また追加 BLOB には最大 50,000 のブロックを含めることができます。 よって追加 BLOB の最大サイズは 195 GB (4 MB X 50,000 ブロック) よりも少し大きくなります。

次の例では、新しい追加 BLOB を作成し、データを追加してシンプルなログ記録操作をシミュレートしています。

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist. 
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);
        
    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

参照してください [ブロック Blob、ページ Blob、および追加 Blob](https://msdn.microsoft.com/library/azure/ee691964.aspx) blob の 3 つの種類の違いの詳細についてです。

## 次のステップ

これを Blob ストレージの基本を学習できました。、次のリンクします。
詳細についてです。

### Blob Storage リファレンス ドキュメント

- [.NET 用ストレージ クライアント ライブラリ リファレンス](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API リファレンス](http://msdn.microsoft.com/library/azure/dd179355)

### その他の機能ガイド

- [.NET 用の Table Storage の概要](storage-dotnet-how-to-use-tables.md)
- [.NET 用の Queue Storage の概要](storage-dotnet-how-to-use-queues.md)
- [.NET 用の File Storage の概要](storage-dotnet-how-to-use-files.md)
- [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)
- [SQL Database を使用したリレーショナル データの格納](../sql-database/articles/sql-database-dotnet-how-to-use.md)
- [Web ジョブ SDK で Azure Blob Storage を使用する方法](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
 



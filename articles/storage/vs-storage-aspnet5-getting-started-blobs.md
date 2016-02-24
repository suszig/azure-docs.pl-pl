<properties
    pageTitle="BLOB ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET 5) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントを作成した後、Visual Studio の ASP.NET 5 プロジェクトで Azure BLOB ストレージの使用を開始する方法について説明します。"
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

# Azure BLOB ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET 5)

##概要

この記事では、Visual Studio の [接続済みサービスの追加] ダイアログを使用して ASP.NET 5 プロジェクトで Azure ストレージ アカウントを作成または参照した後、Visual Studio で Azure BLOB ストレージの使用を開始する方法について説明します。

Azure BLOB ストレージは、大量の非構造化データを格納して HTTP または HTTPS を介して世界中のどこからでもアクセスできるようにするサービスです。 1 つの BLOB は任意のサイズにできます。 BLOB として扱えるのは、画像、オーディオ ファイル、ビデオ ファイル、生データ、およびドキュメント ファイルのようなデータです。 この記事は、Visual Studio を使用して Azure ストレージ アカウントを作成した後、blob ストレージに着手する方法を説明 **[接続済みサービス** ASP.NET 5 プロジェクトでのダイアログ。

ファイルがフォルダーに格納されるのと同様に、ストレージ BLOB はコンテナーに格納されます。 ストレージを作成した後、その内部に 1 つまたは複数のコンテナーを作成します。 たとえば、"Scrapbook" という名前のストレージに、写真を格納するための "images" という名前のコンテナーと、音声ファイルを格納するための "audio" という名前のコンテナーを作成できます。 コンテナーを作成すると、個々の BLOB ファイルをコンテナーにアップロードできるようになります。 参照してください [.NET から blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md ".NET から blob ストレージを使用する方法") プログラムによる blob の操作の詳細についてです。



##コードで BLOB コンテナーにアクセスする

ASP.NET 5 プロジェクトでプログラムを使用して BLOB にアクセスするには、次の項目を追加する必要があります (存在していない場合)。

1. プログラムを使用して Azure Storage にアクセスするすべての C# ファイルの冒頭部分に、次の名前空間宣言コードを追加します。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. 取得、 **CloudStorageAccount** をストレージ アカウント情報を表すオブジェクト。 次のコードを使用して、Azure サービス構成からストレージ接続文字列とストレージ アカウント情報を取得できます。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **注:** で次のセクションでは、すべてのコードの前に上記のコードを使用します。


3. 使用して、 **CloudBlobClient** 取得するオブジェクト、 **CloudBlobContainer** 、ストレージ アカウント内の既存のコンテナーへの参照。

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##コードでコンテナーを作成する

使用することも、 **CloudBlobClient** 、ストレージ アカウント内のコンテナーを作成します。 呼び出しを追加することを行うだけで済みます **CreateIfNotExistsAsync** 次のコードのようにします。

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**注:** ASP.NET 5 で Azure ストレージへの呼び出しを実行する Api は非同期です。 参照してください [Async および Await における非同期プログラミング](http://msdn.microsoft.com/library/hh191443.aspx) の詳細。 次のコードでは、非同期のプログラミング方法を使用していることを前提としています。

コンテナー内のファイルをだれでも利用できるようにするには、次のコードを使ってコンテナーをパブリックに設定できます。

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##コンテナーに BLOB をアップロードする

BLOB ファイルをコンテナーにアップロードするには、コンテナーの参照を取得し、それを使用して BLOB の参照を取得します。 Blob の参照がある場合は後、のデータの任意のストリームをアップロードを呼び出して、 **UploadFromStreamAsync** メソッドです。 この処理により、BLOB が存在しない場合は作成され、存在する場合は上書きされます。 次の例は、BLOB をコンテナーにアップロードする方法を示しています。この例では、既にコンテナーが作成されていることを前提としています。

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##コンテナー内の BLOB を一覧表示する
コンテナー内の BLOB を一覧表示するには、まず、コンテナーの参照を取得します。 コンテナーを呼び出すことができますし、 **ListBlobsSegmentedAsync** blob やディレクトリを取得します。 プロパティとメソッドが返されたの豊富なセットにアクセスする **IListBlobItem**, にキャストする必要があります、 **CloudBlockBlob**, 、**CloudPageBlob**, 、または **CloudBlobDirectory** オブジェクトです。 BLOB の種類がわからない場合は、種類の確認を使うとどれにキャストすればよいかがわかります。 次のコードは、コンテナー内の各項目の URI を取得して出力する方法を示しています。

    BlobContinuationToken token = null;
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
            token = resultSegment.ContinuationToken;

            foreach (IListBlobItem item in resultSegment.Results)
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
        } while (token != null);

BLOB コンテナーの内容を一覧表示する方法は他にもあります。 参照してください [.NET から blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) の詳細。

##BLOB をダウンロードする
Blob をダウンロードするにまず、blob への参照を取得し、まず、 **DownloadToStreamAsync** メソッドです。 次の例では、 **DownloadToStreamAsync** 、ローカル ファイルに保存できるストリーム オブジェクトに blob の内容を転送する方法です。

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

BLOB をファイルとして保存する方法は他にもあります。 参照してください [.NET から blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md/#download-blobs) の詳細。

##BLOB を削除する
Blob を削除するには、まず、blob への参照を取得し、物書き、 **DeleteAsync** メソッドをします。

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## 次のステップ

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


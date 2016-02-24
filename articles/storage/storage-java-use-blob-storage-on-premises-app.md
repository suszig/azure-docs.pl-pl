<properties
    pageTitle="BLOB ストレージを使用する内部設置型アプリケーション (Java) | Microsoft Azure"
    description="画像を Azure にアップロードしてブラウザーに表示するコンソール アプリケーションを作成する方法について説明します。 コード サンプルは Java で記述されています。"
    services="storage"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="12/01/2015" 
    ms.author="robmcm"/>

# BLOB ストレージを使用するオンプレミスのアプリケーション

## 概要

次の例では、Azure Storage を使用して画像を Azure に保存する
方法を示しています。 この記事のコードは、コンソールには
ブラウザーに画像を表示する HTML ファイルを作成するコンソール アプリケーションの
コードを紹介します。

## 前提条件

- Java Developer Kit (JDK) バージョン 1.6 以降がインストールされていること。
- Azure SDK がインストールされていること。
- Azure libraries for Java、および適用されるあらゆる JAR
    依存関係 Jar がインストールされで使用されるビルド パスには
    Java コンパイラです。 Azure Libraries for Java のインストールについては、次を参照してください [をダウンロードします。
   Azure SDK for Java][]します。
- Azure ストレージ アカウントがセットアップされていること。 アカウント名
    ストレージ アカウントのアカウント キーは、コードによって使用されることは
    参照してください。 参照してください [How to Create a Storage Account] 、ストレージ アカウントを作成する方法について
     [ストレージ アカウントの管理方法][] 取得については、
    アカウント キー。
- ローカル画像ファイルを作成したパスに格納されています。
    c:\\myimages\\image1.jpg します。 代わりに、変更、
    **FileInputStream** 別に使用する例ではコンス トラクター
    イメージのパスとファイルの名前です。

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Azure BLOB ストレージを使用してファイルをアップロードするには

ここでは、手順を追って説明します。 場合は、スキップするには
この記事の後半で示すコード全体をします。

コードの先頭には、Azure コア ストレージ クラス、Azure BLOB クライアント クラス、
Java IO クラス、および
 **URISyntaxException** クラスです。

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

という名前のクラスを宣言 **StorageSample**, 、始めかっこを含める
**{**.

    public class StorageSample {

内で、 **StorageSample** クラスでは、文字列変数を宣言
指定されている既定のエンドポイント プロトコル、ストレージ アカウント名、
およびストレージ アクセス キーを格納する文字列変数を
宣言します。 プレース ホルダーの値 **your \_account\_name** と
**\_account\_key** を独自のアカウント名とアカウント キー
置き換えてください。

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

宣言を追加 **メイン**, が含まれて、 **と** ブロック、および
必要な左ブラケット **{**します。

    public static void main(String[] args)
    {
        try
        {

次の型の変数を宣言します (説明は、この例での使用に
関するものです)。

-   **CloudStorageAccount**: してアカウント オブジェクトを初期化するために使用
    Azure ストレージ アカウント名とキーを作成して、
    blob クライアント オブジェクトです。
-   **CloudBlobClient**: blob サービスにアクセスするために使用します。
-   **CloudBlobContainer**: blob コンテナーを作成するために、一覧表示します
    コンテナー、および削除、コンテナー内の blob です。
-   **CloudBlockBlob**: ローカルの画像ファイルをアップロードするために使用します
    できます。

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

値を割り当てる、 **アカウント** 変数です。

    account = CloudStorageAccount.parse(storageConnectionString);

値を割り当てる、 **serviceClient** 変数です。

    serviceClient = account.createCloudBlobClient();

値を割り当てる、 **コンテナー** 変数です。 参照はやはり後、
という名前のコンテナー **gettingstarted**します。

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

コンテナーを作成します。 このメソッドは、インストールされていない場合、コンテナーに作成されます。
存在 (返す **true**)。 コンテナーが存在する場合は、
**false**します。 代わりに **createIfNotExists** は、 **の作成**
メソッド (これは、コンテナーが既に存在する場合はエラーを返します)。

    container.createIfNotExists();

コンテナーへの匿名アクセスを設定します。

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

ブロック blob への参照を取得します。これは、Azure Storage 内の BLOB を
表します。

    blob = container.getBlockBlobReference("image1.jpg");

使用して、 **ファイル** ローカルで作成済みへの参照を取得するコンス トラクター
アップロードするファイルです。 前にこのファイルを作成することを確認します。
コードを実行します。

    File fileReference = new File ("c:\\myimages\\image1.jpg");

呼び出しを通じてローカル ファイルをアップロード、 **CloudBlockBlob.upload**
メソッドを呼び出します。 最初のパラメーター、 **CloudBlockBlob.upload** メソッドは、
**FileInputStream** となるローカル ファイルを表すオブジェクト
Azure storage にアップロードします。 2 番目のパラメーターは、サイズ、します。
ファイルのバイト数。

    blob.upload(new FileInputStream(fileReference), fileReference.length());

という名前のヘルパー関数を呼び出して **MakeHTMLPage**, 、基本的な HTML ページを作成するには
含む、 **& lt; イメージ & gt;** ソースを持つ要素が、blob に設定します。
Azure ストレージ アカウントにします。 コード
**MakeHTMLPage** この記事の後半で説明します。

    MakeHTMLPage(container);

ステータス メッセージと、作成された HTML ページに関する情報を出力します。

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

閉じる、 **と** 閉じかっこを挿入することによってブロック: **}**

次の例外を処理します。

-   **FileNotFoundException**: によってスローされることができます、 **FileInputStream**
    または **FileOutputStream** コンス トラクターです。
-   **StorageException**: Azure クライアントによってスローされることができます
    記憶域ライブラリです。
-   **URISyntaxException**: によってスローされることができます、 **ListBlobItem.getUri**
    メソッドを呼び出します。
-   **例外**: 汎用的な例外処理します。

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

閉じる **メイン** 閉じかっこを挿入して: **}**

という名前のメソッドを作成する **MakeHTMLPage** 基本的な HTML ページを作成します。 この
メソッドは、型のパラメーターを持つ **CloudBlobContainer**, 、となります
アップロードされた blob のリストを反復処理するために使用します。 このメソッドは
型の例外をスロー **FileNotFoundException**, 、スローされる可能性の
によって、 **FileOutputStream** コンス トラクター、および **URISyntaxException**,、
によってスローされる可能性のある、 **ListBlobItem.getUri** メソッドです。 は、
ブラケット **{**します。

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

という名前のローカル ファイルを作成する **index.html**します。

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

追加、ローカル ファイルへの書き込み、 **& lt; html & gt;**, 、**& lt; ヘッダー & gt;**, と
**& lt; 本文 & gt;** 要素です。

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

アップロードされた BLOB のリストに対して反復処理を行います。 HTML の各 blob について
ページを作成する、 **& lt; img & gt;** を持つ要素の **src** に送信される属性
blob の URI は、Azure ストレージ アカウントに存在します。
詳細は、追加した場合、このサンプルでは 1 つのみのイメージが追加されます。
このコードではそれらのすべてを反復処理します。

この例では、わかりやすくするために、アップロードされた各 BLOB が画像であると想定しています。 もし
画像、以外の blob やブロックではなくページ blob をアップロードしました。
blob は、必要に応じて、コードを調整します。

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

閉じる、 **& lt; 本文 & gt;** 要素および **& lt; html & gt;** 要素。

    stream.println("</body>");
    stream.println("</html>");

ローカル ファイルを閉じます。

    stream.close();

閉じる **MakeHTMLPage** 閉じかっこを挿入して: **}**

閉じる **StorageSample** 閉じかっこを挿入して: **}**

この例の完全なコードを次に示します。 変更に注意してください。
プレース ホルダー値 **your \_account\_name** と
**\_account\_key** アカウント名とアカウント キーを使用するには
置き換えてください。

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Azure ストレージにローカルの画像ファイルをアップロードするだけでなく
サンプル コードは、可能なローカル ファイル namedindex.html を作成します。
アップロードされた画像を表示するブラウザーで開きます。

コードには、アカウント名とアカウント キーが含まれている、ためにいることを確認します。
ソース コードは、セキュリティで保護されたです。

## コンテナーを削除するには

ストレージには課金されるため、この例を試した後、
**gettingstarted** コンテナーが完了したら実験の
の例を参照してください。 コンテナーを削除するには、 **CloudBlobContainer.delete**
メソッドを呼び出します。

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

呼び出すため、 **CloudBlobContainer.delete** メソッドは、プロセス
初期化 **CloudStorageAccount**, 、**ClodBlobClient**, 、および **CloudBlobContainer** オブジェクトを示すように同じです、
**createIfNotExist** メソッドです。 完全な例を次にします。
という名前のコンテナーを削除 **gettingstarted**します。

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

その他の blob ストレージ クラスとメソッドの概要については、次を参照してください。 [How to
Java から blob ストレージ サービスを使用して]します。

## 次のステップ

さらに複雑なストレージ タスクの詳細については、次のリンク先をご覧ください。

- [Azure Storage SDK for Java][]
- [Azure ストレージ クライアント SDK リファレンス][]
- [Azure Storage REST API][]
- [Azure のストレージ チーム ブログ][]

  [Download the Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
  [How to Create a Storage Account]: storage-create-storage-account.md#create-a-storage-account
  [How to Manage Storage Accounts]: storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys
  [How to Use the Blob Storage Service from Java]: storage-java-how-to-use-blob-storage.md
  [Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
  [Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/


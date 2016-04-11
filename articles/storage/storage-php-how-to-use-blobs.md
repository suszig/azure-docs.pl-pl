<properties
    pageTitle="PHP から BLOB ストレージを使用する方法 | Microsoft Azure"
    description="Azure BLOB サービスを使用して、BLOB をアップロード、列挙、ダウンロード、削除する方法について説明します。 コード サンプルは PHP で記述されています。"
    documentationCenter="php"
    services="storage"
    authors="tfitzmac"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="tomfitz"/>

# PHP から BLOB ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概要

このガイドでは、Azure BLOB サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは PHP を使用で記述され、 [Azure SDK for PHP] [download]します。 紹介するシナリオ **アップロード**, 、**を一覧表示する**, 、**ダウンロード**, 、および **を削除する** blob です。 Blob の詳細については、次を参照してください。、 [次のステップ](#next-steps) セクションです。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## PHP アプリケーションの作成

Microsoft Azure BLOB サービスにアクセスする PHP アプリケーションを作成するための要件は、コード内から Microsoft Azure SDK for PHP のクラスを参照することのみです。 アプリケーションの作成には、メモ帳などの任意の開発ツールを使用できます。

このガイドで使用するサービス機能は、PHP アプリケーション内でローカルで呼び出すことも、Azure の Web ロール、worker ロール、または Web サイト上で実行されるコード内で呼び出すこともできます。

## Azure クライアント ライブラリの入手

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## BLOB サービスにアクセスするようにアプリケーションを構成する

Azure BLOB サービス API を使用するには、次の要件があります。

1. 使用してオートローダー ファイルを参照、 [require_once][require_once] ステートメントと
2. 使用する可能性のあるクラスを参照する

次の例では、オートローダー ファイルと参照を含める方法、 **ServicesBuilder** クラスです。

> [AZURE.NOTE] この例 (および、この記事では、その他の例) は、Composer を使用して Azure 向け PHP クライアント ライブラリをインストールしたと仮定します。 ライブラリを手動でまたは PEAR パッケージとしてインストールした場合は、`WindowsAzure.php` オートローダー ファイルを参照する必要があります。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


下のすべてのサンプルに `require_once` ステートメントが入っていますが、サンプルの実行に必要なクラスのみが参照されます。

## Azure のストレージ接続文字列の設定

Azure BLOB サービス クライアントをインスタンス化するには、まず有効な接続文字列が必要です。 BLOB サービスの接続文字列の形式は次のとおりです。

ライブ サービスにアクセスする場合:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

ストレージ エミュレーターにアクセスする場合:

    UseDevelopmentStorage=true


いずれの Azure サービス クライアントを作成するには、使用する必要があります、 **ServicesBuilder** クラスです。 そのための方法は次のとおりです。

* 接続文字列を直接渡す
* 使用して、 **CloudConfigurationManager (CCM)** を接続文字列の複数の外部ソースを確認します。
    * 既定では 1 つの外部ソース (環境変数) のみサポートされています。
    * 新しいソースを追加するには拡張することによって、 **ConnectionStringSource** クラスです。

ここで概説している例では、接続文字列を直接渡します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## コンテナーを作成する

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** オブジェクトでは、使用して blob コンテナーを作成することができます、 **createContainer** メソッドです。 コンテナーの作成時にコンテナーのオプションを設定できますが、この設定は必須ではありません (次の例では、コンテナーのアクセス コントロール リスト (ACL) とメタデータを設定する方法を示しています)。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Blob\Models\CreateContainerOptions;
    use WindowsAzure\Blob\Models\PublicAccessType;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

呼び出す **\_and\_blobs** 、コンテナーと blob データを匿名の要求でアクセスできるようにします。 呼び出す **setPublicAccess(PublicAccessType::BLOBS_ONLY)** により、匿名の要求でアクセスできるデータ blob のみです。 コンテナー Acl の詳細については、次を参照してください。 [Set container ACL (REST API)][container-acl]します。

Blob サービスのエラー コードの詳細については、次を参照してください。 [Blob サービスのエラー コード][error-codes]します。

## コンテナーに BLOB をアップロードする

Blob としてファイルをアップロードするには、使用、 **BlobRestProxy createBlockBlob]-> [** メソッドです。 この処理により、BLOB が存在しない場合は作成され、存在する場合は上書きされます。 次のコード例では、コンテナーが既に作成されていてを使用するいると想定しています [fopen][fopen] をストリームとしてファイルを開きます。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

前の例では、ストリームとして BLOB をアップロードしていることに注意してください。 ただし、blob できるアップロードすることも、文字列を使用して、たとえば、 [file \_get\_contents][file_get_contents] 関数です。 前のサンプルを使用してこれを行うには `$content = fopen("c:\myfile.txt", "r");` を `$content = file_get_contents("c:\myfile.txt");` に変更します。

## コンテナー内の BLOB を一覧表示する

コンテナー内の blob の一覧を表示する、 **BlobRestProxy listBlobs]-> [** メソッドを **foreach** ループ結果をループ処理します。 次のコードでは、コンテナー内の各 BLOB の名前を出力として表示し、ブラウザーへの URI を表示します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## BLOB をダウンロードする

Blob をダウンロードするには、呼び出し、 **BlobRestProxy getBlob]-> [** メソッドを呼び出して、 **getContentStream** メソッド **GetBlobResult** オブジェクトです。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

この例では、ストリーム リソースとして BLOB を取得していることに注意してください (既定の動作)。 ただし、使用することができます、 [stream \_get\_contents][stream-get-contents] 、返されたストリームを文字列に変換する関数。

## BLOB を削除する

Blob を削除するコンテナー名と blob 名に渡します **BlobRestProxy deleteBlob]-> [**します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## BLOB コンテナーを削除する

最後に、blob コンテナーを削除するにコンテナー名を渡す **BlobRestProxy deleteContainer]-> [**します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 次のステップ

これで、Azure BLOB サービスの基本を学習できました。さらに複雑なストレージ タスクについて学習するには、次のリンク先をご覧ください。

- 参照してください、 [Azure Storage チームのブログ](http://blogs.msdn.com/b/windowsazurestorage/)
- PHP ブロック blob の例を参照してください <https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php>します。
- PHP ページ blob の例を参照してください。 <https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php>
- [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)
 
詳細については、「関連項目、 [PHP デベロッパー センター](/develop/php/)します。


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents


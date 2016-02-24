<properties
    pageTitle="iOS から Azure BLOB ストレージを使用する方法 | Microsoft Azure"
    description="Azure BLOB ストレージを使用して、BLOB の内容をアップロード、ダウンロード、一覧表示、削除する方法について説明します。 サンプルは Objective-C で記述されています。"
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Objective-C"
    ms.topic="article"
    ms.date="10/07/2015"
    ms.author="micurd"/>

# iOS から BLOB ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概要

この記事では、Microsoft Azure BLOB ストレージを使用して一般的なシナリオを実行する方法について説明します。 サンプルは OBJECTIVE-C と使用の記述、 [Azure Storage iOS ライブラリ](https://github.com/Azure/azure-storage-ios)します。 紹介するシナリオ **アップロード**, 、**を一覧表示する**, 、**ダウンロード**, 、および **を削除する** blob です。 Blob の詳細については、次を参照してください。、 [次のステップ](#next-steps) セクションです。 ダウンロードすることも、 [サンプル アプリ](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) で iOS アプリケーションで Azure ストレージの使用をすばやく表示します。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## フレームワーク ファイルを作成する
Azure Storage iOS ライブラリを使用するには、最初にフレームワーク ファイルを作成する必要があります。

1. 最初に、ダウンロードまたは複製、 [azure ストレージ-ios リポジトリ](https://github.com/azure/azure-storage-ios)します。

2. 移動 *azure ストレージ-ios* ]-> [ *Lib* ]-> [ *Azure ストレージ クライアント ライブラリ*, を再度開き、 `Azure Storage Client Library.xcodeproj` Xcode でします。

3. Xcode の左上部分で、アクティブ スキームを "Azure Storage Client Library" から "Framework" に変更します。

4. プロジェクトをビルド (⌘ + B キー) します。 これによって、`Azure Storage Client Library.framework` ファイルがデスクトップ上に作成されます。

## Azure Storage iOS ライブラリをアプリケーションにインポートする

次の手順を実行すると、Azure Storage iOS ライブラリをアプリケーションにインポートできます。

1. Xcode で新しいプロジェクトを作成するか、既存のプロジェクトを開きます。

2. 左側のナビゲーションで、プロジェクトをクリックし、 *全般* プロジェクト エディターの上部にあるタブをクリックします。

3. 下にある、 *リンク フレームワークとライブラリ* セクションで、追加ボタン (+) をクリックします。

4. クリックして *]、[その他*します。 作成した `Azure Storage Client Library.framework` ファイルに移動して、それを追加します。

5. 下にある、 *リンク フレームワークとライブラリ* セクションで、[追加] ボタン (+) をもう一度クリックします。

6. 既に表示されているライブラリの一覧で `libxml2.2.dylib` を検索し、それをプロジェクトに追加します。

##import ステートメント
ファイル内の Azure Storage API を呼び出す場所に、次の import ステートメントを加える必要があります。

    // Include the following import statement to use blob APIs.
    #import <Azure Storage Client Library/Azure_Storage_Client_Library.h>

## BLOB ストレージにアクセスするようにアプリケーションを構成する

Storage サービスにアクセスできるようにアプリケーションを認証するには、次の 2 つの方法があります。

- 共有キー: テスト目的のみに共有キーを使用します
- Shared Access Signature (SAS): 運用アプリケーション用に SAS を使用します

###共有キー
共有キー認証の場合、アプリケーションは Storage サービスへのアクセスにアカウント名とアカウント キーを使用します。 iOS から BLOB ストレージを使用する方法を簡単に説明するため、ここでは共有キー認証を使用します。

> [AZURE です。警告 (のみ使用して共有キー認証テスト目的で!)]アカウント名とアカウント キーに関連付けられたストレージ アカウントに完全な読み取り/書き込みアクセスできるように、これは、アプリをダウンロードするすべての人に配布されます。 これは、 **いない** なように、キーを信頼されていないクライアントが侵害されるリスクを練習します。

共有キー認証を使用する場合は、接続文字列を作成します。 接続文字列の構成要素は次のとおりです。  

-  **DefaultEndpointsProtocol** -HTTP または HTTPS を選択できます。 HTTPS の使用を強くお勧めします。
-  **アカウント名** -ストレージ アカウントの名前
-  **アカウント キー** - を使用している場合、 [Azure ポータル](portal.azure.com), ] をクリックし、ストレージ アカウントに移動、 **キー** アイコン情報を確認します。 使用する場合、 [Azure Classic Portal](manage.windowsazure.com), 、ポータルでストレージ アカウントに移動し、クリックして、 **アクセス キーの管理**します。 

アプリケーションでは次のようになります。

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=account_name;AccountKey=account_key"];

###Shared Access Signatures (SAS)
iOS アプリケーションの場合、BLOB ストレージに対するクライアントからの要求を認証する際には、Shared Access Signature (SAS) を使用することをお勧めします。 SAS を使用すると、期間およびアクセス許可セットを指定して、リソースへのクライアント アクセスを許可することができます。
ストレージ アカウント所有者は、iOS クライアントが使用する SAS を生成する必要があります。 SAS を生成するには、クライアントに配布する SAS を生成する別のサービスを記述することが必要になります。 テスト用には、Azure CLI を使用して SAS を生成することもできます。 SAS の生成には共有キーの資格情報が使用されますが、生成後、クライアントは、SAS URL にカプセル化された認証情報によって SAS を使用できます。
SAS の作成時には、SAS の有効期間と、SAS がクライアントに付与するアクセス許可を指定できます。 たとえば、BLOB コンテナーの場合、SAS はコンテナー内の BLOB に対する読み取り、書き込み、削除のアクセス許可と、コンテナー内の BLOB を一覧に表示するための一覧表示アクセス許可を付与することができます。

次の例は、Azure CLI を使用している付与読み書き可能なコンテナーのアクセス許可は、SAS トークンを生成する方法を示しています。*sascontainer*, 、午前 12:00 (UTC)、2015 年 9 月 5 日までです。  

1. 最初に、この後に [ガイド](../xplat-cli/#how-to-install-the-azure-cli)  に Azure CLI をインストールし、Azure サブスクリプションに接続する方法について説明します。

2. 次に、Azure CLI で次のコマンドを入力し、アカウントの接続文字列を取得します。

        azure storage account connectionstring show youraccountname

3. 生成した接続文字列で環境変数を作成します。

        export AZURE_STORAGE_CONNECTION_STRING=”your connection string”

4. SAS URL を生成します。

        azure storage container sas create --container sascontainer --permissions rw --expiry 2015-09-05T00:00:00

5. SAS URL は次のようになります。

        https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-29T22%3A18%3A26Z&se=2013-04-30T02%3A23%3A26Z&sr=b&sp=rw&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D

6. iOS アプリケーションでは、SAS URL を次のように使用して、コンテナーへの参照を取得できます。

        // Get a reference to a container in your Storage account
        AZSCloudBlobContainer *blobContainer = [[AZSCloudBlobContainer alloc] initWithUrl:[NSURL URLWithString:@" your SAS URL"]];

ご覧のように、SAS トークンを使用する場合、iOS アプリケーションでアカウント名とアカウント キーを公開することはありません。 SAS についてを説明するにはチェック アウトして、 [共有アクセス署名のチュートリアル](../storage-dotnet-shared-access-signature-part-1)します。

##非同期操作
> [AZURE.NOTE] サービスに対する要求を実行するすべてのメソッドは、非同期操作です。 コード サンプルでは、このようなメソッドに完了ハンドラーが含まれていることがわかります。 完了ハンドラー内でコードが実行 **後** 要求を完了します。 コードの完了ハンドラーが実行後 **中** 要求になっています。

## コンテナーを作成する
Azure Storage のどの BLOB もコンテナーに格納する必要があります。 次の例は、という名前のコンテナーを作成する方法を示しています。 *新しいコンテナー*, 、存在しない場合は、ストレージ アカウントにします。 コンテナーの名前を選択する場合は、上記の名前付け規則に注意してください。

     -(void)createContainer{
        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

見るに動作することを確認できます、 [Azure ポータル](portal.azure.com) または [記憶域エクスプ ローラー](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) ことを確認 *新しいコンテナー* がストレージ アカウントのコンテナーの一覧にします。

## コンテナーのアクセス許可を設定する
コンテナーのアクセス許可が設定されて **プライベート** 既定でアクセスします。 ただし、コンテナーには、コンテナー アクセス用にいくつかの異なるオプションが用意されています。

- **プライベート**: アカウントの所有者だけで、コンテナーと blob データを読み取ることができます。

- **Blob**: このコンテナー内の Blob データは匿名要求で読み取ることができますが、コンテナー データは使用できません。 クライアントはコンテナー内の BLOB を匿名要求で列挙することはできません。

- **コンテナー**: 匿名要求でコンテナーと blob データを読み取ることができます。 クライアントは匿名要求でコンテナー内の BLOB を列挙できますが、ストレージ アカウント内のコンテナーを列挙することはできません。

次の例を使用して、コンテナーを作成する方法を示します **コンテナー** 、インターネット上のすべてのユーザーに対して、パブリックの読み取り専用アクセスを許可するアクセス許可にアクセスします。

     -(void)createContainerWithPublicAccess{
        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## コンテナーに BLOB をアップロードする
説明したように、 [Blob サービスの概念](#blob-service-concepts) ] セクションで、Blob ストレージには、3 種類の blob が提供しています。 ブロック blob、追加 blob、およびページ blob です。 現在、Azure Storage iOS ライブラリでは、ブロック BLOB のみがサポートされています。 ほとんどの場合は、ブロック BLOB を使用することをお勧めします。

次の例では、NSString からブロック BLOB をアップロードする方法を示します。 同じ名前の BLOB が既にこのコンテナーに存在する場合は、この BLOB の内容が上書きされます。

     -(void)uploadBlobToContainer{
        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }             
                 }];
             }
         }];
     }

調べるにはこれを確認することができます、 [Azure ポータル](portal.azure.com) または任意 [記憶域エクスプ ローラー](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) ことを確認し、コンテナー *containerpublic*, 、blob を格納 *sampleblob*します。 この例ではパブリック コンテナーを使用したため、次の BLOB URI にアクセスすることによっても、これを確認できます。

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

NSString からブロック BLOB をアップロードする場合だけでなく、NSData、NSInputStream、またはローカル ファイルの場合にも同様のメソッドが存在します。

## コンテナー内の BLOB を一覧表示する
次の例では、コンテナー内のすべての BLOB を一覧表示する方法を示します。 この操作を実行する場合は、次のパラメーターに注意してください。     

- **continuationToken** -位置一覧作成操作を開始する継続トークンを表します。 トークンが指定されていない場合、最初から BLOB を一覧表示します。 0 から設定された最大値まで、BLOB はいくつでも一覧表示できます。 このメソッドによって返される結果が 0 件でも、`results.continuationToken` が nil でない場合は、一覧表示されていない BLOB がもっとサービス上に存在する可能性があります。
- **プレフィックス** -blob リストを使用するプレフィックスを指定できます。 このプレフィックスで始まる名前の BLOB のみが一覧表示されます。
- **useFlatBlobListing** で説明したよう -、 [名前付けとコンテナーと blob を参照する](#naming-and-referencing-containers-and-blobs) ] セクションで、Blob サービスは、フラット ストレージ スキームできる階層を作成する仮想パス情報を使用した blob の名前を付けています。 ただし、フラットでない一覧表示は現在サポート対象外であり、近日対応予定です。 現時点では、この値は `YES` にする必要があります
- **blobListingDetails** -blob を一覧表示するときに含めるアイテムを指定することができます
    - `AZSBlobListingDetailsNone`: コミット済みの BLOB のみを一覧表示し、BLOB メタデータは返しません。
    - `AZSBlobListingDetailsSnapshots`: コミット済みの BLOB と BLOB スナップショットを一覧表示します。
    - `AZSBlobListingDetailsMetadata`: 一覧に返された BLOB ごとに BLOB メタデータを取得します。
    - `AZSBlobListingDetailsUncommittedBlobs`: コミット済みおよびコミット前の BLOB を一覧表示します。
    - `AZSBlobListingDetailsCopy`: コピー プロパティを一覧に含めます。
    - `AZSBlobListingDetailsAll`: 使用可能なコミット済みの BLOB、コミット前の BLOB、スナップショットをすべて一覧表示し、それらの BLOB に関するすべてのメタデータとコピーの状態を返します。
- **maxResults** -この操作で返される結果の最大数。 制限を設けない場合は -1 を使用します。
- **completionHandler** - ブロックの一覧作成操作の結果でコードを実行します。

この例では、継続トークンが返されるたびに BLOB の一覧表示メソッドを再帰的に呼び出すために、ヘルパー メソッドが使用されています。

    -(void)listBlobsInContainer{
      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      //List all blobs in container
      [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
         if (error != nil){
             NSLog(@"Error in creating container.");
         }
      }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
      [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
         if (error)
         {
             completionHandler(error);
         }
         else
         {
             for (int i = 0; i < results.blobs.count; i++) {
                 NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
             }
             if (results.continuationToken)
             {
                 [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
             }
             else
             {
                 completionHandler(nil);
             }
         }
      }];
    }

## BLOB をダウンロードする

次の例では、BLOB を NSString オブジェクトにダウンロードする方法を示します。

     -(void)downloadBlobToString{
        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob    
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## BLOB を削除する

次の例では、BLOB を削除する方法を示します。

     -(void)deleteBlob{
        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
          if (error) {
            NSLog(@"Error in deleting blob.");
          }
        }];
     }

## BLOB コンテナーを削除する

次の例では、コンテナーを削除する方法を示します。

     -(void)deleteContainer{
        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"];

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Delete container
        [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
            if(error){
                NSLog(@"Error in deleting container");
            }
        }];
    }

## 次のステップ

これで、BLOB ストレージの基本を学習できました。さらに複雑なストレージ タスクを実行するには、次のリンク先をご覧ください。

- [Azure Storage iOS ライブラリ]
- [Azure Storage REST API]
- [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)
- [Azure のストレージ チーム ブログ]

このライブラリについて質問がある場合は、気軽に投稿する、 [MSDN Azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) または [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files)します。
Azure ストレージの機能の候補がある場合に投稿してください [Azure ストレージからのフィードバック](http://feedback.azure.com/forums/217298-storage)します。

[Azure Storage iOS Library]: https://github.com/azure/azure-storage-ios
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage


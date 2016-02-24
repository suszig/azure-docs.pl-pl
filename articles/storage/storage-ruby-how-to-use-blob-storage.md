<properties
    pageTitle="Ruby から BLOB ストレージを使用する方法 | Microsoft Azure"
    description="Azure BLOB サービスを使用して、BLOB の内容をアップロード、ダウンロード、一覧表示、および削除する方法について説明します。 コード サンプルは Ruby で記述されています。"
    services="storage"
    documentationCenter="ruby"
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="tomfitz"/>


# Ruby から BLOB ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概要

このガイドでは、Azure BLOB サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは Ruby API を使用して記述されています。 紹介するシナリオ **アップロード、一覧表示、ダウンロード、** と **削除** blob です。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Ruby アプリケーションの作成

Ruby アプリケーションを作成します。 手順については、
参照してください [Azure で Ruby アプリケーションの作成](/develop/ruby/tutorials/web-app-with-linux-vm/)します。

## アプリケーションのストレージへのアクセスの構成

Azure Storage を使用するには、Ruby azure パッケージをダウンロードして使用する必要があります。このパッケージには、ストレージ REST サービスと通信するための便利なライブラリのセットが含まれています。

### RubyGems を使用してパッケージを取得する

1. コマンド ライン インターフェイスを使用して **PowerShell** (Windows)、 **ターミナル** (Mac)、または **Bash** (Unix)。

2. コマンド ウィンドウに「gem install azure」と入力して、gem と依存関係をインストールします。

### パッケージをインポートする

任意のテキスト エディターを使用して、ストレージを使用するアプリケーションの Ruby ファイルの先頭に次のコードを追加します。

    require "azure"

## Azure のストレージ接続文字列の設定

Azure モジュールでは、環境変数を読み取ります **azure \_storage\_account** と **azure \_storage\_access_key**
を読み取ります。 これらの環境変数が設定されていない場合は、使用する前にアカウント情報を指定する必要があります **Azure::Blob::BlobService** を次のコード。

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


これらの値を取得するには、次の手順を実行します。

1. ログイン、 [Azure ポータル](portal.azure.com)します。
2. 使用するストレージ アカウントを表示します。
3. クリックして **キーの管理** ナビゲーション ウィンドウの下部にあります。
4. ポップアップ ダイアログに、ストレージ アカウント名、プライマリ アクセス キー、およびセカンダリ アクセス キーが表示されます。 アクセス キーには、プライマリとセカンダリのどちらでも使用できます。

## コンテナーを作成する

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

 **Azure::Blob::BlobService** オブジェクトでは、コンテナーおよび blob を操作することができます。 コンテナーを作成するには、使用、 **create \_container ()** メソッドです。

次のコード例では、コンテナーを作成し、既に存在している場合はエラーを出力します。

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

コンテナー内のファイルを公開する場合は、コンテナーのアクセス許可を設定できます。

変更、 <strong>create \_container ()</strong> 呼び出しに渡す、 **: \_access\_level** オプション。

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


有効な値、 **: \_access\_level** オプションします。

* **blob:** コンテナーと blob データに対する完全パブリック読み取りアクセスを指定します。 クライアントは匿名要求でコンテナー内の BLOB を列挙できますが、ストレージ アカウント内のコンテナーを列挙することはできません。

* **コンテナー:** blob に対するパブリック読み取りアクセスを指定します。 該当するコンテナー内の BLOB データは匿名要求で読み取り可能ですが、コンテナー データは参照できません。 クライアントはコンテナー内の BLOB を匿名要求で列挙することはできません。

使用して、コンテナーのパブリック アクセス レベルを変更する代わりに、 **\_container\_acl** パブリック アクセス レベルを指定します。

次のコード例にパブリック アクセス レベルを変更する **コンテナー**:

    azure_blob_service.set_container_acl('test-container', "container")

## コンテナーに BLOB をアップロードする

Blob にコンテンツをアップロードするには、使用、 **create \_block\_blob ()** 、blob を作成するメソッドでは、blob の内容として、ファイルまたは文字列が使用されます。

次のコード ファイルをアップロードする **test.png** として、新しい blob をコンテナーに"image-blob"という名前です。

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## コンテナー内の BLOB を一覧表示する

コンテナーの一覧を表示する **list_containers()** メソッドです。
コンテナー内の blob の一覧を表示する **list \_blobs ()** メソッドです。

次のコードでは、アカウントのすべてのコンテナーからすべての BLOB の URL を出力します。

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## BLOB をダウンロードする

Blob をダウンロードするには、使用、 **get \_blob ()** 内容を取得します。

次のコード例では、使用方法を示します **get \_blob ()** "image-blob"の内容をダウンロードしてローカル ファイルに記述します。

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## BLOB を削除する
最後に、blob を削除するには、 **\_blob ()** メソッドです。 次のコード例は、BLOB の削除方法を示しています。

    azure_blob_service.delete_blob(container.name, "image-blob")

## 次のステップ

さらに複雑なストレージ タスクの詳細については、次のリンク先をご覧ください。

- [Azure のストレージ チーム ブログ](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub のリポジトリ
- [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)

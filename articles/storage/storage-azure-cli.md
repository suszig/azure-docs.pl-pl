<properties
    pageTitle="Azure Storage での Azure CLI の使用 | Microsoft Azure"
    description="Azure Storage で Azure コマンド ライン インターフェイス (Azure CLI) を使用して、ストレージ アカウントの作成と管理および Azure の BLOB やファイルの操作を行う方法について説明します。"
    services="storage"
    documentationCenter="na"
    authors="tamram"
    manager="jdial"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/28/2015"
    ms.author="chungli;jiyang;yaxia;tamram"/>

# Azure Storage での Azure CLI の使用

## 概要

Azure CLI は、Azure Platform で使用できるオープン ソース、クロスプラットフォームのコマンド群です。 については、同じ機能の多くを提供、 [Azure ポータル](portal.azure.com) も、豊富なデータの機能にアクセスします。

このガイドでは、使用する方法を見ていきます [Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-install.md) さまざまな Azure Storage での開発と管理のタスクを実行します。 このガイドを使用する前に、最新の Azure CLI をダウンロードしてインストールするか、最新の Azure CLI にアップグレードすることをお勧めします。

このガイドでは、Azure Storage の基本概念を理解していることを前提としています。 また、Azure CLI と Azure Storage を使用する方法を示すための多くのスクリプトを用意しています。 各スクリプトの実行前に、使用する構成に基づいてスクリプト変数を更新してください。

> [AZURE.NOTE] このガイドでは、(ASM) の Azure サービス管理モードで実行されている Azure CLI コマンドとスクリプト例を示します。 参照してください [Mac、Linux、および Azure リソース管理で Windows 用 Azure CLI の使用](../azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Azure リソース管理 (ARM) モードでストレージ用の Azure CLI コマンドにします。

## 5 分で始める Azure Storage と Azure CLI の使用

このガイドの例では Ubuntu を使用しますが、他の OS プラットフォームでも同様に動作します。

**Azure に新しい:** Microsoft Azure サブスクリプションとそのサブスクリプションに関連付けられている Microsoft アカウントを入手します。 Azure の購入オプションについては、次を参照してください。 [無料評価版](http://azure.microsoft.com/pricing/free-trial/), 、[購入オプション](http://azure.microsoft.com/pricing/purchase-options/), 、および [メンバー プラン](http://azure.microsoft.com/pricing/member-offers/) (MSDN、Microsoft Partner Network、BizSpark、および他の Microsoft プログラムのメンバー) 用です。

参照してください [アカウントの管理、サブスクリプション、および管理者の役割](https://msdn.microsoft.com/library/azure/hh531793.aspx) 詳細については、Azure サブスクリプション。

**Microsoft Azure アカウントとサブスクリプションを作成済みである場合:**

1. ダウンロードし、インストールの手順に従って、Azure CLI [Azure CLI をインストール](../xplat-cli-install.md)します。
2. Azure CLI をインストールすると、コマンド ライン インターフェイス (Bash、Terminal、Command プロンプト) から azure コマンドを使用して Azure CLI コマンドにアクセスできるようになります。 `azure` コマンドを入力すると、次の出力が表示されます。

    ![Azure Command Output][Image1]

3. コマンド ライン インターフェイスで「`azure storage`」と入力すると、Azure Storage のすべてのコマンドの一覧が表示され、Azure CLI に備わった機能の全体像が得られます。 コマンド名を入力する **-h** パラメーター (たとえば、 `azure storage share create -h`) コマンドの構文の詳細を表示します。
4. ここでは、Azure Storage にアクセスするための基本的な Azure CLI コマンドを示すシンプルなスクリプトを取り上げます。 このスクリプトでは、まずストレージ アカウントとキーの 2 つの変数を設定するよう求められます。 次に、この新しいストレージ アカウントで新しいコンテナーが作成され、既存の画像ファイル (BLOB) がこのコンテナーにアップロードされます。 このスクリプトにより、コンテナー内のすべての BLOB が一覧表示されると、ローカル コンピューターにある格納先ディレクトリに画像ファイルがダウンロードされます。

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. ローカル コンピューターで、任意のテキスト エディター (vim など) を開きます。 上記のスクリプトをテキスト エディターに入力します。

6. ここで、構成設定に基づいてスクリプト変数を更新する必要があります。

    - **<:Storage_account_name >** スクリプトの指定された名前を使用するか、ストレージ アカウントの新しい名前を入力します。 **重要:** ストレージ アカウントの名前は、Azure 内で一意である必要があります。 また、小文字にする必要もあります。

    - **< Storage_account_key >** ストレージ アカウントのアクセス キー。

    - **< コンテナー名 >** スクリプトの指定された名前を使用するか、コンテナーの新しい名前を入力します。

    - **< Image_to_upload >** 、ローカル コンピューターでは画像にパスをなど入力:"~/images/HelloWorld.png"です。

    - **< Destination_folder >** など、Azure Storage からダウンロードしたファイルを格納するローカル ディレクトリへのパスを入力してください:"~/downloadImages"です。

7. Vim で必要な変数を更新した後は、キーの組み合わせを押します"esc キー、: wq!" スクリプトを保存します。

8. このスクリプトを実行するには、bash コンソールでスクリプト ファイル名を入力するだけです。 このスクリプトを実行すると、ダウンロードした画像ファイルを格納するローカルの格納先フォルダーの準備が整います。 次のスクリーンショットは、この出力の例を示しています。

スクリプトを実行すると、ダウンロードした画像ファイルを格納するローカルの格納先フォルダーの準備が整います。

## Azure CLI を使用してストレージ アカウントを管理する

### Azure サブスクリプションへの接続

ほとんどのストレージ コマンドは、Azure サブスクリプションがなくても動作しますが、Azure CLI からサブスクリプションに接続することをお勧めします。 Azure CLI がサブスクリプションで動作するを構成するには、手順に従います [、Azure サブスクリプションに接続する方法](../xplat-cli-install.md#how-to-connect-to-your-azure-subscription)します。

### 新しいストレージ アカウントの作成

Azure Storage を使用するには、ストレージ アカウントが必要です。 コンピューターを構成してサブスクリプションに接続できるようにすると、新しい Azure ストレージ アカウントを作成できます。

        azure storage account create <account_name>

ストレージ アカウント名の長さは 3 ～ 24 文字で、数字と小文字のみを使用する必要があります。

### 環境変数で既定の Azure ストレージ アカウントを設定する

サブスクリプションで複数のストレージ アカウントを持つことができます。 その中から 1 つを選択し、同じセッションのすべてのストレージ コマンドに対する環境変数にそれを設定できます。 そうすることにより、ストレージ アカウントとキーを明示的に指定しなくても、Azure CLI のストレージ コマンドを実行できます。

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

既定のストレージ アカウントを設定するもう 1 つの方法として、接続文字列を使用します。 まず、次のコマンドで接続文字列を取得します。

        azure storage account connectionstring show <account_name>

出力された接続文字列をコピーし、環境変数に設定します。

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## BLOB を作成および管理する

Azure BLOB ストレージは、HTTP または HTTPS 経由で世界中のどこからでもアクセスできるテキストやバイナリ データなど、大量の非構造化データを格納するためのサービスです。 このセクションでは、Azure BLOB ストレージの概念について理解しているユーザーを対象としています。 詳細については、次を参照してください。 [.NET から Blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md) と [Blob サービスの概念](http://msdn.microsoft.com/library/azure/dd179376.aspx)します。

### コンテナーを作成する

Azure Storage のすべての BLOB はコンテナーに格納する必要があります。 次の `azure storage container create` コマンドを使用して、プライベート コンテナーを作成できます。

        azure storage container create mycontainer

> [AZURE.NOTE] 次の 3 つのレベルの匿名読み取りアクセスがある: **オフ**, 、**Blob**, 、および **コンテナー**します。 Blob への匿名アクセスを防ぐために、Permission パラメーターを設定します。 **オフ**します。 既定では、新しいコンテナーはプライベートであり、アカウント所有者のみがアクセスできます。 匿名パブリック読み取りアクセス権 blob リソースに、コンテナーのメタデータに設定したり、コンテナー内の blob の一覧 Permission パラメーターを設定 **Blob**します。 Blob リソース、コンテナー メタデータ、コンテナー内の blob の一覧に完全パブリック読み取りアクセスを許可するように、Permission パラメーターを設定 **コンテナー**します。 詳細については、次を参照してください。 [Azure ストレージ リソースへのアクセスの管理](storage-manage-access-to-resources.md)します。

### コンテナーに BLOB をアップロードする

Azure Blob Storage では、ブロック BLOB とページ BLOB がサポートされています。 詳細については、次を参照してください。 [ブロック Blob およびページ Blob について](http://msdn.microsoft.com/library/azure/ee691964.aspx)します。

BLOB をコンテナーにアップロードするには、`azure storage blob upload` を使用できます。 既定では、このコマンドにより、ローカル ファイルがブロック BLOB にアップロードされます。 BLOB の種類を指定するには、`--blobtype` パラメーターを使用できます。

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### コンテナーから BLOB をダウンロードする

次の例は、BLOB をコンテナーからダウンロードする方法を示しています。

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### BLOB をコピーする

BLOB は、ストレージ アカウント内、またはストレージ アカウントとリージョン間にまたがって非同期的にコピーできます。

次の例は、BLOB をあるストレージ アカウントから別のストレージ アカウントにコピーする方法を示しています。 この例では、匿名でパブリックに BLOB にアクセスできるコンテナーを作成します。

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

この例で実行するのは非同期コピーです。 各コピー操作の状態は、`azure storage blob copy show` 操作を実行して監視できます。

コピー操作で指定されたソース URL は、パブリックにアクセスできるようにするか、SAS (Shared Access Signature) トークンを含める必要があります。

### BLOB を削除する

BLOB を削除するには、次のコマンドを使用します。

        azure storage blob delete mycontainer myBlockBlob2

## ファイル共有を作成および管理する

Azure File ストレージは、標準的な SMB プロトコルを使用して、アプリケーション用の共有ストレージを提供します。 Microsoft Azure の仮想マシンとクラウド サービスでは、オンプレミスのアプリケーションと同じように、ファイル データを共有できます。 ファイル共有とファイル データは、Azure CLI を使用して管理できます。 Azure File ストレージの詳細については、次を参照してください。 [Windows で Azure File ストレージを使用する方法](storage-dotnet-how-to-use-files) または [Linux で Azure File ストレージを使用する方法](storage-how-to-use-files-linux.md)します。

### ファイル共有を作成する

Azure File 共有は、Azure 内の SMB ファイル共有です。 ディレクトリとファイルはすべて、ファイル共有に作成する必要があります。 アカウントに含まれる共有の数と、共有に格納できるファイル数には制限がなく、ストレージ アカウントの容量の上限まで増やすことができます。 次の例では、という名前のファイル共有 **myshare**します。

        azure storage share create myshare

### ディレクトリを作成する

ディレクトリは、Azure ファイル共有の任意の階層構造を示します。 次の例では、という名前のディレクトリ **myDir** ファイル共有にします。

        azure storage directory create myshare myDir

ディレクトリ パスには、複数のレベルを含めることができます *例:*, 、**、/b**します。 ただし、すべての親ディレクトリが存在することを確認する必要があります。 たとえば、パス **、/b**, 、ディレクトリを作成する必要があります **、** 最初に、ディレクトリを作成し、 **b**します。

### ディレクトリにローカル ファイルをアップロードする

次の例は、ファイルからアップロード **~/temp/samplefile.txt** に、 **myDir** ディレクトリ。 ファイル パスを編集して、ローカル マシン上の有効なファイルを指定してください。

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

共有内のファイル サイズは最大 1 TB です。

### 共有ルートまたはディレクトリ内のファイルの一覧を表示する

次のコマンドを使用して、共有ルートまたはディレクトリ内のサブディレクトリとファイルの一覧を表示できます。

        azure storage file list myshare myDir

一覧表示操作では、ディレクトリ名を省略できます。 省略した場合は、共有のルート ディレクトリの内容が一覧表示されます。

### ファイルのコピー

Azure CLI のバージョン 0.9.8 以降では、ファイルを別のファイルにコピーしたり、ファイルを BLOB にコピーしたり、BLOB をファイルにコピーしたりすることができます。 次に、CLI コマンドを使用してこれらのコピー操作を実行する方法を示します。 ファイルを新しいディレクトリにコピーするには、次の操作を実行します。

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
    
BLOB をファイル ディレクトリにコピーするには、次の操作を実行します。

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## 次のステップ

Azure Storage の詳細についての関連記事とリソースがあります。

- [Azure ストレージのドキュメント](http://azure.microsoft.com/documentation/services/storage/)
- [Azure Storage REST API リファレンス](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
 

